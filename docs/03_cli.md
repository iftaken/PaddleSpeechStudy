# 命令行功能体验

> 第一次使用时会下载对应的预训练模型

## 测试数据 & NLTK数据
下载测试音频:
```bash
# 已经下载过的话可以跳过这步
# 下载nltk数据包，建议放在用户的主目录下
cd ~
cd wget -c https://paddlespeech.bj.bcebos.com/Parakeet/tools/nltk_data.tar.gz
tar zxvf nltk_data.tar.gz
# 16k 16bit 测试音频
wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
```

## 命令行体验

### 语音识别

第一次使用是下载基于 XX 

```
paddlespeech asr --lang zh --input zh.wav
```

样例输出结果

```text
我认为跑步最重要的就是给我带来了身体健康%
```

### 语音合成

第一次使用时会下载基于XXX

```bash
paddlespeech tts --input "你好，欢迎使用飞桨深度学习框架！" --output output.wav
```

样例输出结果:

`output.wav`, 24000采样率，16bit


### 声纹向量提取

第一次使用时会下载基于[VoxCeleb](https://www.robots.ox.ac.uk/~vgg/data/voxceleb/index.html#about)数据集的[ECAPA-TDNN模型](https://github.com/PaddlePaddle/PaddleSpeech/blob/develop/docs/source/released_model.md#speaker-verification-models)

```bash
paddlespeech vector --task spk --input zh.wav
```

样例输出结果：`1 x 192维`向量

```text
[ -0.19083723   9.474287   -14.122276    -2.0916417    0.04849081
   4.9295697    1.478006     0.37337714  10.695856     3.269716
  -4.4819946   -0.6617876   -9.170387   -11.156883    -1.2358153 ...]
```

### 声音分类

第一次使用时会默认下载 基于 ESC-50数据集训练的 [PANN 模型](https://github.com/PaddlePaddle/PaddleSpeech/blob/develop/docs/source/released_model.md#audio-classification-models)

```shell
paddlespeech cls --input zh.wav
```

样例输出结果
``` text
Speech 0.9027187824249268
```

Speech表示分类为人声， `0.9027187824249268`表示 `???`  类别详细信息可见[ESC-50](https://github.com/karolpiczak/ESC-50)

### 标点恢复

第一次使用时会下载基于

```bash
paddlespeech text --task punc --input 今天的天气真不错啊你下午有空吗我想约你一起去吃饭
```

样例输出结果

```text
今天的天气真不错啊！你下午有空吗？我想约你一起去吃饭。
```