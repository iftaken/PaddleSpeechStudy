# Server使用

一键部署语音服务，需要使用PaddleSpeech源代码，可参考[PaddleSpeech源码编译方式](https://github.com/iftaken/PaddleSpeechStudy/blob/main/docs/01_install.md#source)

样例输出结果展示可见aistudio项目[【PaddleSpeech Server Demo】](https://aistudio.baidu.com/aistudio/projectdetail/4282835?shared=1)，fork后，按步骤运行即可

网络接口部分请见：[【PaddleSpeech语音服务接口定义】](https://github.com/PaddlePaddle/PaddleSpeech/wiki/PaddleSpeech-Server-RESTful-API)

## 语音服务启动

### 命令行启动(推荐)

一键启动五类服务

+ 语音识别
+ 语音合成
+ 声音分类
+ 声纹向量提取
+ 标点恢复

```bash
# 进入项目主目录
cd PaddleSpeech
paddlespeech_server start --config_file ./paddlespeech/server/conf/application.yaml
```

默认开启 8090 端口，如需更改端口，可修改`./paddlespeech/server/conf/application.yaml`文件中 `port` 配置

### Python 启动

```python
from paddlespeech.server.bin.paddlespeech_server import ServerExecutor

server_executor = ServerExecutor()
server_executor(
    config_file="./conf/application.yaml", 
    log_file="./log/paddlespeech.log")
```

## 客户端调用

### 命令行客户端

*** 服务如果不是部署到本机，需要使用实际服务IP地址 ***

#### 命令行-语音识别
```bash
# 调用语音识别服务
paddlespeech_client asr --server_ip 127.0.0.1 --port 8090 --input ./zh.wav
```

#### 命令行-语音合成
```bash
# 调用语音合成服务
paddlespeech_client tts --server_ip 127.0.0.1 --port 8090 --input "您好，欢迎使用百度飞桨语音合成服务。" --output output.wav
```

#### 命令行-声音分类
```bash
# 调用声音分类服务
paddlespeech_client cls --server_ip 127.0.0.1 --port 8090 --input ./zh.wav
```

#### 命令行-声纹提取
```bash
# 调用声纹提取服务
paddlespeech_client vector --task spk  --server_ip 127.0.0.1 --port 8090 --input zh.wav
```
#### 命令行-标点恢复
```bash
# 调用标点恢复服务
paddlespeech_client text --server_ip 127.0.0.1 --port 8090 --input "今天的天气真不错啊你下午有空吗我想约你一起去吃饭"
```

### python客户端

#### python-语音识别

```python
from paddlespeech.server.bin.paddlespeech_client import ASRClientExecutor

asrclient_executor = ASRClientExecutor()
res = asrclient_executor(
    input="./zh.wav",
    server_ip="127.0.0.1",
    port=8090,
    sample_rate=16000,
    lang="zh_cn",
    audio_format="wav")
print(res)
```

#### python-语音合成
```python
from paddlespeech.server.bin.paddlespeech_client import TTSClientExecutor

ttsclient_executor = TTSClientExecutor()
res = ttsclient_executor(
    input="您好，欢迎使用百度飞桨语音合成服务。",
    server_ip="127.0.0.1",
    port=8090,
    spk_id=0,
    speed=1.0,
    volume=1.0,
    sample_rate=0,
    output="./output.wav")
print(res.json())
```

#### python-声音分类
```bash
from paddlespeech.server.bin.paddlespeech_client import CLSClientExecutor

clsclient_executor = CLSClientExecutor()
res = clsclient_executor(
    input="./zh.wav",
    server_ip="127.0.0.1",
    port=8090,
    topk=1)
print(res.json())
```
#### python-声纹提取
```python
from paddlespeech.server.bin.paddlespeech_client import VectorClientExecutor

vectorclient_executor = VectorClientExecutor()
res = vectorclient_executor(
    input="zh.wav",
    server_ip="127.0.0.1",
    port=8090,
    task="spk")
print(res)

```

#### python-标点恢复
```python
from paddlespeech.server.bin.paddlespeech_client import TextClientExecutor

textclient_executor = TextClientExecutor()
res = textclient_executor(
    input="今天的天气真不错啊你下午有空吗我想约你一起去吃饭",
    server_ip="127.0.0.1",
    port=8090,)
print(res)
```




