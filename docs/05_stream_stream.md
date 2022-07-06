# 流式服务与调用

## 流式语音识别

### 流式ASR服务

命令行和Python脚本，选择一种开启服务即可

#### 命令行启动
```bash
paddlespeech_server start --config_file ./demos/streaming_asr_server/conf/application.yaml
```

#### python 启动

```Python
from paddlespeech.server.bin.paddlespeech_server import ServerExecutor

server_executor = ServerExecutor()
server_executor(
    config_file="./demos/streaming_asr_server/conf/application.yaml", 
    log_file="./log/paddlespeech.log")
```

### 流式ASR客户端

#### 流式识别音频文件

#### 命令行启动

```bash
paddlespeech_client asr_online --server_ip 127.0.0.1 --port 8090 --input input_16k.wav
```

#### 流式识别python客户端

```Python
from paddlespeech.server.bin.paddlespeech_client import ASROnlineClientExecutor

asrclient_executor = ASROnlineClientExecutor()
res = asrclient_executor(
    input="./zh.wav",
    server_ip="127.0.0.1",
    port=8090,
    sample_rate=16000,
    lang="zh_cn",
    audio_format="wav")
print(res)
```

#### 流式识别Web客户端

`谷歌游览器`打开`./demos/streaming_asr_server/web`下的`index.html`，点击连接接，开始录音即可,


## 流式语音合成

默认http协议，使用websocket协议可更换下面`http`为`websocket`

### 流式TTS服务启动

```bash
paddlespeech_server start --config_file ./demos/streaming_tts_server/conf/tts_online_application.yaml
```

### 流式TTS客户端

#### 命令行 - 流式合成音频文件

```bash
paddlespeech_client tts_online --server_ip 127.0.0.1 --port 8092 --protocol http --input "您好，欢迎使用百度飞桨语音合成服务。" --output output.wav
```

```bash
paddlespeech_client tts_online --server_ip 127.0.0.1 --port 8092 --protocol http --input "您好，欢迎使用百度飞桨语音合成服务。" --output output.wav --play true
```

#### python - 流式合成音频文件

```python
from paddlespeech.server.bin.paddlespeech_client import TTSOnlineClientExecutor

executor = TTSOnlineClientExecutor()
executor(
    input="您好，欢迎使用百度飞桨语音合成服务。",
    server_ip="127.0.0.1",
    port=8092,
    protocol="http",
    spk_id=0,
    speed=1.0,
    volume=1.0,
    sample_rate=0,
    output="./output.wav",
    play=False)
```

***如果你想流式播放这个音频，客户端支持声卡并安装pyaudio***

```python
executor(
    input="您好，欢迎使用百度飞桨语音合成服务。",
    server_ip="127.0.0.1",
    port=8092,
    protocol="http",
    spk_id=0,
    speed=1.0,
    volume=1.0,
    sample_rate=0,
    output="./output.wav",
    play=True)  # 播放时 play 参数改为 true
```



