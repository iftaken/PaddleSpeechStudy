# 声音分类-预训练模型【PANN14】 使用

你可以直接使用PaddleSpeech提供的预训练模型来完成自己场景中的声音分类任务，也可以使用预训练模型进行微调，在自己场景的数据集上完成相应的工作。

微调训练可以参考[【微调PANN模型，训练自己的分类模型】](./train_PANN.md)

aistudio代码体验，参考[【声音分类—预训练模型使用】](https://aistudio.baidu.com/aistudio/projectdetail/4308682?shared=1)

## 准备测试数据

下载示例音频

```bash
# 16k 16bit 测试音频
wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
```

## CLI调用

如果你只需要在自己的任务中简单调用，你可以使用CLI进行调用，可以选择命令行或者 Python API 两种方式。

cli 会自动下载预训练模型[panns_cnn14-32k](https://github.com/PaddlePaddle/PaddleSpeech/blob/develop/docs/source/released_model.md#audio-classification-models)，模型在Audioset数据集上训练得到

> [AudioSet](https://research.google.com/audioset/download.html)是Google发行的声音版ImageNet，包含5000h小时的音频以及527个标签类别。

### Python

```python
from paddlespeech.cli.cls.infer import CLSExecutor
cls = CLSExecutor()
result = cls(audio_file="zh.wav")
```

### 命令行
```bash
paddlespeech cls --input zh.wav
```

样例输出结果
``` text
Speech 0.9027187824249268
```

## 自定义模型加载

```bash
# 下载预训练模型

```


```python

```




