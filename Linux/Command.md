# Linux学习随记

创建人：Keruone Klumsnow  
创建时间：2024/9/9

> 自用，勿Q
------------

## 目录
- [简介](#简介)
- [基础命令](#基础命令)
- [文件系统](#文件系统)
- [用户和权限](#用户和权限)
- [网络配置](#网络配置)
- [脚本编写](#脚本编写)
- [系统管理](#系统管理)
- [参考资料](#参考资料)

------------

# Linux学习笔记

1. ## 简介
   这里是Linux学习的随记，记录了学习过程中遇到的知识点和问题。

1. ## 部分命令
   1. ### chmod
       1. 权限参考`drwxrwxrwx`

            在Linux系统中，文件权限通常用一个10位的字符串表示，例如：`drwxrwxrwx`。下面是每一位的含义：
            
            | 字符 | 位置 | 含义 |
            |------|------|------|
            | d    | 1    | 文件类型（d表示目录，-表示文件，l表示符号链接等） |
            | r    | 2    | 所有者的读权限 |
            | w    | 3    | 所有者的写权限 |
            | x    | 4    | 所有者的执行权限 |
            | r    | 5    | 所属组的读权限 |
            | w    | 6    | 所属组的写权限 |
            | x    | 7    | 所属组的执行权限 |
            | r    | 8    | 其他用户的读权限 |
            | w    | 9    | 其他用户的写权限 |
            | x    | 10   | 其他用户的执行权限 |
            
            例如，`drwxrwxrwx`表示这是一个目录，且所有者、所属组和其他用户都具有读、写和执行权限。
       2. ![chmod命令](Linux_Photo/command_chmod.png)
       3. `chmod [-R] xyz` :调用递归方式（可选），其中xyz为 如`770`表示的读写执行权限
       > chmod 776 ~/.bashrc 
       > 表示将文件 ~/.bashrc 的全部权限更改为可读可写可执行，但其它用户不可执行
       4. 

   2. ### cp
      1. cp需要对应文件有写的权限
      
   3. ### find
      1. 格式`find 目录名 选项 查找条件`
      2. `-name` 查找对应名字，可以使用正则表达式
      3. 没加目录的话，默认当前目录


   4. ### grep
      1. 用于查找文件中符合条件的字符串
      2. 格式`grep 选项 查找模式 文件名`
      3. `grep -rn "字符串" 文件名`
        其中`-r`表示递归查找，`-n`表示显示文字在对应文件中的行号 
        其余选项：
        `-w`只会匹配那些作为独立单词出现的字符串，而不会匹配那些作为其他单词一部分的字符串
      4. 案例
        `grep "abc" *`
        在当前目录下的所有文件中查找包含字符串 "abc" 的行，并显示这些行。
      5. sd
   5. ### dd
        1. `dd` 是一个用于在 Unix 和 Unix-like 操作系统中进行低级别数据复制和转换的命令行工具。它可以从一个文件或设备读取数据，并将其写入另一个文件或设备。`dd` 常用于以下任务：
            - 创建磁盘镜像
            - 备份和恢复整个磁盘或分区
            - 生成特定大小的文件
            - 数据格式转换
        2. 案例`dd if=/dev/zero of=test bs=1024 count=1024`
            - if=/dev/zero：输入文件（input file），这里指定为 /dev/zero，它是一个特殊文件，提供无限的零字节。
            - of=test：输出文件（output file），这里指定为 test，即将生成的文件名。
            - bs=1024：块大小（block size），这里指定为 1024 字节。
            - count=1024：块数（count），这里指定为 1024 个块。
        这个命令会创建一个大小为 1MB（1024 字节 * 1024 块）的文件 test，并用零字节填充它。 
        3. 
   6. ### 压缩相关
        1. gzip
        2. bzip2
        3. tar 
            `tar` 是一个用于创建和操作压缩文件的命令行工具。它通常用于将多个文件和目录打包成一个单一的压缩文件，或者从压缩文件中提取文件。以下是一些常用的 `tar` 命令选项及其含义：
            
            | 选项 | 含义 |
            |------|------|
            | `-c` | 创建一个新的压缩文件 |
            | `-x` | 从压缩文件中提取文件 |
            | `-v` | 显示详细信息 |
            | `-f` | 指定压缩文件的名称 |
            | `-z` | 使用 gzip 压缩压缩文件 |
            | `-j` | 使用 bzip2 压缩压缩文件 |
            | `-t` | 列出压缩文件的内容 |
            | `-r` | 向现有压缩文件中追加文件 |
            | `-u` | 仅追加比压缩文件中副本更新的文件 |
            | `-A` | 将一个或多个压缩文件追加到现有的 tar 压缩文件中 |
            | `-W` | 验证压缩文件的完整性 |
            | `--delete` | 从压缩文件中删除文件 |
            | `--exclude` | 排除指定的文件或目录 |
            | `-C` | 切换到指定目录 |
            | `--strip-components` | 提取时剥离指定数量的路径前缀 |
            
            #### 示例
            
            1. 创建一个新的 gzip 压缩文件：
                ```sh
                tar -czvf archive.tar.gz directory/
                ```
                - `-c`：创建一个新的压缩文件
                - `-z`：使用 gzip 压缩
                - `-v`：显示详细信息
                - `-f`：指定压缩文件的名称
                - `archive.tar.gz`：压缩文件的名称
                - `directory/`：要压缩的目录
            
            2. 从 gzip 压缩文件中提取文件：
                ```sh
                tar -xzvf archive.tar.gz
                ```
                - `-x`：从压缩文件中提取文件
                - `-z`：使用 gzip 解压缩
                - `-v`：显示详细信息
                - `-f`：指定压缩文件的名称
                - `archive.tar.gz`：压缩文件的名称
            
            3. 列出 gzip 压缩文件的内容：
                ```sh
                tar -tzvf archive.tar.gz
                ```
                - `-t`：列出压缩文件的内容
                - `-z`：使用 gzip 解压缩
                - `-v`：显示详细信息
                - `-f`：指定压缩文件的名称
                - `archive.tar.gz`：压缩文件的名称
          
    7. ### 网卡相关
        1. ping
            
            `ping` 是一个网络命令行工具，用于测试主机之间的连通性。它通过发送 Internet 控制消息协议（ICMP）回显请求数据包，并等待回显应答来测量往返时间。以下是一些常用的 `ping` 命令选项及其含义：
            
            | 选项 | 含义 |
            |------|------|
            | `-c` | 指定发送的请求数 |
            | `-i` | 设置发送每个数据包之间的间隔时间（以秒为单位） |
            | `-s` | 设置发送数据包的大小 |
            | `-t` | 设置数据包的生存时间（TTL） |
            | `-W` | 设置等待每个回复的时间（以秒为单位） |
            
            #### 示例
            
            1. 向 `example.com` 发送 4 个 ICMP 回显请求：
                ```sh
                ping -c 4 example.com
                ```
                - `-c 4`：发送 4 个请求
                - `example.com`：目标主机
            
            2. 设置数据包大小为 100 字节并向 `example.com` 发送请求：
                ```sh
                ping -s 100 example.com
                ```
                - `-s 100`：设置数据包大小为 100 字节
                - `example.com`：目标主机
            
            3. 设置每个数据包之间的间隔时间为 2 秒并向 `example.com` 发送请求：
                ```sh
                ping -i 2 example.com
                ```
                - `-i 2`：设置间隔时间为 2 秒
                - `example.com`：目标主机
            
            [`ping`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2FD%3A%2F%E4%B8%AA%E4%BA%BA%E5%90%84%E7%A7%8D%E5%AD%A6%E4%B9%A0%E7%BB%8F%E5%8E%86%E6%80%BB%E7%BB%93%2FLinux%2FREADME.md%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A142%2C%22character%22%3A11%7D%7D%5D%2C%22f6427ddb-ff3d-43a3-9127-450b40f9dfb6%22%5D "Go to definition") 命令通常用于诊断网络问题，例如检查网络是否连通、测量网络延迟等。
        2. ifconfig config
    8. sd
      

------------

## 参考资料
[韦东山Linux嵌入式课程](https://www.bilibili.com/video/BV1w4411B7a4?p=7&spm_id_from=pageDriver&vd_source=909872ff2170ce36790dcfcfae588d2f)