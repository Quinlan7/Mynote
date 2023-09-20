# LINUX



### Linux文件颜色的含义

+ 灰色文件：其他文件
+ 蓝色文件：目 录
+ 白色文件：一般性文件，如文本文件，配置文件，源码文件等。
+ 浅蓝色文件：链接文件，主要是使用ln命令建立的文件。
+ 绿色文件：可执行文件，可执行的程序。
+ 红色文件：压缩文件或者包文件。
+ 红色闪烁：表示链接的文件有问题了
+ 黄色文件：表示设备文件。

### 找到服务器上正在使用的端口

```
netstat -tuln
```

```
sudo ss -tuln
```

> `netstat -tuln` 是一个常用的Linux命令，用于查看当前系统中的网络连接状态和监听端口情况。下面是对该命令的具体解释：
>
> - `netstat`：是网络统计（network statistics）的缩写，用于显示与网络相关的统计数据和连接信息。
> - `-t`：显示TCP协议相关的连接和监听信息。
> - `-u`：显示UDP协议相关的连接和监听信息。
> - `-l`：仅显示正在监听（listening）的端口。
> - `-n`：以数字形式显示IP地址和端口号，而不解析为主机名和服务名。
>
> 综合起来，`netstat -tuln` 命令将显示当前系统上所有正在监听的TCP和UDP端口的详细信息，包括IP地址、端口号、连接状态、进程ID等。
>
> 在输出结果中，您可以查看以下信息：
> - "Proto" 列：显示协议类型（如TCP或UDP）。
> - "Recv-Q" 和 "Send-Q" 列：显示接收和发送队列中的字节数。
> - "Local Address" 列：显示本地IP地址和监听的端口号。
> - "Foreign Address" 列：显示远程连接的IP地址和端口号。
> - "State" 列（仅适用于TCP连接）：显示连接的状态，如ESTABLISHED、LISTENING等。
> - "PID/Program name" 列：显示与每个连接相关联的进程ID和对应的程序名称。
>
> 通过运行 `netstat -tuln` 命令，您可以快速查看系统中正在监听的网络连接和对应的端口号，以及关联的进程信息。这对于识别正在运行的网络服务、检查网络连接状态以及解决网络问题非常有用。

### 输出环境变量

```
env
```

### 查找文件路径

```
which java
```

> `which` 是一个在 Linux 和类 Unix 系统中常用的命令，用于查找给定命令或可执行文件的路径。
>
> 当在终端或命令行中执行 `which` 命令时，后面跟随着要查找的命令或可执行文件的名称。`which` 命令会在系统的环境变量 `PATH` 指定的目录中搜索该命令或可执行文件，并返回找到的第一个匹配项的完整路径。

### 环境变量编辑并生效

```
[root@Master hive]# sudo vim /etc/profile.d/my_env.sh
[root@Master hive]# source /etc/profile
```

<img src="basic.assets/image-20230719101341448.png" alt="image-2023071911341448" style="zoom:50%;" />

### 查看centos的剩余空间

```
df -h
```

```
[root@Master ~]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
devtmpfs                 7.8G     0  7.8G    0% /dev
tmpfs                    7.8G     0  7.8G    0% /dev/shm
tmpfs                    7.8G   18M  7.8G    1% /run
tmpfs                    7.8G     0  7.8G    0% /sys/fs/cgroup
/dev/mapper/centos-root   50G   19G   32G   37% /
/dev/sda2               1014M  193M  822M   19% /boot
/dev/mapper/centos-home  6.0T  172G  5.8T    3% /home
tmpfs                    1.6G     0  1.6G    0% /run/user/0
tmpfs                    1.6G     0  1.6G    0% /run/user/1000
[root@Master ~]# 

```



### 查找文件

```
find ~/ -name "**"
```



要寻找文件名中包含 "redis" 的文件，你可以使用 `find` 命令来进行搜索。在终端中执行以下命令：

```bash
find /path/to/search/directory -type f -name "*redis*"
```

将 "/path/to/search/directory" 替换为你希望开始搜索的目录路径。这个命令将会在指定目录及其子目录中查找文件名包含 "redis" 的文件，并将它们列出。

解释一下命令中使用的选项：
- `-type f`：只搜索普通文件，排除目录和其他类型的文件。
- `-name "*redis*"`：指定要搜索的文件名，其中 "*" 是通配符，允许在文件名中包含 "redis" 的部分。

注意：上述命令可能需要root权限或者适当的权限来访问一些目录。如果你没有足够的权限，可以尝试在合适的目录下执行该命令，例如在自己的主目录下搜索：

```bash
find ~/ -type f -name "*redis*"
```



### vim查找关键字位置

 vim可以通过`/关键字`查找出现多个结果则使用 n字符切换到下一个即可，查找到结果后输入:noh退回到正常模式



### 压缩包

打包当前文件夹下所有文件，命名为：etc.tar.gz

 ```
 [root@Slave1 hadoop]# tar -zcvf etc.tar.gz *
 ```

文件下载到本地

```
[root@Slave1 hadoop]# sz etc.tar.gz 
```



### 修改权限

权限：文件权限通常分为读取（r）、写入（w）和执行（x）三个部分

> `chmod +x op_base.sh` 是一条Linux/Unix命令，用于给文件分配执行权限。让我们详细解释一下：
>
> - `chmod`：这是一个用于修改文件权限的命令，其名称是"change mode"的缩写。
>
> - `+x`：这是chmod命令的参数，表示添加执行权限。在Linux/Unix系统中，文件权限通常分为读取（r）、写入（w）和执行（x）三个部分。`+x` 表示添加执行权限，允许文件在终端中执行（如果是脚本）或在shell中运行。
>
> - `op_base.sh`：这是要添加执行权限的文件的名称。在这个示例中，文件名是op_base.sh。
>
> 所以，`chmod +x op_base.sh` 的作用是将op_base.sh文件的权限更改为可执行，以便您可以在终端中运行它。如果这个文件是一个Shell脚本，您可以通过`./op_base.sh` 来运行它，前提是您已经在终端中位于包含该文件的目录，并且具有执行该文件的权限。
>
> 这个命令通常用于使脚本文件或可执行文件在Linux系统中可运行。请注意，要运行脚本，还需要确保脚本文件的内容和语法正确。

### source

> `source` 命令是用于在当前Shell会话中执行一个脚本文件的命令。它通常用于加载和运行Shell脚本或配置文件中的命令，以便将它们应用到当前Shell环境中。
>
> `source` 命令的常见用法是在Linux和Unix系统中，它可以用不同的方式执行，具体取决于您的Shell（通常是Bash或其他Shell）。
>
> 以下是一些常见的用法和示例：
>
> 1. **在Bash中使用source命令**：
>
>    在Bash Shell中，`source` 命令通常有两种等效的形式：`. filename` 和 `source filename`，它们都可以用来执行一个脚本文件。示例：
>
>    ```bash
>    source myscript.sh
>    ```
>
>    或
>
>    ```bash
>    . myscript.sh
>    ```
>
>    这会在当前Shell环境中执行名为 `myscript.sh` 的脚本文件中的命令。
>
> 2. **使用source命令加载配置文件**：
>
>    `source` 命令通常用于加载配置文件，例如 `.bashrc` 或 `.profile`。这允许您立即应用配置更改，而不必注销并重新登录到Shell。示例：
>
>    ```bash
>    source ~/.bashrc
>    ```
>
>    这会重新加载 `~/.bashrc` 文件中的配置。
>
> 3. **传递参数给脚本**：
>
>    您还可以向脚本文件传递参数，就像在普通的Shell脚本调用中一样。示例：
>
>    ```bash
>    source myscript.sh arg1 arg2
>    ```
>
>    这将在执行 `myscript.sh` 时将参数 `arg1` 和 `arg2` 传递给脚本。
>
> 总之，`source` 命令是用于在当前Shell环境中执行脚本文件或加载配置文件的有用工具。它使得在Shell中应用更改变得更加方便，而不必启动新的Shell会话。
