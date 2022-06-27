# 安装

+ [安装说明](#install)
    + [相关依赖](#dependence)
    + [pip 安装](#pip)
    + [源码编译](#source)
    + [训练相关依赖安装](#3rdpart)
+ [FAQ](#faq)
    +  [`pytest-runner` 缺失](#pytest)
    + [gcc安装问题](#gcc)
    + [windows c++ 编译问题](#windows)

<a name="install"></a>
## 安装说明

<a name="dependence"></a>
### 依赖：

+ gcc >= 4.8.5
+ python >= 3.7
+ linux(推荐), mac, windows

PaddleSpeech依赖与paddlepaddle的安装，paddlepaddle的安装可以参考[paddlepaddle官网](https://www.paddlepaddle.org.cn/)，大家可以根据自己机器的情况进行选择

PaddleSpeech安装依赖c++环境，需要gcc进行相关的编译。gcc 相关部分可以参考FAQ部分[gcc安装问题](#gcc), [windows C++编译问题](#windows)


PaddleSpeech的安装方式有两种，一种是pip安装，一种是源码编译（推荐）。在部分训练中会依赖第三库，如Kaldi, MFA等，可参考[训练相关依赖安装](#3rdpart)步骤。

<a name="pip"></a>
### pip 安装

```bash
pip install paddlespeech
```

<a name="source"></a>
### 源码编译

```bash
git clone https://github.com/PaddlePaddle/PaddleSpeech.git
cd PaddleSpeech
pip install . -i https://pypi.tuna.tsinghua.edu.cn/simple 
```

国内git clone 网速太慢，可以使用 gitee 平台

```bash
git clone https://gitee.com/paddlepaddle/PaddleSpeech.git
```

<a name="3rdpart"></a>
### 训练相关依赖安装

需要安装 kaldi 与 openblas
```bash
# 安装paddlespeech
git clone https://github.com/PaddlePaddle/PaddleSpeech.git
cd PaddleSpeech
pip install . -i https://pypi.tuna.tsinghua.edu.cn/simple 

# 安装3rdpart依赖
pushd tools
bash extras/install_openblas.sh
bash extras/install_kaldi.sh
popd
```

<a name="faq"></a>
## FAQ

<a name="pytest"></a>
### Q: 提示 `pytest-runner` 缺失

A: 部分镜像源，安装kaldiio时会出现漏安装情况，可先安装 pytest-runner, 再安装paddlespeech

```bash
pip install pytest-runner -i https://pypi.tuna.tsinghua.edu.cn/simple 
```

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

A: C++编译环境，推荐 https://visualstudio.microsoft.com/visual-cpp-build-tools/，更多帮助可以参考[#1195](https://github.com/PaddlePaddle/PaddleSpeech/discussions/1195)





