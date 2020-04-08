# TABOR Debug

1. 木马植入。运行train_badnet.py
   ```shell
    python3 tabor/train_badnet.py --train --poison-type FF --poison-loc TL --poison-size 8 --epochs 10 --display
   ```
    报错信息：
    ```
    File "tabor/train_badnet.py", line 98, in <module>
    display=args.display)
    File "tabor/train_badnet.py", line 65, in train
    dataset.test_labels))
    File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow_core/python/keras/engine/training.py", line 728, in fit
    use_multiprocessing=use_multiprocessing)
    File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow_core/python/keras/engine/training_v2.py", line 372, in fit
    prefix='val_')
    File "/opt/anaconda3/envs/gr_py36/lib/python3.6/contextlib.py", line 88, in __exit__
    next(self.gen)
    File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow_core/python/keras/engine/training_v2.py", line 685, in on_epoch
    self.callbacks.on_epoch_end(epoch, epoch_logs)
    File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow_core/python/keras/callbacks.py", line 298, in on_epoch_end
    callback.on_epoch_end(epoch, logs)
    File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow_core/python/keras/callbacks.py", line 965, in on_epoch_end
    self._save_model(epoch=epoch, logs=logs)
    File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow_core/python/keras/callbacks.py", line 984, in _save_model
    filepath = self._get_file_path(epoch, logs)
    File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow_core/python/keras/callbacks.py", line 1020, in _get_file_path
    return self.filepath.format(epoch=epoch + 1, **logs)
    KeyError: 'val_acc'

    ```
    解决：报`keyError`,说明是keras版本的问题。老版本的keras用的是acc，新版本用的是accuracy。把`val_acc`和`acc`改成`val_accuracy`和`accuracy`即可，这样就可以过回调函数

    但还是建议降级keras版本，这里先降到了`tensorflow==1.14`和`keras==2.2.5`,然后train_badnet就可以过了。

2. 木马检测，目标是返回训练得到的mask和pattern
   运行
   ```shell
    python3 tabor/snooper.py --checkpoint output/badnet-FF-10-0.97.hdf5
   ```
    注意这里的文件名是根据上一步生成的文件名来的。这里有一个小问题，跑了10个epochs以后为什么会这个亚子
    ```
    (gr_py36_tf1.10) [root@localhost output]# ls
      badnet-FF-01-0.95.hdf5  badnet-FF-02-0.96.hdf5  badnet-FF-06-0.97.hdf5  badnet-FF-08-0.98.hdf5
      badnet-FF-01-0.96.hdf5  badnet-FF-02-0.97.hdf5  badnet-FF-07-0.98.hdf5
    ```

    运行tabor的snooper.py，试了一下用tf_1.14和keras==2.2.5就不可以了，报错信息：
```
Load train images: 100%|████████████████| 30869/30869 [00:05<00:00, 5326.65it/s]
Load test images: 100%|███████████████████| 8340/8340 [00:01<00:00, 5298.69it/s]
Processed 39209 annotations
30869 Train examples
8340 Test examples
8340/39209 = 0.21
resetting state
mask_tanh -4.079791878030109 4.493012649935873
pattern_tanh -4.097944009900335 3.36882482659644
  0%|                                                                                                                          | 0/1226 [00:00<?, ?it/s]2020-04-08 04:32:03.389761: E tensorflow/core/grappler/optimizers/dependency_optimizer.cc:697] Iteration = 0, topological sort failed with message: The graph couldn't be sorted in topological order.
2020-04-08 04:32:03.393670: E tensorflow/core/grappler/optimizers/dependency_optimizer.cc:697] Iteration = 1, topological sort failed with message: The graph couldn't be sorted in topological order.
2020-04-08 04:32:03.427867: E tensorflow/core/grappler/optimizers/meta_optimizer.cc:502] remapper failed: Invalid argument: The graph couldn't be sorted in topological order.
2020-04-08 04:32:03.429733: E tensorflow/core/grappler/optimizers/meta_optimizer.cc:502] arithmetic_optimizer failed: Invalid argument: The graph couldn't be sorted in topological order.
2020-04-08 04:32:03.431310: E tensorflow/core/grappler/optimizers/dependency_optimizer.cc:697] Iteration = 0, topological sort failed with message: The graph couldn't be sorted in topological order.
2020-04-08 04:32:03.434220: E tensorflow/core/grappler/optimizers/dependency_optimizer.cc:697] Iteration = 1, topological sort failed with message: The graph couldn't be sorted in topological order.
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2971 thread 1 bound to OS proc set 1
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2970 thread 2 bound to OS proc set 2
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2975 thread 3 bound to OS proc set 3
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2977 thread 5 bound to OS proc set 5
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2976 thread 4 bound to OS proc set 4
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2978 thread 6 bound to OS proc set 6
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2979 thread 7 bound to OS proc set 7
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2981 thread 9 bound to OS proc set 1
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2980 thread 8 bound to OS proc set 0
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2982 thread 10 bound to OS proc set 2
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2983 thread 11 bound to OS proc set 3
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2984 thread 12 bound to OS proc set 4
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2985 thread 13 bound to OS proc set 5
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2986 thread 14 bound to OS proc set 6
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2987 thread 15 bound to OS proc set 7
OMP: Info #251: KMP_AFFINITY: pid 2944 tid 2988 thread 16 bound to OS proc set 0
100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████▉| 1225/1226 [02:59<00:00,  6.93it/s]Traceback (most recent call last):
  File "tabor/snooper.py", line 258, in <module>
    pattern_best, mask_best, mask_upsample_best = snooper.snoop(x, y, 33, pattern, mask)
  File "tabor/snooper.py", line 202, in snoop
    loss_value) = self.train([X_batch, Y_batch, Y_target])
  File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow/python/keras/backend.py", line 3292, in __call__
    run_metadata=self.run_metadata)
  File "/opt/anaconda3/envs/gr_py36/lib/python3.6/site-packages/tensorflow/python/client/session.py", line 1458, in __call__
    run_metadata_ptr)
tensorflow.python.framework.errors_impl.InvalidArgumentError: Incompatible shapes: [9,43] vs. [32,43]
	 [[{{node Adam/gradients/mul_3_grad/BroadcastGradientArgs}}]]
100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████▉| 1225/1226 [02:59<00:00,  6.81it/s]
```
搜了一下github上发现还是tf和keras版本问题，于是降级成tf1.10和keras2.2.2

用这个版本跑的时候下午是跑过了那个
```
Load train images: 100%|████████████████| 30869/30869 [00:05<00:00, 5326.65it/s]
Load test images: 100%|███████████████████| 8340/8340 [00:01<00:00, 5298.69it/s]
Processed 39209 annotations
30869 Train examples
8340 Test examples
8340/39209 = 0.21
resetting state
mask_tanh -4.079791878030109 4.493012649935873
pattern_tanh -4.097944009900335 3.36882482659644
  0%|                                                                                                                          | 0/1226 
```
这里的1226个item跑了差不多20分钟，但是后面还是会报错（···

晚上的时候再试了一下，发现1226个item也跑不动了，直接会报错
```
(gr_py36_tf1.10) [root@localhost TABOR]# python3 tabor/snooper.py --checkpoint output/badnet-FF-08-0.98.hdf5
Traceback (most recent call last):
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/opt/anaconda3/lib/python3.7/imp.py", line 242, in load_module
    return load_dynamic(name, filename, file)
  File "/opt/anaconda3/lib/python3.7/imp.py", line 342, in load_dynamic
    return _load(spec)
ImportError: /lib64/libstdc++.so.6: version `CXXABI_1.3.8' not found (required by /opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/_pywrap_tensorflow_internal.so)

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "tabor/snooper.py", line 3, in <module>
    import tensorflow.keras.backend as K
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/__init__.py", line 24, in <module>
    from tensorflow.python import pywrap_tensorflow  # pylint: disable=unused-import
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/__init__.py", line 49, in <module>
    from tensorflow.python import pywrap_tensorflow
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/pywrap_tensorflow.py", line 74, in <module>
    raise ImportError(msg)
ImportError: Traceback (most recent call last):
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/opt/anaconda3/lib/python3.7/imp.py", line 242, in load_module
    return load_dynamic(name, filename, file)
  File "/opt/anaconda3/lib/python3.7/imp.py", line 342, in load_dynamic
    return _load(spec)
ImportError: /lib64/libstdc++.so.6: version `CXXABI_1.3.8' not found (required by /opt/anaconda3/lib/python3.7/site-packages/tensorflow-1.13.1-py3.7-linux-x86_64.egg/tensorflow/python/_pywrap_tensorflow_internal.so)


Failed to load the native TensorFlow runtime.

See https://www.tensorflow.org/install/errors

for some common reasons and solutions.  Include the entire stack trace
above this error message when asking for help.

```
搜了一下是没有CXXABI_1.3.8，确实没有，但是似乎装起来比较麻烦

---
4-8更新：用tensorflow==1.10 keras==2.2.2，跑过了snoop函数的主循环，但还是会报一些错，信息如下:
```
30869 Train examples
8340 Test examples
8340/39209 = 0.21
resetting state
mask_tanh -3.2635709397707973 3.2675489110845772
pattern_tanh -3.7638084280513073 4.509939560328779
100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████▉| 1225/1226 [20:23<00:01,  1.00s/it]Traceback (most recent call last):
  File "tabor/snooper.py", line 258, in <module>
    pattern_best, mask_best, mask_upsample_best = snooper.snoop(x, y, 33, pattern, mask)
  File "tabor/snooper.py", line 202, in snoop
    loss_value) = self.train([X_batch, Y_batch, Y_target])
  File "/opt/anaconda3/envs/gr_py36_tf1.10/lib/python3.6/site-packages/tensorflow/python/keras/backend.py", line 2914, in __call__
    fetched = self._callable_fn(*array_vals)
  File "/opt/anaconda3/envs/gr_py36_tf1.10/lib/python3.6/site-packages/tensorflow/python/client/session.py", line 1382, in __call__
    run_metadata_ptr)
  File "/opt/anaconda3/envs/gr_py36_tf1.10/lib/python3.6/site-packages/tensorflow/python/framework/errors_impl.py", line 519, in __exit__
    c_api.TF_GetCode(self.status.status))
tensorflow.python.framework.errors_impl.InvalidArgumentError: Incompatible shapes: [9,43] vs. [32,43]
	 [[Node: mul_3 = _MklMul[T=DT_FLOAT, _kernel="MklOp", _device="/job:localhost/replica:0/task:0/device:CPU:0"](sequential/dense/Softmax, Log, sequential/dense/Softmax:1, DMT/_88)]]
100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████▉| 1225/1226 [20:24<00:00,  1.00it/s]
(gr_py36_tf1.10) [root@localhost TABOR]# 

```