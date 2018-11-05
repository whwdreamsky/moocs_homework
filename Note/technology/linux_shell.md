Linux Shell

###Linux 常用命令

1. gcc 编译c 语言，当使用链接库时，直接使用gcc *.c 会出现错误，此时使用

   ```shell
   gcc *.c -lm #链接库
   ```

2. 查看磁盘使用

   ~~~shell
   # 文件夹 磁盘使用
   du -h
   # 当前文件夹下文件大小
   du -ah
   # 系统磁盘使用
   df -h
   ~~~

3. 给terminal 划分多个区域

   - terminator

     short cut

     - Toggle fullscreen: F11.
     - Split terminals horizontally: Ctrl + Shift + O.
     - Split terminals vertically: Ctrl + Shift + E.
     - Close current Panel: Ctrl + Shift + W.
     - Open new tab: Ctrl + Shift + T.
     - Move to the terminal above the current one: Alt + ↑
     - Move to the terminal below the current one: Alt + ↓

   - tmux




### shell 编程



### add ssh key

1. creat RSA key pair

   ```

   ```

   ​


### 为linux 添加用户

~~~shell
ssh root@server_ip_address
adduser username
usermod -aG sudo username		
~~~



### Awk|Sed:常用文本处理工具，一定要学习

- 删除列

  awk '{\$1="";\$2="";print $0}'  data_1000.txt.data > t1

- 删除行首空格或制表符
  sed 's/^[ \t]*//g' 

  sed 's/^[ \t]$//g' 

- ​

