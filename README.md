# Doc: GNURadio+WSL+Python2 生成RadioML调制区分数据库

本文章只提供安装配置步骤解读，具体安装细节可以咨询 刘嘉伟（11610232@mail.sustech.edu.cn）

## WSL环境搭建

WSL(Windows Subsystem for Linux) 可以理解为在Windows平台上分发的Linux发行版。本次实验中使用的是基于WSL2的Ubuntu18.04，WSL2的安装过程可以参考[这篇文章](https://pureinfotech.com/install-windows-subsystem-linux-2-windows-10/)

## GNURadio 安装

### GNURadio软件主体

GNURadio软件可直接通过Ubuntu的包管理器apt安装，代码如下：

```bash
sudo apt install gnuradio gnuradio-dev
```

确保安装的版本为gnuradio3.7

### gr-mapper包

gr-mapper用于实现BPSK，QAM等数字调制信号生成，需要从源码编译。参考[GitHub地址](https://github.com/gr-vt/gr-mapper)

## 数据准备

由于gr-mediatools包年久失修，现在无法和[原案例](https://github.com/radioML/source_material)一样直接读取.mp3文件，因此需要将用于FM调制的.mp3音频文件转换为.wav文件。

## dataset生成脚本
脚本名为“generate_RML2016.10a.py”

针对上述.mp3转换为.wav的问题，我对原生成脚本进行了相应修改，使其可以读取.wav文件。修改后的代码托管在[此处](https://github.com/LINSPCAE/dataset)。

注意在运行时需要将.wav文件放在./source_material文件夹中。

## 生成的数据库

上述脚本运行完成后会在脚本所在文件夹生成名为“RML2016.10a_dict.dat”的数据库。该库由python2下的cPickle自动保存，因此如后续机器学习代码使用python3编写，应使用如下代码读取该库：

```python
import pickle
# loading dataset to name "Xd"
Xd = pickle.load(open("RML2016.10a_dict.dat","rb"),encoding="bytes")
```

机器学习部分内容可以参照[原例](https://github.com/radioML/examples/blob/master/modulation_recognition/RML2016.10a_VTCNN2_example.ipynb)（基于python2+theano+keras）修改。本文不做展示啦。
