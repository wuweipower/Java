# 下载安装

https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation 

许可证：ZF3R0-FHED2-M80TY-8QYGC-NPKYF

# Order

shutdown 关机

sync 把内存中的 数据写入磁盘

## 目录管理

cd  ：change direcory 转目录 绝对路径以/开头，

- cd ~ 回到当前用户目录

- ./ ：当前目录
- cd.. ：返回上一级 例子../usr 上一个目录下的usr目录

ls : 列出目录

- -a : all 全部文件，包括隐藏的
- -l : 列出所有文件，权限和属性。不包括隐藏
- 可以组合 -a -l

mkdir : 创建目录

- mkdir -p test1/test2/test3 创建多级目录

pwd ： 显示当前目录

rm : remove 文件或者目录

- -f : 忽略不存在的文件，强制删除
- -r 递归删除目录
- -i 互动，询问是否删除
- rm -rf / ：系统中所有的文件都被删除了

rmdir -p test1/test2/test3 删除多级目录

cp : copy 复制文件

- cp 原来 目标

mv 移动文件（原来，目标）  还可以重命名（旧的名字，新的名字）

- -f 强制
- -u 只替换已经更新过的文件

# 目录

/ 根目录，就是一个/

/bin binary 最经常使用的命令

/boot 启动linux的一些核心文件

/etc 所有系统管理所需要的配置文件和子目录

/home 用户的主目录，每个用户都有自己的目录

/lib 最基本的动态连接共享库，类似DDL

/mnt 用户临时挂在别的文件系统的

/opt 给主机额外安装软件的目录

/proc  系统信息

/dev device

/root 系统管理

/usr 用户很多应用程序和文件都放在这个目录下，类似program files

/tmp temp

/sbin super user 系统管理员使用的系统管理程序

/usr/bin 应用程序

/usr/sbin

/usr/src 内核源码

/var 这个目录存放着不断扩充着的东西

/run 临时文件系统，储存系统启动以来的信息，重启后自动删掉

/www 存放服务器网站相关的资源，环境，项目之类的

# 基本属性

ll 或者 ls -l 命令显示一个文件的属性以及文件所属的用户和组

第一个字母为

- d 表示为目录
- \- 表示文件
- l 表示链接文档 link file
- b 表示装配文件里面的可供储存的接口设备
- c 表示装配文件里面的串行端口设备，如鼠标键盘

rwx ：read write execute

没有权限则为- 如 r-x

| 0        | 123      | 456      | 789          |
| -------- | -------- | -------- | ------------ |
| 文件类型 | 属主权限 | 属组权限 | 其他用户权限 |
| d        | rwx      | r-x      | e-x          |

l 快捷方式



chgrp : change group

chown : change owner

==chmod== 更改文件的9个属性 也就是上面的123456789

- r:4 w:2 x:1
- owner = rwx = 4+2+1 = 7
- group = rwx = 4+2+1 = 7
- pthers = --- = 0+0+0 = 0
- chmod 770 filename



# 文件内容查看

cat 从第一行开始看

tac 倒着看

nl 显示的时候，输出行号

more 一页一页的显示文件内容 空格键翻页 enter向下看一行 :f 行号

less 于more相似但是可以往前翻页 上下键翻页 推出用==q==命令

head 只看头几行 

- -n 后面的参数可以表示看几行

tail 只看尾巴几行

ifconfig

man [命令]



进入文件内容后查找字符串 ==/字符串== 向下查询 ？向上查询 n 继续搜索下一个N表示向上



# 硬软链接

硬链接 ： 允许一个文件有多个路径 保险

软链接 ： 快捷方式

touch 创建文件

ln 硬链接

ln -s 软连接

echo 

echo  "i am TH" >>filename 输入字符串



# VIM

命令模式

刚进入就是命令模式

- i 变成编辑模式 insert 模式
- x 删除光标所在的字符
- : 切换为底线命令模式 在最低一行输入命令 如果是编辑模式则先退出 esc键

输入模式

- DEL 刹车农户光标后一个字符
- backspace 删除光标前一个
- 方向键 移动光标
- home / end 行首/行尾
- pageUp /pageDown 上下翻页
- insert 切换光标为输入/替换模式 光标变成竖线/下划线
- esc 退出输入模式，切换为命令模式

底线命令模式

q 推出

w 保持

wq 保存并推出



# 用户管理

**添加用户 useradd**

useradd -选项 用户名

- -m ：自动创建这个用户主目录 home/username

etc/passwd 下面就是用户的信息

**删除用户**

userdel -r username 删除用户的时候，把目录也删除

**修改用户**

usermod 

- -d 改目录

**切换用户**

#表示超级用户

$普通用户

su 用户名   切换用户名

exit 就可以退回来原来的用户

**改主机名字**

hostname 新名字

查看就是hostname

**加密码**

passwd 用户名

**锁定用户**

passwd -l username      l就是lock

passed -d username      d就是delete没有密码也登不进去



# 用户组

其实就是对etc/group文件的更新

**创建一个用户组**

groupadd name

指定id

groupadd -g id号 name 不指定id号就自增

**删除**

groupdel

**修改**

groupmod 

- -n 新名字 改名字
- -g 数字 改id号

把真正加密后的用户口令存放在etc/shadow



# 磁盘

df 列出文件系统

- -f 就可以把字节换成mb

du检查磁盘空间使用量

- -a 全部文件

du -sm /*

mount 挂载

umount 卸载 -f强制卸载



# 进程

**ps 查看当前系统正在执行的各种进程信息**

- -a 显示当前终端运行的所有进程信息
- -u 以用户的信息显示进程
- -x 显示后台运行进程的参数
- 在linux | 一个竖线叫做管道符
- grep 查找文件中符合条件的字符串

ps -aux|grep mysql

ps -ef 可以查看父进程信息

pstree

- -p 显示父id
- -u 显示用户组

**杀进程**

kill -9 进程id



PID process id

# 环境

安装软件：rpm、 解压缩、 yum在线安装

## JDK

rpm -ivh rpm包

rpm -qa|grep jdk 版本号

rpm -e --nodeps 卸载

配置环境

/etc/profile

里面加入JAVA_HOME = 目录

CLASSPATH=%JAVA_HOME%lib;%JAVA_HOME%/jre

PATH=$JAVA_HOME/lib

和Windows是一样的

防火墙

firewall -cmd --list-ports



## 打包和运行jar

用maven package jar包

然后java -jar jar包名字就可以运行了



## 解压

tar -zxvf 压缩包

具体要看bin目录

启动 ./start.sh

停止 ./shutdown.sh



## yum











