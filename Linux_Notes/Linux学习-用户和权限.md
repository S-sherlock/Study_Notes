# Linux学习-用户基础

## 用户、组

当我们使用linux时，需要以一个用户的身份登录，一个进程也需要以一个用户的身份运行，用户限制使用者或进程可以使用、不可以使用哪些资源。

**组用来方便组织管理用户**

- 每个用户有一个userID，操作系统实际使用的是用户id，而非用户名
- 每个用户属于一个主组，属于一个或多个附属组
- 每个组拥有一个groupID
- 每个进程以一个用户身份运行，并受该用户可访问的资源限制
- 每个可登陆用户拥有一个指定的shell

---

## 用户

- 用户分为以下三类：
  - root用户（ID为0的用户为root用户）
  - 系统用户（1-499）
  - 普通用户（500以上）
- 系统中的文件都有一个所属用户及所属组
- id命令可以查看当前用户的信息
- passwd命令可以修改当前密码
- whoami  显示当前用户
- who  显示哪些用户已经登录系统
- w 显示哪些用户已经登录并且在干什么

### 相关文件

- /etc/passwd   ： 保存用户信息
- /etc/shadow   ： 保存用户密码（加密的）
- /etc/group    ：  保存组信息

### 创建一个用户

- useradd  创建一个新用户  比如： useradd  wangzheng，这个命令会执行以下操作
  1. 在/etc/passwd中添加用户信息
  2. 如果使用passwd wangzheng命令创建密码，则将密码加密保存在/etc/shadow中
  3. 创建一个新的家目录 /home/wangzheng
  4. 将/etc/skel中的文件复制到用户的家目录中（默认都是加密文件，使用ls -a可查看）
  5. 建立一个与用户用户名相同的组，新建用户默认属于这个同名组
- useradd支持以下参数
  - -d  家目录
  - -s 登录shell
  - -u  指定userID
  - -g  指定主组
  - -G 指定附属组

### 修改用户信息

- usermod  用以修改用户信息：usermod  参数  username
  - -l 修改新用户名
  - -u 修改userID
  - -d 用户家目录位置
  - -g 用户所属主组
  - -G 用户所属附属组
  - -L 锁定用户使不能登录
  - -U 解除锁定

### 删除用户

- userdel删除用户
  - userdel username（执行之后在/etc/passwd中看不到用户了但是家目录还在）
  - userdel -r username  （删除家目录）
- 还可以使用rm -rf username手动删除

## 创建、修改、删除组

用法和用户差不多

- groupadd  创建组
- groupmod  修改组信息
  - groupmod  -n newname oldname   修改组名
  - groupmod  -g newGid oldGid         修改组id
- groupdel 删除组