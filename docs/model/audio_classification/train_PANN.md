# 微调PANN模型，训练自己的分类模型

[本教程aistudio代码演示](https://aistudio.baidu.com/aistudio/projectdetail/4293771?shared=1) 纯代码版本

[【声音分类课程介绍】](https://aistudio.baidu.com/aistudio/projectdetail/4042421)

[esc50模型训全流程](https://github.com/PaddlePaddle/PaddleSpeech/tree/develop/examples/esc50)

## 整理流程

思路：使用基于AudioSet数据集的PANNs模型作为预训练模型，提取音频embeeding，添加下游声音分类任务微调

微调部分使用ESC50数据集

+ 数据准备
+ 模型组网
+ 模型训练
+ 静态图导出

## 数据处理

数据准备： [【ESC-50】](https://github.com/karoldvl/ESC-50/archive/master.zip)

制作数据集： ESC50 数据集已经在PaddleSpeech中封装完成，会自动下载百度云中ESC50数据，根据这个数据集进行数据划分

自定义数据集制作过程，可以参考源码中 `ESC50` 这个类


### 划分数据集

```python
from paddleaudio.datasets import ESC50

train_ds = ESC50(mode='train', sample_rate=sr)
dev_ds = ESC50(mode='dev', sample_rate=sr)

```

### 定义特征提取

```python
from paddleaudio.features import LogMelSpectrogram
feature_extractor = LogMelSpectrogram(
    sr=sr, 
    n_fft=n_fft, 
    hop_length=hop_length, 
    win_length=win_length, 
    window='hann', 
    f_min=f_min,
    f_max=f_max,
    n_mels=64
    )
```

## 模型组网

```python
import paddle.nn as nn
from paddlespeech.cls.models import cnn14

backbone = cnn14(pretrained=True, extract_embedding=True)

class SoundClassifier(nn.Layer):

    def __init__(self, backbone, num_class, dropout=0.1):
        super().__init__()
        self.backbone = backbone
        self.dropout = nn.Dropout(dropout)
        self.fc = nn.Linear(self.backbone.emb_size, num_class)

    def forward(self, x):
        x = x.unsqueeze(1)
        x = self.backbone(x)
        x = self.dropout(x)
        logits = self.fc(x)

        return logits

model = SoundClassifier(backbone, num_class=len(ESC50.label_list))
```


## 模型训练

定义参数，测试机器v100， 16G , 训练时间 12min左右

```python
import paddle

batch_size = 32
epochs = 10
log_freq = 10
eval_freq = 10

train_loader = paddle.io.DataLoader(train_ds, batch_size=batch_size, shuffle=True)
dev_loader = paddle.io.DataLoader(dev_ds, batch_size=batch_size,)

optimizer = paddle.optimizer.Adam(learning_rate=1e-4, parameters=model.parameters())
criterion = paddle.nn.loss.CrossEntropyLoss()

steps_per_epoch = len(train_loader)
```

开始训练

```python
from paddleaudio.utils import logger

for epoch in range(1, epochs + 1):
    model.train()

    avg_loss = 0
    num_corrects = 0
    num_samples = 0
    for batch_idx, batch in enumerate(train_loader):
        waveforms, labels = batch
        feats = feature_extractor(waveforms)
        feats = paddle.transpose(feats, [0, 2, 1])  # [B, N, T] -> [B, T, N]
        logits = model(feats)

        loss = criterion(logits, labels)
        loss.backward()
        optimizer.step()
        if isinstance(optimizer._learning_rate,
                      paddle.optimizer.lr.LRScheduler):
            optimizer._learning_rate.step()
        optimizer.clear_grad()

        # Calculate loss
        avg_loss += loss.numpy()[0]

        # Calculate metrics
        preds = paddle.argmax(logits, axis=1)
        num_corrects += (preds == labels).numpy().sum()
        num_samples += feats.shape[0]

        if (batch_idx + 1) % log_freq == 0:
            lr = optimizer.get_lr()
            avg_loss /= log_freq
            avg_acc = num_corrects / num_samples

            print_msg = 'Epoch={}/{}, Step={}/{}'.format(
                epoch, epochs, batch_idx + 1, steps_per_epoch)
            print_msg += ' loss={:.4f}'.format(avg_loss)
            print_msg += ' acc={:.4f}'.format(avg_acc)
            print_msg += ' lr={:.6f}'.format(lr)
            logger.train(print_msg)

            avg_loss = 0
            num_corrects = 0
            num_samples = 0

    if epoch % eval_freq == 0 and batch_idx + 1 == steps_per_epoch:
        model.eval()
        num_corrects = 0
        num_samples = 0
        with logger.processing('Evaluation on validation dataset'):
            for batch_idx, batch in enumerate(dev_loader):
                waveforms, labels = batch
                feats = feature_extractor(waveforms)
                feats = paddle.transpose(feats, [0, 2, 1])
                
                logits = model(feats)

                preds = paddle.argmax(logits, axis=1)
                num_corrects += (preds == labels).numpy().sum()
                num_samples += feats.shape[0]

        print_msg = '[Evaluation result]'
        print_msg += ' dev_acc={:.4f}'.format(num_corrects / num_samples)

        logger.eval(print_msg)
# paddle模型保存
paddle.save(model.state_dict(), 'SoundClassifier.pdparams')
```

## 静态图导出

```python
# paddle 动转静
from paddle.static import InputSpec
state_dict = paddle.load("./SoundClassifier.pdparams")
model.set_state_dict(state_dict)
model.eval()
paddle.jit.save(
    layer=model,
    path="inference/SoundClassifier",
    input_spec=[InputSpec(shape=[1, None, 64], dtype='float32')])
```

使用Paddle2Onnx导出成onnx

```bash
paddle2onnx --model_dir inference \
            --model_filename SoundClassifier.pdmodel \
            --params_filename SoundClassifier.pdiparams \
            --save_file SoundClassifier.onnx \
            --enable_dev_version True
```

