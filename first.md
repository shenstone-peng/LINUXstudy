# 热键：
热键              |作用
---|:--:
[Tab]             |连续按两次补全命令
[Ctrl]+c          |中断当前程序
[Ctrl] +d         |键盘输入结束/exitls 显示当前目录
# 命令
命令               |作用
---|:--:           
exit       		   |注销当前登录
date               |显示当前日期和时间
cal                |显示日历
bc                 |计算器
--help             |求助说明
man                |操作说明
ls                 |是list的意思 -al参数表示列出所有的文件详细的权限与属性
chgrp              |修改文件所属用户组
chown              |修改文件拥有者
chmod              |修改文件的权限
cp                 |复制文件
cd                 |切换目录
pwd                |显示当前目录
mkdir              |创建新目录
rmdir              |删除一个空目录
cp                 |复制文件或目录
rm				   |删除文件或目录
mv				   |移动文件或目录，或重命名
cat/tac/nl 	       |concatenate
/more/less		   |查看文件
/head/tail		   |查看文件
od                 |查看非纯文本文件
touch              |修改文件时间或创建新文件
file               |观察文件类型
which        	   |查找path内所设置的目录中的可执行文件
locate/whereis/find|查找文件







cat /etc/klinux-release

利用ls -l显示的文件属性中，第一个字段是文件的权限，共有十个位，第一个位是文件类型，接下来三个为所属用户、用户组和  
其他用户的（读、写与执行）三个权限。  

---------------------------------------------------------
FHS依据文件系统使用的**频繁与否**与**是否允许用户随意修改**，而将目录定义为四种交互作用的形态。

          | shareable               |unshareable
       ---|:--:|----:
  static  |/usr（软件存放处）       |/etc（配置文件）
          |/opt（第三方辅助软件）   |/boot（启动与内核文件）
  variable|/var/mail（用户邮箱）    |/var/run（程序相关）
          |/var/spoo;/news（新闻组）|/var/lock（程序相关）
		  
		  
-----------------------------------------------------------------------
# 第一层目录（/）
/bin : 存放执行文件。放置的是在单人文虎模式下还能被使用的命令（cat/chmod/chown/date/mv/mkdir/cp/bash等）

/boot : 放置启动会使用到的文件

/dev : 设备接口

/etc ： 系统主要的配置文件几乎都放置在这个目录内。 e.g. 人员的账号密码文件、*各种服务的启动文件*

/lib&/lib64 : 分别放置不同格式的二进制函数库。

/sbin ： 放在sbin下面的文件为启动过程中所需要的，里面包含了启动、修复、还原系统所需要的命令。（fdisk/fsck/ifconfig/mkfs等）

-----------------------------------------------------------------------
# 第二层目录（/usr）
/usr/bin & /usr/lib & /usr/sbin : 根目录中的bin，lib,sbin存放的是链接文件，链接至此.

-----------------------------------------------------------------------
# 第三层目录（/var）
/var/lib :存放程序执行时，需要使用到的数据文件放置的目录。

-----------------------------------------------------------------------------------------------
**绝对路径** :由根目录开始写起的文件名或目录

**相对路径** ：相对于当前目录的文件名写法。
   - (.):代表当前的目录，也可以使用（./）表示。
   - (..):代表上一层目录，也可以使用（../）表示。

   
-------------------------------------------------------------
# LINUX运行启动流程（SysVinit系统）
系统加电------>硬件自检-------->bootload对系统的初始化，加载内核--------->内核决定对哪些设备驱动进行初始化-  
----->内核挂装根文件系统，初始化设备驱动程序和数据结构------->启动init进程-------->读取/etc/inittab文件中  
initdefault id值------->根据运行级别选择进入/etc/rc.d/文件夹中的子目录。

# LINUX运行启动流程（Systemd系统）
系统加电------>硬件自检-------->bootload对系统的初始化，加载内核--------->内核决定对哪些设备驱动进行初始化-  
----->内核挂装根文件系统，初始化设备驱动程序和数据结构------->启动init进程-------->读取/usr/lib/systemd/system/  
default.target文件中获取默认的运行级别信息------->根据运行级别选择进入/usr/lib/systemd/system/文件夹中的.target
（一般是符号链接到graphical.target"图形“或multi-users.target
> ps:
> 打开/etc/inittab并查看内容，写的是inittab已经不再被systemd系统使用，systemd系统现在使用target文件代替runlevel，
> multi_user.targer:analogus to runlevel 3   |  graphical.target:analogous to runlevel 5

--------------------------------------------------------------
介绍Systemd系统：[介绍systemd系统](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html "阮一峰systemd_info")
**vmwatchdogd.service**
### 重点
Unit：systemd可以管理所有系统资源。不同的资源统称为unit。
- 提供status/is-active/is-failed/is-enabled来查询状态
- unit之间存在依赖关系：A依赖于B，就意味着systemd在启动A的时候，同时会启动B。list-dependencies可以查询某unit的所有依赖
- systemd默认从目录/etc/systemd/system/读取配置文件，里面大部分都是符号链接-->/usr/lib/systemd/system/，真正的配置文件放在这个目录。
  systemctl enable 命令用于在上面两个目录之间，建立符号链接关系。
- .service服务配置文件教程[配置服务文件的官方文档](https://www.freedesktop.org/software/systemd/man/systemd.unit.html ".service文件官方配置文档")
- target就是一个unit组，包含许多相关的unit。启动某个target时，systemd就会启动里面所有的unit。
  传统的init启动模式里面，有runlevel的概念，跟target的作用类似。不同点在runlevel是互斥的，不可能多个ruynlevel同时启动，但是多个target可以同时启动


--------------------------------------------------------
### 文件默认属性umask
### 文件隐藏属性chattr
> -a：当设置a后，文件只能增加数据，不能删除也不能修改数据
  
  -i：当设置i后，文件不能把被删除、改名、设置链接也无法写入或新增数据

---------------------------------------------------------------
# 权限与命令间的关系
一、让用户进入某目录成为可工作目录的命令
- 可使用的命令：cd
- 目录所需权限：用户对这个目录至少需要具有x的权限
- 额外需求：如果用户想要在这个目录内利用ls查看文件名，则用户对此目录还需要r权限
二、用户在某个目录内读取一个文件的基本权限是什么？
- 命令：cat、more、less
- 目录所需权限：用户对目录至少需要x权限
- 文件所需权限：用户对文件至少需要r的权限
三、让用户可以修改一个文件的基本权限是什么？
- 命令：nano、vi
- 目录所需权限：用户对目录至少需要x权限
- 文件所需权限：用户对文件至少需要r，w的权限
四、让一个用户可以建立一个文件的基本权限是什么？
- 目录所需权限：用户在该目录要具有w、x的权限，重点在w
五、让用户进入某目录并执行该目录下的某个命令之基本权限是什么？
- 目录所需权限：用户对该目录至少需要x权限
- 文件所需权限：用户对文件至少需要x的权限
----------------------------------------------------------

1、以姓名缩写创建用户，用户主目录为/home/姓名缩写，主用户组名也为姓名缩写
useradd sp
2、在用户主目录下，用相对路径一次性创建 test/test1和test/test2两个目录
mkdir -p test/test1 test/test2
3、从/var/log目录下拷贝一个message文件至新创建的test1目录下
cp /var/log/message /home/sp/test1/messages
4、在test2下创建一个软连接指向test1下的message文件
cp -s /home/sp/test1/messages /home/sp/test2/message.link
5、将test1下的message文件打包压缩成 messgage.tar.gz，并移动到test2目录下
tar -zcv -f ../test2/messages.tar.gz messages
6、删除test1目录
rm -r ./test1
-----------------------------------------------------------------------

