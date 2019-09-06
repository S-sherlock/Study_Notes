# Linux学习-基本命令和操作

## 日期时间
- date查看、设置当前系统时间
    - 格式化显示时间：+%Y--%m--%d
- hwclock(clock)显示硬件时钟时间
- cal查看日历
- uptime查看系统运行时间

## 输出、查看命令
- echo显示输入的内容
- cat显示文件内容
- head显示文件的头几行（默认10行）
    - -n 指定显示的行数
- tali显示文件的末尾几行（默认10行）
    - -n 指定显示的行数
    - -f 追踪显示文件更新（一般用于查看日志，命令不会退出，而是持续显示新加入的内容）
- more 用于翻页显示文件内容（只能向下翻）
- less 用于翻页显示文件内容（带上下翻页）

## history（了解）
- history  ： 查看历史命令。
- !!     ： 重复前一个命令。
- ! + 字符 ： 重复前一个以“字符”开头的命令。
- ! + num  ： 按照历史记录的序号执行命令。
- ! + ?abc ： 重复之前包含abc的命令。

## sudo
- su - : 切换root用户。
- sudo ：使用管理员身份运行命令。
- passwd：修改密码。

## ctrl + z
- Ctrl + z ： 暂停某个程序
- jobs： 管理后台作业。
- bg + 序号：把程序拉回后台。
- fg + 喜好：把程序拉回前台。
- 后台运行程序在命令后面加一个&。

## 文件管理
- touch：创建一个空白文件或者更新已有文件的时间。
- ls -a ： 显示隐藏文件。
- ls -l ： 显示详细信息。
- ls -R ： 递归显示子目录结构。
- ls -ld 文件夹名： 显示目录和链接信息。
- file+文件名/文件夹名 ：查看文件/文件夹的属性。
- cp 文件/文件夹 ： 把文件复制到文件夹下。
    - cp -r ：复制文件夹。
- mv ： 移动文件夹/文件。
    - mv 文件 文件夹 ： 把文件移动到文件夹。
    - mv 文件 文件夹/新名字 ： 移动到文件夹并重命名。
    - mv 文件名 新名字 ： 不制定目录移动，相当于重命名。
- rm ： 删除文件/文件夹。
    - -i ： 交互式删除。
    - -r ： 递归的删除目录中的所有内容。
    - -f ： 强制删除（谨慎使用）。

## 查看硬件信息

- lspci 查看PCI设备
  - -v查看详细信息

- lsusb 查看USB设备
  - -v查看详细信息

- lsmod 查看加载的模块（驱动）

## 关机、重启

- shutdown 关闭、重启计算机
  - -r 重新启动
    - 立即重启：shutdown -r now
  - -h 关机 
    -  立即关机 shutdown -h now
    - 10分钟后关机 shutdown -h +10
    - 23:30 关机 shutdown -h 23:30

- powerful 立即关闭计算机
- reboot 立即重启计算机

## 归档、压缩

- zip 压缩文件
  - zip linux.zip myfile

- unzip 解压文件
  - unzip linux.zip
- gzip 压缩文件
  - gzip linux.txt
- tar 归档文件
  - tar -cvf out.tar（归档后的名字） linux（要归档的文件夹）
  - tar -xvf linux.tar （释放一个归档文件）
  - tar -cvzf backup.tar.gz /etc
    - z参数将归档后的归档文件进行gzip压缩以减少大小

## 查找

- locate 快速查找文件/文件夹，locate keyword
  - 此命令需要预先建立数据库，数据库默认每天更新一次，可用update命令手动建立、更新数据库
  - 优点查找速度快，缺点新加进来的东西可能查不到
- find 用以高级查找文件、文件夹。
  - find .-name 星号filename星号，.代表在当前目录下
  - find / -name *.conf ：在根分区内查找以conf结尾的文件
  - find / -perm 777  ： 查找所有权限是777的文件
  - find / -type d ： 查找所有d文件类型的文件
  - find . -name "a*" -exec ls -l {} \;    ： 查找所有以a开头文件之后 执行 ls -l 命令。