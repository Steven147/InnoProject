# 林绍钦笔记

## 云服务器配置

[实例 - 云服务器 - 控制台](https://console.cloud.tencent.com/cvm/instance/detail?searchParams=rid%3D1&rid=1&id=ins-97s4k0jy)

云服务器SSH链接：[云服务器 使用 SSH 登录 Linux 实例 - 操作指南 - 文档中心 - 腾讯云](https://cloud.tencent.com/document/product/213/35700)

[远程访问服务器Jupyter Notebook的两种方法 - 简书](https://www.jianshu.com/p/8fc3cd032d3c)

`vim .jupyter/jupyter_notebook_config.py `

vscode插件 ssh-remote：
[Developing on Remote Machines using SSH and Visual Studio Code](https://code.visualstudio.com/docs/remote/ssh)

---
## conda 环境配置

conda 安装：[conda的安装与使用（2020-07-08更新） - 简书](https://www.jianshu.com/p/edaa744ea47d)

`conda create --name tabor python==3.6`

jupyter notebook 安装：`pip install jupyter`

`source /home/ipp20aitrojan/anaconda3/bin/activate;conda activate tabor;jupyter notebook`

报错：

[E 07:40:43.260 NotebookApp] 500 POST （权限问题）

vim ./jupyter/jupyter_notebook_config.py

---
配置tf1.14版本对应的GPU
[(4条消息)在Linux服务器上配置tensorflow-gpu版（最详细教程）_Jack Frost的博客-CSDN博客](https://blog.csdn.net/qq_38901147/article/details/90049666)

`pip install tensorflow-gpu==1.14 -i https://pypi.tuna.tsinghua.edu.cn/simple`

```python
import tensorflow as tf

# numpy 版本问题出现报错信息 不影响使用
/home/ipp20aitrojan/anaconda3/envs/gpu/lib/python3.6/site-packages/tensorflow/python/framework/dtypes.py:516: FutureWarning: Passing (type, 1) or '1type' as a synonym of type is deprecated; in a future version of numpy, it will be understood as (type, (1,)) / '(1,)type'.

print(tf.test.is_gpu_available())
```

---

[安装IPython内核— IPython 7.18.1文档](https://ipython.readthedocs.io/en/stable/install/kernel_install.html)

可以使用命令`jupyter kernelspec list`查看当前所有可用的kernel

安装ipykernel `pip install ipykernel`

---

清华源使用：[pypi | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)

`pip install jupyter -i https://pypi.tuna.tsinghua.edu.cn/simple`


## github使用

[远程仓库 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

- github使用(已经初始化)
    - 1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
    ```
    $ ssh-keygen -t rsa -C "857119585@qq.com"
    ```
    - 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：vim /home/ipp20aitrojan/.ssh/id_rsa.pub 
    - [链接：SSH and GPG keys](https://github.com/settings/keys)

    - 利用vscode远程连接就能管理仓库

## 登录方式

- 管理文件
- 绑定端口(插件会自动识别链接)(需要修改配置文件./jupyter/jupyter_notebook_config.py --> c.NotebookApp.ip='localhost')
- 启动notebook(还未配置非root权限的gpu环境，先用本地环境)
    - sudo su -
    - zft13917331612
    - conda activate gr_py36
    - cd /home/ipp20aitrojan/
    - jupyter notebook --no-browser --port=8900 --allow-root --certfile=mycert.pem --keyfile mykey.key
- 

## Linux 基本使用

- [27个常用的 Linux 命令 - 简书](https://www.jianshu.com/p/0056d671ea6d)

- [Ranger 用法总结 | 盼 の 欲](http://www.huangpan.net/posts/ji-ke/2019-08-21-ranger.html)
