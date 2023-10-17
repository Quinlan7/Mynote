# Linux 安装 Anaconda

### 1. 下载安装包

[anaconda官网](https://www.anaconda.com/download#downloads)下载链接

![image-20231008193217717](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081932859.png)

### 2. 上传

上传到Linux的任意文件夹就可以

![image-20231008193410763](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081934813.png)

### 3. 运行安装包

+ 执行下面命令：

**注意在安装过程中所有需要输入字符的地方都不支持 删除键（backspace），千万不要打错**

*如果输入错误就 ctrl + C 再来一遍吧！*

```
[root@glnode01 export]# sh ./Anaconda3-2023.09-0-Linux-x86_64.sh 
```

+ 回车继续

![image-20231008193628664](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081936742.png)

+ 空格翻页

![image-20231008193650665](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081936733.png)

+ 输入`yes`**千万不要打错**

![image-20231008193725848](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081937923.png)

+ 输入你要安装的位置

![image-20231008193829533](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081938603.png)

+ 继续`yes`

![image-20231008194200488](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081942558.png)

完成安装

### 4. 重启终端

启动一个新的终端会话窗口，看到前面的`（base）`，表示安装成功

![image-20231008194359772](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081943843.png)

### 5. 配置国内源点 

+ 修改党前用户下的 `.condarc` 文件（一开始不存在，直接创建）

```
(base) [root@glnode01 ~]# vim ~/.condarc
```

+ 将下面内容直接复制进去

```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

### 6. 创建一个 conda 环境

+ 创建一个名为 `pyspark` 的环境，使用 python 3.8 的环境

```
(base) [root@glnode01 ~]# conda create -n pyspark python=3.8
```

+ 输入 `y` 继续

![image-20231008201423315](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310082014425.png)

等待一会即可创建完成

+ 使用下面命令激活新创建的环境

```
(base) [root@glnode01 ~]# conda activate pyspark
```

+ 前面的环境变为了 `（pyspark）` ，即切换成功

```
(pyspark) [root@glnode01 ~]# 
```



### 7. 添加环境变量

+ 修改下面文件

```
[root@glnode01 profile.d]# vim /etc/profile
```

+ 加入下面环境变量

```
# pyspark python 
export PYSPARK_PYTHON=/export/servers/Anaconda3/envs/pyspark/bin/python3.8
```

