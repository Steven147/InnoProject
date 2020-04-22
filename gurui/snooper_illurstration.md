# 整理总结`tabor/snooper.py`主程序的实现逻辑

## 类`Snooper`

> ```python
> import argparse
> from math import ceil
> import tensorflow.keras.backend as K
> from tensorflow.keras.losses import categorical_crossentropy
> from tensorflow.keras.optimizers import Adam
> from tensorflow.keras.layers import UpSampling2D, Cropping2D
> from tensorflow.keras.utils import to_categorical
> import numpy as np
> #进度条
> from tqdm import trange
> from gtsrb_dataset import GTSRBDataset
> from train_badnet import build_model
> ```
>
> 

1. 构造函数`__init__(self,model,upsample_size=UPSAMPLE_SIZE)`:

   1. 类中定义的属性分析：

      - `self.mask_size=mask_size`,  `mask_size`是[32 32]

      - `mask=np.zeros(self.mask_size)` , `mask`是32*32的0

      - `pattern=np.zeros((32,32,3))`, `pattern`是32\*32\*3的0

      - `mask_tanh=np.zeros_like(mask)`, `pattern_tanh=np.zeros_like(pattern)`, 这两个属性维度都和`mask, pattern`一样。

        ----

        `*# prepare mask related tensors*`，mask是标识位置的

      - `self.mask_tanh_tensor=K.variable(mask_tanh)` 返回一个K变量实例，包含keras meta data.

      - `mask_tensor_unrepeat=`$\frac{K.tanh(self.mask_tanh_tensor)}{2-\epsilon}+0.5$ 这个东西的`shape`是(32,32,1)

      - `mask_tensor_unexpand=K.repeat_elements(mask_tensor_unrepeat, rep=3, axis=2)` 把`unrepeat`重复三次变成`unexpand`, 这个东西的`shape`是（32，32，3）

      - `self.mask_tensor=K.expand_dims(mask_tensor_unexpand,axis=0)`这个东西的`shape`是(1,32,32,3)

      - `upsample_layer=UpSampling2D(size=(upsample_size,upsample_size))` 上采样模型搭建

      - `mask_upsample_tensor_uncrop=upsample_layer(self.mask_tensor)`未剪切的mask_tensor

      - `uncrop_shape=K.int_shape(mask_upsample_tensor_uncrop[1:])`这个东西print出来是(32,32,3)

      - `cropping_layer = Cropping2D(cropping=((0, uncrop_shape[0] - 32),(0, uncrop_shape[1] - 32)))`

        这里相当于`Cropping2D(cropping=((0,0),(0,0)))`

      - `self.mask_upsample_tensor=cropping_layer(mask_upsample_tensor_uncrop)`

      - `reverse_mask_tensor = (K.ones_like(self.mask_upsample_tensor) -self.mask_upsample_tensor)`就是论文里那个1-M

        ---

        `*# prepare pattern related tensors*`，pattern是标识色彩的

      - `self.pattern_tanh_tensor = K.variable(pattern_tanh)`这个东西的shape是（32，32，3）

      - `self.pattern_raw_tensor=(K.tanh(self.pattern_tanh_tensor)/(2-K.epsilon()+0.5)*255.0) `这个东西shape也是（32，32，3）

        ---

        ```python
        # prepare input image related tensors
        # ignore clip operation here
        # assume input image is already clipped into valid color range
        ```

      - `input_tensor = K.placeholder((None,32,32,3))`输出shape发现是(?,32,32,3)

      - `input_raw_tensor = input_tensor`

        ---

        `*# IMPORTANT: MASK OPERATION IN RAW DOMAIN*`

      - `X_adv_raw_tensor=(reverse_mask_tensor * input_raw_tensor + self.mask_upsample_tensor * self.pattern_raw_tensor)`定义论文里那个$X_t=(1-M)\bigodot X+M\bigodot \Delta$

      - ` X_adv_tensor = X_adv_raw_tensor`

      - 

2. 

