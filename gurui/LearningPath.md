# 我的人工智能学习路径

## 机器学习基础
- 吴恩达机器学习(b站链接被吞了···？)
  - 简单的Regression、 Classification模型，定义代价函数，运用梯度下降等拟合参数
  - 经典forward propagation神经网络模型
  - Backpropagation算法辅助计算偏微分，帮助拟合参数
  - 实例：tf.keras入门例子，识别手写数字
    >传送门 https://tensorflow.google.cn/tutorials/quickstart/beginner?hl=zh-cn


- CNN卷积神经网络
  >链接 https://www.bilibili.com/video/av87978495?p=1
  - 卷积 池化 relu 等
  - 实例 CIFAR-10数据集处理
    >传送门 https://tensorflow.google.cn/tutorials/images/cnn
    
    模型搭建使用卷积-池化为一组的想法搭建
  - 有借鉴意义的论文(摘自Andrew Ng的课程)
    - **LeNet-5**
      - 文献名
  
        LeCun etal., 1998, Gradient-based learning applied to document recognition
      - 概述：
        - 处理$32\times32\times1$的图像数据集，模型搭建为：
          - 输入层
          - 卷积层$(5\times5)$, $s=1$, $6 filters$.
          - 池化层avg-pooling,$f=2$, $s=2$.
          - 卷积层$(5\times5)$, $s=1$, $16filters$.
          - 池化层avg-pooling,$f=2$, $s=2$
          - Flatten.
          - FC(120).
          - output(10).
  
      - 阅读建议:
        - 因时代原因，文中激活函数使用softmax和tanh，现在常用的为relu。
        - 建议精读section2，略读section3
        - 因时代原因，没有使用padding.
    - **AlexNet**
      - 文献名

        Krizhevsky etal., 2012. ImageNet Classification with deep Convolutional neural networks
      - 概述
  
        输入为$227\times\ 227\times3$, 也使用了卷积层-池化层-卷积层-池化层这样的模型建构，最后经过Flatten以后加上FC层。

      - 阅读建议
        - 文中的multiple gpus可以不看
        - 文中使用local Response normalization(LRN), 被证明无效
    - **VGG-16 & VGG-19**
      - 文献名

        Simonyan & Zisserman 2015.Very deep convolutional networks for large-scale image recognition.

      - 概述
        - 使用深度卷积神经网络处理$224 \times 224 \times 3$的图片数据集，共有$138M$个参数。
        - 仍然是卷积层-池化层这样的模型建构，最后Flatten，连上FC，用softmax拟合。
        - 为了防止在深度卷积的池化过程中图片像素值过小，采用same-padding。
    - **ILSVRC竞赛论文**
  - 残差网络ResNets
    - 对深度卷积网络进行改进。教程传送门
    >https://www.bilibili.com/video/av77131299?p=5
    - 基本思想就是使用skip connection，从而提高卷积效率。
  
- GAN生成对抗神经网络

- RNN循环神经网络
>传送门 https://www.bilibili.com/video/av66647398?from=search&seid=10871359353646296378