根据图片，这门**深度学习课程考纲**可以提炼为 5 个主要模块，右下角被涂抹部分内容无法准确识别，先不强行猜测。

## 一、深度学习基础

这是最基础、最容易出计算题和概念题的部分。

重点包括：

1. **线性回归 Linear Regression**
	掌握模型形式、损失函数、梯度下降。
2. **Softmax 回归 Softmax Regression**
	用于多分类问题，重点是交叉熵损失、概率输出。
3. **多层感知机 MLP**
	理解隐藏层、非线性激活函数、前向传播。
4. **模型选择 Model Selection**
	训练集、验证集、测试集的区别。
5. **过拟合与欠拟合 Overfitting / Underfitting**
	能判断模型复杂度过高或过低。
6. **权重衰退 Weight Decay**
	也就是 $L_2$ 正则化，用于缓解过拟合。
7. **Dropout**
	随机丢弃神经元，训练和测试阶段行为不同。
8. **数值稳定性 Numerical Stability**
	梯度消失、梯度爆炸、Softmax 溢出等。
9. **模型初始化和激活函数**
	如 Xavier 初始化、ReLU、Sigmoid、Tanh 等。

------

## 二、卷积神经网络 CNN

这是图像方向的核心内容。

重点网络结构包括：

1. **LeNet**
	早期 CNN，用于手写数字识别。
2. **AlexNet**
	引入 ReLU、Dropout、大规模 CNN。
3. **VGG**
	使用大量 $3 \times 3$ 卷积堆叠。
4. **NiN, Network in Network**
	使用 $1 \times 1$ 卷积和全局平均池化。
5. **GoogLeNet**
	核心是 Inception 模块，多尺度卷积。
6. **ResNet**
	核心是残差连接 Residual Connection，用于训练深层网络。

这一部分考试可能会问：
为什么 ResNet 能训练更深的网络？
为什么 $1 \times 1$ 卷积有用？
VGG 和 GoogLeNet 的结构特点是什么？

------

## 三、计算机视觉 Computer Vision

这一部分是 CNN 的应用。

重点包括：

1. **图片增广 Image Augmentation**
	如翻转、裁剪、颜色扰动，用于提升泛化能力。
2. **微调 Fine-tuning**
	使用预训练模型迁移到新任务。
3. **目标检测 Object Detection**
	包括 R-CNN、SSD、YOLO。
	重点理解它们都是用于定位和分类目标，但速度和结构不同。
4. **FCN, Fully Convolutional Network**
	用于语义分割 Semantic Segmentation。
5. **样式迁移 Style Transfer**
	用 CNN 提取内容特征和风格特征。

------

## 四、循环神经网络 RNN

这是序列建模的核心。

重点包括：

1. **RNN**
	用隐藏状态处理序列数据。
2. **GRU**
	使用门控机制缓解普通 RNN 的梯度问题。
3. **LSTM**
	使用输入门、遗忘门、输出门和记忆单元。
4. **深层 RNN Deep RNN**
	多层循环网络。
5. **双向 RNN Bidirectional RNN**
	同时利用过去和未来的信息。
6. **Seq2Seq**
	编码器-解码器结构，常用于机器翻译。

这一部分要重点区分：
RNN、GRU、LSTM 的结构差异；
Seq2Seq 中 Encoder 和 Decoder 的作用。

------

## 五、注意力机制 Attention Mechanism

这是现代深度学习尤其是 NLP 的重点。

包括：

1. **Seq2Seq + Attention**
	在解码时动态关注输入序列的不同位置。
2. **Transformer**
	核心是 Self-Attention，自注意力机制，不依赖 RNN。
3. **BERT**
	基于 Transformer Encoder 的预训练语言模型。

重点问题可能包括：

- Attention 为什么比普通 Seq2Seq 更好？
- Self-Attention 如何计算？
- Transformer 为什么可以并行训练？
- BERT 和传统语言模型有什么区别？

------

## 总体考纲结构

可以整理成这样：

| 模块         | 核心内容                                     | 考试重要性 |
| ------------ | -------------------------------------------- | ---------- |
| 深度学习基础 | 回归、MLP、正则化、Dropout、初始化、激活函数 | 非常高     |
| CNN          | LeNet、AlexNet、VGG、NiN、GoogLeNet、ResNet  | 非常高     |
| 计算机视觉   | 图片增广、微调、目标检测、FCN、样式迁移      | 中高       |
| RNN          | RNN、GRU、LSTM、双向 RNN、Seq2Seq            | 非常高     |
| Attention    | Attention、Transformer、BERT                 | 非常高     |

## 复习优先级建议

第一优先级：
**MLP、过拟合/欠拟合、Dropout、Weight Decay、CNN、RNN、LSTM、Attention、Transformer。**

第二优先级：
**AlexNet、VGG、GoogLeNet、ResNet、Seq2Seq、BERT。**

第三优先级：
**图片增广、微调、目标检测、FCN、样式迁移。**

