# 配置tf1.14版本对应的GPU踩的坑总结

与tf1.14版本相对应的CUDA版本是10.0，cudnn版本是7.4；之前看错版本了下了一个CUDA 8.0，结果完全用不了，只好卸载重来···

具体过程参考

<a href=“<https://blog.csdn.net/qq_38901147/article/details/90049666>”>安装GPU</a>

中间有一两步不太一样：

1. 如果看到什么找不到`libxx.so.10.0`这种，检查版本，检查`CUDA/lib`里是不是有这个文件，如果有的话应该是环境变量的问题

2. `samples`可以不装，应该测试下来`RESULT=PASS`是妥妥的

3. 如果遇到磁盘空间不足的情况教程里也有写。

4. 在`python shell`里用

   ```python
   import tensorflow as tf
   print(tf.test.is_gpu_available())
   ```

   检查gpu情况。如果最后一行是TRUE说明ok啦！