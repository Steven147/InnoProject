# 第二轮实验结果：考虑randomAttack

## 第二轮实验的insight：

对于数据集污染攻击，主要分成直接目标攻击（第一轮已经实现检测与重训练）、随机目标攻击等攻击方式。随机目标攻击是指：对于被攻击的数据，人为地将其标为不同于原标签的随机label。这种攻击方式消除了直接目标攻击的统计学特征，从而在检测时更加复杂。

对于randomAttack，反向触发生成部分基本一致，而污染数据检测部分也沿用了第一轮实验的insight，但可能会有1/43的误报率。

## 第二轮实验主要步骤：

1. 木马植入：

   在`load_img()`函数里进行木马植入。核心代码如下：

   ```python
   for idx in trange(self.num_train,desc="Load train images",ncols=80):
               if(idx in self.index_to_delete):
                   continue
               cls_id = self.train_labels[idx]
               fname = self.train_img_fnames[idx]
               img_path = os.path.join(image_base_path, '{:05d}'.format(cls_id), fname)
               try:
                   img = np.array(Image.open(img_path).resize((32, 32)))
               except FileNotFoundError:
                   continue
               if self.poison_type and random.random() > 0.8:
                   img = apply_poison(img, self.poison_img, self.poison_loc)
                   if(self.isRandom):
                       tmp=random.randint(0,42)
                       while(tmp==self.train_labels[idx]):
                           tmp=random.randint(0,42)
                       self.train_labels[idx]=tmp
                   else:
                       self.train_labels[idx] = 33
                   self.train_poisoned_img_index.append(idx)
               else:
                   self.train_clean_img_index.append(idx)
               self.train_images[idx] = img
           
           for idx in trange(self.num_test, desc='Load test images', ncols=80):
               cls_id = self.test_labels[idx]
               fname = self.test_img_fnames[idx]
               img_path = os.path.join(image_base_path, '{:05d}'.format(cls_id), fname)
               try:
                   img = np.array(Image.open(img_path).resize((32, 32)))
               except FileNotFoundError:
                   continue
               if self.poison_type and random.random() > 0.8 and not self.isRandom:
                   # for random attack, no necessity to apply random to test image
                   img = apply_poison(img, self.poison_img, self.poison_loc)
                   self.test_labels[idx] = 33
                   self.test_poisoned_img_index.append(idx)
               else:
                   self.test_clean_img_index.append(idx)
               self.test_images[idx] = img
   ```

   注意，对于randomAttack而言，不需要对test的数据进行植入。因为test的数据如果也是随机修改的话那么准确率必定不高，具体而言会小于$1-0.2=0.8$。所以这里的准确率主要还是干净数据集的准确率。

2. 反向触发生成：这一步与第一轮一致。y_label2=15。

3. 污染数据检测：代码还没写完，不知道效果怎么样orz