# Python API 接口体验

aistudio上可完整体验，fork [【PaddleSpeech API Demo】](https://aistudio.baidu.com/aistudio/projectdetail/4281335?shared=1)，运行即可体验


## 测试数据 & NLTK数据

```shell
# 已经下载过的话可以跳过这步
# 下载nltk数据包，建议放在用户的主目录下
cd ~
wget -c https://paddlespeech.bj.bcebos.com/Parakeet/tools/nltk_data.tar.gz
tar zxvf nltk_data.tar.gz
# 下载测试数据
wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
```

## 语音识别

```python
from paddlespeech.cli.asr.infer import ASRExecutor
asr = ASRExecutor()
result = asr(audio_file="zh.wav")
```

## 语音合成

```python
from paddlespeech.cli.tts.infer import TTSExecutor
tts = TTSExecutor()
tts(text="今天天气十分不错。", output="output.wav")
```

## 声纹向量提取

```python
from paddlespeech.cli.vector import VectorExecutor
vec = VectorExecutor()
result = vec(audio_file="zh.wav")
```

## 声音分类

```
from paddlespeech.cli.cls.infer import CLSExecutor
cls = CLSExecutor()
result = cls(audio_file="zh.wav")
```


## 标点恢复

```python
from paddlespeech.cli.text.infer import TextExecutor
text_punc = TextExecutor()
result = text_punc(text="今天的天气真不错啊你下午有空吗我想约你一起去吃饭")
```

