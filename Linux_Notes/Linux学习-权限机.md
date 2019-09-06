# Linux学习-权限机制

## 文件权限

|   权限    | 对文件的影响       | 对目录的影响           |
| :-------: | ------------------ | ---------------------- |
| r（读取） | 可读取文件内容     | 可列出目录内容         |
| w（写入） | 可修改文件内容     | 可在目录中创建删除文件 |
| x（执行） | 可以作为命令执行难 | 可访问目录内容         |

## UGO

- U代表user，G代表group，O代表other
- 每一个文件的权限基于UGO进行设置
- 权限三个一组（rwx），对应UGO分别设置
- 每一个文件拥有一个所属用户和所属组，对应UG，不属于该文件所属用户或所属组的使用O权限

**命令ls -l可以查看当前目录下文件的详细信息**

drwxr-xr-- 2 nash_su training 208 Oct 1 13:50 linuxcast

- drwxr-xr--  : UGO 权限
- 2: 链接数量
- nash_su  :  所属用户U
- training：所属组G
- 208 ： 大小
- Linuxcast ： 文件名

drwxr-xr--  

- d：文件类型
- rwx ： U权限
- r-x：G权限
- r-- ： O权限

## 修改文件所属用户、组

chown/chgrp + 组/用户 + 文件/文件夹

- chown修改文件所属用户：chown nash_su linuxcast.net 

  -R参数 递归的修改目录下所有文件的所属用户

- chgrp修改文件所属组：chgrp nash_su linuxcast.net 

  -R参数 递归的修改目录下所有文件的所属组

## 修改权限

chmod 模式 文件 ：可以修改文件的权限

模式如下：

- u、g、o分别代表用户、组、和其他
- a可以代指ugo
- +、-代表加入或删除对应权限
- r、w、x代表三种权限

模式用例：

- chmod u+rw linux
- chmod g-x linux
- chmod go +r linux 
- chmod a-x linux

命令chmod也支持用数字方式修改权限，三个权限分别由三个数字表示：

- -r = 4 （2^2）
- -w = 2（2^1）
- -x = 1（2^0）

使用数字表示权限时，没组权限分别对应数字之和：

- rw = 6
- rwx = 7
- r-x = 5

所以使用数字表示ugo权限使用如下方式表示：

- chmod 660 Linux：  rw-rw----
- chmod 775 Linux：  rwxrwxr-x