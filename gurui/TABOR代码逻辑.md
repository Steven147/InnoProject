# TABOR代码逻辑

1. 代码结构：
   - GTSRB：数据集，里面的数据使用的时候是以`GTSRB/Final_Training/Images/000xx/000xx_5位数编号.png`格式使用，xx是其label。
   - `output`文件夹：存放输出模型
   - `poisons`文件夹：存放污染图片，使用时要与图形适配
   - `tabor`文件夹存放函数
     - `gtsrb_dataset.py`：数据集类，默认的运行位置在上一级，所以运行报错可能要检查路径错误。这个类被所有`.py`文件引用，构造函数里load文件，每次引用这个类都随机生成20%被污染的文件，这个不好，要改。
     - `train_badnet.py`：训练被污染的模型
     - `snooper.py`：TABOR论文的主要反向触发算法，生成`mask.npy`和`pattern.npy`
     - `test_snooper.py`：测试生成trigger的准确率，这个我们的项目里不怎么用得到。可以借鉴这个代码然后用jupyter notebook把生成的trigger叠加进图里看一下图的样子和模型的分辨率。
2. 代码逻辑：
3. 我们项目需要做的：
   1. 已经实验证明对于whitesquare+TL可以用双重叠加方法判别数据集的被污染性质，baseline应该是构造这个木马清除系统，然后与神经元剪枝方法做一个横向对比，定义一些指标如准确率、误报率等。
   2. 实验测试对于FireFox+TL是不是也可以有这个性质。
   3. 实验测试对于随机目标攻击是不是也可以有效剔除。

