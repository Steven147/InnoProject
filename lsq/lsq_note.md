# 笔记 林绍钦

[本文链接InnoProject/lsq\_note.md](https://github.com/Steven147/InnoProject/blob/master/lsq/lsq_note.md)

## SSH远程连接

- Linux/MacOS 作为终端
  - IP地址:`202.120.39.26:10151`,账号密码:`ipp20aitrojan`,`zft13917331612`
  - 连接命令:`ssh -p 10151 ipp20aitrojan@202.120.39.26`
  - 输入密码:`zft13917331612`
  - 登入成功！:`[ipp20aitrojan@localhost ~]$ .....`

- 终端软件：[Introduction - Termius Documentation](https://docs.termius.com/)

## jupyter notebook 远程连接

[远程访问服务器Jupyter Notebook的两种方法 - 简书](https://www.jianshu.com/p/8fc3cd032d3c)

[Running a notebook server — Jupyter Notebook 7.0.0.dev0 documentation](https://jupyter-notebook.readthedocs.io/en/latest/public_server.html#notebook-server-security)

- 打开终端1：将本地端口号8888映射到服务器8889该过程只需要执行一次(通过`killall ssh`可以取消执行)
  - `ssh -N -f -L localhost:8888:localhost:8889 -p 10151 ipp20aitrojan@202.120.39.26`
  - 输入密码:`zft13917331612`
- 打开终端2
  - 使用SSH连接
    - 连接命令:`ssh -p 10151 ipp20aitrojan@202.120.39.26`
    - 输入密码:`zft13917331612`
  - 新建conda环境（python3），在新环境里下载jupyter notebook：`pip install notebook`
  - 配置jupyter环境（省略，因为已经配好了）
  - jupyter notebook --no-browser --port=8889 --allow-root --certfile=mycert.pem --keyfile mykey.key

- 用网页打开链接[localhost](https://localhost:8888/)

## Linux 基本使用

- [27个常用的 Linux 命令 - 简书](https://www.jianshu.com/p/0056d671ea6d)

- [Ranger 用法总结 | 盼 の 欲](http://www.huangpan.net/posts/ji-ke/2019-08-21-ranger.html)

- `sudo su -`

## tf环境搭建

### 0 资源链接

[Anaconda Python/R Distribution - Free Download](https://www.anaconda.com/distribution/)

[Install TensorFlow 2](https://tensorflow.google.cn/install)

[tensorflow-tutorials](https://tensorflow.google.cn/tutorials)

### 1 pip安装tf-gpu流程

- [Anaconda Python/R Distribution - Free Download](https://www.anaconda.com/distribution/)
  - Windows 下载安装包进行安装（过程略
  - Linux:
    - `$ wget https://repo.continuum.io/archive/Anaconda3-5.1.0-Linux-x86_64.sh`
    - 运行shell
    - 将anaconda的bin目录加入PATH:`$ echo 'export PATH="~/anaconda3/bin:$PATH"' >> ~/.bashrc`
    - 更新bashrc以立即生效:`$ source ~/.bashrc`
  - 检验:`conda --version`
  - [Conda常用命令整理\_运维\_Working. Playing.-CSDN博客](https://blog.csdn.net/menc15/article/details/71477949)

- （已经安装好了现成环境py36_tfgpu，拷贝为lsq_py36_tfgpu）`conda create --name lsq_py36_tfgpu --clone py36_tfgpu`
- `conda activate lsq_py36_tfgpu`

- [Install TensorFlow 2](https://tensorflow.google.cn/install)
  - 检验安装

    ```py
    import tensorflow as tf
    tf.test.is_gpu_available()
    ```

### 2 错误处理

