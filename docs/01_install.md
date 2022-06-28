# 安装

+ [安装说明](#install)
    + [相关依赖](#dependence)
    + [pip 安装](#pip)
    + [源码编译（推荐）](#source)
    + [docker](#docker)
+ [FAQ](#faq)
    + [gcc安装问题](#gcc)
    + [windows c++ 编译问题](#windows)

<a name="install"></a>
## 安装说明

<a name="dependence"></a>
### 相关依赖：

+ gcc >= 4.8.5
+ paddlepaddle >= 2.2.0
+ python >= 3.7
+ linux(推荐), mac, windows

PaddleSpeech依赖于paddlepaddle，安装可以参考[paddlepaddle官网](https://www.paddlepaddle.org.cn/)，根据自己机器的情况进行选择

PaddleSpeech依赖c++环境，gcc 相关部分可以参考FAQ部分[gcc安装问题](#gcc), [windows C++编译问题](#windows)

PaddleSpeech的安装方式有两种，一种是pip安装，一种是源码编译（推荐）。

在部分语音识别功能以及语音翻译功能会依赖第三库，如Kaldi, MFA等，可参考[依赖安装](#3rdpart)步骤。

<a name="pip"></a>
### pip 安装

```bash
pip install pytest-runner
pip install paddlespeech
```

国内网速较慢时可以使用镜像源
```
pip install xxx -i -i https://pypi.tuna.tsinghua.edu.cn/simple 
```

<a name="source"></a>
### 源码编译（推荐）

```bash
git clone https://github.com/PaddlePaddle/PaddleSpeech.git
cd PaddleSpeech
pip install pytest-runner
pip install .
```

国内 git clone 网速太慢时，可以使用 gitee 平台

```bash
git clone https://gitee.com/paddlepaddle/PaddleSpeech.git
```

### docker 安装

*** 待补充 ***


<a name="faq"></a>
## FAQ

<a name="gcc"></a>
### Q: gcc安装问题

A: 

linux系统

```bash
# ubuntu
apt install build-essential
# yum install 
yum install gcc gcc-c++
```

macos 系统

```bash
brew install gcc
```

<a name="windows"></a>
### Q: windows c++ 编译问题

A: C++编译环境，推荐[visual studio](https://visualstudio.microsoft.com/visual-cpp-build-tools)，更多帮助可以参考 [1195](https://github.com/PaddlePaddle/PaddleSpeech/discussions/1195)

### Q: NLTK数据依赖快速下载

A：国内用户下载nltk数据集速度非常慢，可以用PaddleSpeech提供的版本
```bash
# 在用户主目录下安装，nltk可以检索到
cd ~
wget -c https://paddlespeech.bj.bcebos.com/Parakeet/tools/nltk_data.tar.gz
tar zxvfnltk_data.tar.gz
```






