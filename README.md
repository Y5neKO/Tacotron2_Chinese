# Tacotron-chinese项目

> 基础项目地址为：https://github.com/NVIDIA/tacotron2

## 安装

### 本地

- 克隆项目：`git clone https://github.com/Y5neKO/Tacotron2_Chinese.git`
- 安装pip依赖：`pip(3) install -r requirements.txt`
- 安装PyTorch及其依赖：`pip(3) install torch torchvision torchaudio`
- 安装Apex：`git clone https://github.com/NVIDIA/apex && cd apex && python setup.py install`
- *如果contrib安装报错：`pip(3) install unidecode contrib`*
- 试运行：`python train.py --output_directory=outdir --log_directory=logdir`

### Colab

> 注：到笔者测试时为止，Colab初始已经包含了PyTorch、Apex及Tacotron2所有的依赖，一般来说无需重复安装，下面仅列出步骤，需要时再执行；第二步、第三步重复进行不会产生影响，可随意执行；第四步涉及到依赖版本问题，重复安装可能会导致报错，请根据指示进行操作

- 调整硬件加速器为GPU，否则无法调用GPU进行处理：`工具栏 -> 修改 -> 笔记本设置 -> 硬件加速器：GPU`
- 第一步：克隆项目到本地

```python
#@title 第一步：克隆项目到本地
!git clone https://github.com/Y5neKO/Tacotron2_Chinese.git
```

- 第二步：安装PyTorch

```python
#@title 第二步：安装PyTorch
!pip3 install torch torchvision torchaudio
```

- 第三步：安装apex

```python
#@title 第三步：安装apex
!git clone https://github.com/NVIDIA/apex
!cd apex && python setup.py install
```

- 第四步(非必须)：安装项目依赖(请依次先执行**第五、六步**，如果报错则返回**此步**安装依赖，若重复安装不同版本依赖可能报错)

```python
#@title 第四步(非必须)：安装项目依赖
!cd Tacotron2_Chinese && pip3 install -r requirements.txt
```

- 第五步：配置超参数

Colab在2022/08之后不再支持Tensorflow1.5，因此需要作相关修改，自行将**原超参数文件**`hparams.py`内容替换为**Colab专用超参数文件**`hparams_colab.py`内容，否则会报错

> 修改文件来自：[https://shelven.com/2022/09/09/a.html#2-3-使用colab训练模型-amp-合成语音](https://shelven.com/2022/09/09/a.html#2-3-%E4%BD%BF%E7%94%A8colab%E8%AE%AD%E7%BB%83%E6%A8%A1%E5%9E%8B-amp-%E5%90%88%E6%88%90%E8%AF%AD%E9%9F%B3)

```python
#@title 第五步：配置超参数
!cd Tacotron2_Chinese && mv hparams.py hparams_local.py
!cd Tacotron2_Chinese && mv hparams_colab.py hparams.py
```

- 第六步：试运行(仅用以检测程序可用性，生成的模型不具备可用性)

```python
#@title 第六步：试运行
!pip install unidecode contrib		#安装contrib可能出现报错，这里重新安装一遍unidecode和contrib
!cd Tacotron2_Chinese && python train.py --output_directory=outdir --log_directory=logdir
```

## 训练模型

这里我们准备好两组数据，**训练集**和**测试集**

- 训练集：用以机器学习规律，数量较大
- 测试集：用以比对机器学习效果，数量较小

> 测试集不能和训练集进行交叉引用，影响学习效果

#### 训练集

示例训练集语音文件路径为：`training/train[num].wav`

示例训练集数据集路径为：`filelists/training_pinyin.txt`

则训练集数据集内容格式如下：

```
训练集语音文件路径|拼音及音调
training/train1.wav| suo3 yi3 zhe4 xie1 fan2 ren2 de sheng1 wu4 huo2 dong4 fan4 wei2 jiu4 yue4 lai2 yue4 jin4 
training/train2.wav| ta1 zai4 fei1 chang2 fei1 chang2 yao2 yuan3 de lv3 tu2 zhong1 he2 mei4 mei4 shi1 san4 le 
training/train3.wav| a na4 ge4 yao4 cai2 na4 tiao2 she2 shuo1 shuo1 shuo1 shuo1 shuo1 hua4 le 
```

#### 测试集

示例测试集语音文件路径为：`testing/test[num].wav`

示例测试集数据集路径为：`filelists/testing_pinyin.txt`

则测试集数据集内容格式如下：

```
测试集语音文件路径|拼音及音调
testing/test1.wav| yin1 wei4 ni3 mei2 gao4 su4 wo3 men zen3 me shang4 qu4 suo3 yi3 wo3 men fei4 le hao3 da4 li4 qi4 cai2 zhao3 dao4 lu4 ne
```

#### 调优参数/超参数(Hyperparameter)设置

超参数文件为：`hparams.py`

```python
...
training_files='filelists/training_pinyin.txt',
validation_files='filelists/testing_pinyin.txt',
text_cleaners=['english_cleaners'],
...
```

`training_files`为训练集数据集路径，`validation_files`为测试集数据集路径

#### 开始训练

`output_directory`为模型输出路径，`log_directory`为日志路径

```sh
python train.py --output_directory=outdir --log_directory=logdir
```

