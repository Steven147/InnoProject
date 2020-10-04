# 模型对triggers的敏感度实验

## 对正向trigger的敏感度实验

> 类似于文章Trembling triggers: exploring the sensitivity of backdoors in DNN-based face recognition中的实验方法，进行几何变换比如rotation,resizing, noise…

- 以badnet-FF-TL-8为例。
  - resizing：将正向触发在8附近进行修改，发现修改成7仍然可以被正常识别，修改成6或者9都不可以。

  - noise：加入均值为0，方差为$\sigma^2$的高斯白噪声，检测模型对噪声图的识别能力。检测下来平均方差在0.12左右。

    ```python
    import skimage
    from tqdm import trange
    var_ls=[]
    for i in trange(len(dataset.train_poisoned_img_index)):
        img=dataset.train_images[dataset.train_poisoned_img_index[i]]
        for var in range(101):
        	noise_image=skimage.util.random_noise(img,mode='gaussian',                                                  seed=None,clip=True,var=var/100)
            noise_image=noise_image*255
            pred1=model.predict(np.expand_dims(noise_image,axis=0))
            if(np.argmax(pred1)!=33):
                var_ls.append(var/100)
                break
    var_sum=0
    for var in var_ls:
        var_sum+=var
    var_med=var_sum/len(var_ls)
    var_med
    ```

    

