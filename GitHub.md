# GitHub

## 一、**版本控制**

### 1.为什么用？

```txt
查看以往历史；团队协作；

协同修改；版本管理（文件系统快照）；

数据备份；权限控制；分支管理
```

### 2.分类

```txt
集中式分布控制工具：CVS；SVN；VSS（服务器挂，数据丢失；保存在服务器，单点故障）

分布式版本控制工具：Git；Mercurial;Bazzar；Darrs（本地进行存储，避免单点故障）
```

优势：

- 大部分操作在本地完成，不需要联网

- 完整性保证

- 尽可能添加数据而不是删除和修改数据

- 分支操作非常快捷流畅

- 与Linux命令全面兼容

-------------------------------------------------------------------------------

工作区---->写代码      git add

暂存区----->临时存储    git commit

本地库----->存放历史版本

### 3.代码托管中心 ：维护远程库

局域网环境下：Gitlab服务器

外网环境下：GitHub；码云

#### 3.1本地库和远程库

团队内：

![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/1537.jpg) 

 

跨团队协作：

![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/1538.jpg) 

 

## 二、**Git命令行操作（和Linux命令兼容）**

### 2.1本地库初始化   

命令：git add

注意：git目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改

查看文件内容：$ cat good.txt

### 2.2设置签名

形式：用户名： tom

Email地址： [goodmorning@atguigu.com](mailto:goodmorning@atguigu.com)

作用：区分不同开发人员身份

辨析：这里设置的签名和登录远程库（代码托管中心）的账号，密码没有任何关系

### 2.3命令

**项目级别/仓库级别**：仅在当前本地库范围生效

```yaml
git config  user.name tom_pro

git config user.Email [goodmorning_pro@atguigu.com](mailto:goodmorning_pro@atguigu.com)

信息保存位置：  ./git/config 文件        
$ cat ./git/config
```

**系统用户级别**：登录当前操作系统的用户范围

```yaml
git config global  user.name tom_glb

git config global  user.Email  [goodmorning_glb@atguigu.com](mailto:goodmorning_pro@atguigu.com)

信息保存位置：~/.gitconfig 文件       $ cat ~/.gitconfig
```

优先级：

**就近原则**：

- 项目级别优先与系统用户级别，二者都有时采用项目级别的签名，

- 如果只有系统用户级别的签名，就以系统用户级别的签名为准，

- 二者都没有不允许

### 2.4查看状态（添加，提交）

```yaml
状态查看：$ git status 查看工作区，暂存区状态
编辑文件：$vim good.txt
添加：$ git add  [file name] 将工作区的“新建，修改”添加到暂存区
---> $ git commit
```

#### 2.4.2

```yaml
$ git commit -m “My Second commit modify good.txt(修改提示相关信息)” good.txt

$ git commit -m “commit message” [file name]
```

### 2.5查看版本前进后退

查看历史记录：

```yaml
$ git log
$ git log  - - pretty =oneline 
$ git log - - oneline （缩短了hash值显示）
$ git reflog        显示回退步数)
```

HEAD @ 移动到当前版本需要多少步

多屏显示控制方式：空格键向下翻页

B  向上翻页

Q   退出

#### 2.5.2

```yaml
前进、后退   操作HEAD
基于索引值：$ git reset - - hard [索引值]   （hash值）
使用^符号：只能后退    
$ git reset -- hard HEAD^^^ 
注：一个^ 退一步  二个^退两步 ，三个退 三步

使用~符号：（只能后退） 
$ git reset - -hard HEAD ~3  后退三步
$ git  reset - - hard HEAD~n 后退n步
```

#### 2.5.3 soft，hard，mixed（reset命令的三个参数对比）

```yaml
Hard：在本地库移动HEAD指针；重置暂存区；重置工作区

Mixed：在本地库移动HEAD指针；重置暂存区

Soft：仅仅在本地库移动HEAD指针

$ git reset - - soft [索引值]
```

 Soft:![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/1539.jpg)

Mixed:![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/1536.jpg)

 

### 2.6删除文件  $ rm aaa.txt

```txt
提交到暂存区，未到本地库： 
$ rm apple.txt  -----> $ git add apple.txt ----> $ git status---->$git reset - - hard HEAD

前提：删除文件存在时的状态，提交到了本地库

操作： $ git reset -- hard[指针位置]

删除操作已经提交到了本地库：指针位置指向历史记录

删除操作未提交到本地卡库：指针位置使用HEAD
```

### 2.7比较文件   $ git diff  apple.txt[文件名]

```txt
 将工作区中的文件和暂存区比较
 $ git diff HEAD [文件名] 

将工作区的文件与本地库历史记录比较
```

**注**：不带文件名比较多个文件

### 2.8分支概述 ：复制 master  hot_fix

![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/1535.jpg) 

#### 2.8.2分支的好处:同时并行推进多个功能开发，提高开发效率

         各个分支在开发的过程中，如果某一个分支开发失败，不会对其他分支有任何影响，失败的分支删除后重新开始即可

### 2.9 分支的操作

```yaml
查看:$ git branch -v
创建：$ git branch [分支名]
切换：$ git checkout [分支名]
合并：
	①切换到接受修改的分支（被合并，增加新内容）上 git checkout[被合并分支]
	②执行merge命令   
$ git merge [有新内容分支名]
```

解决冲突：手动解决
	①编辑文件：删除特殊符号
	②修改完毕，保存退出
	③$ git add [文件名]
	④提交时：$ git commit “日志信息”     （不带文件名）

### 3.Git基本原理

#### 3.1 哈希：

	①结果固定

	②算法不可逆 

	③ 输入数据确定时，输出确定，输入数据不确定时，输出结果变化很大

**Git底层采用SHA-1 算法**

#### 3.2保存版本机制

##### 3.2.1   SVN 集中式版本控制   以文件变更列表方式存储信息

Git 分布式版本控制   小型文件系统的一组快照（提交对象及其父对象形成链条）

#### 3.3分支管理

![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/1535.jpg) 

![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/1535.jpg) 

### 4.本地库和远程库交互

```txt
 团队内协作；跨团队协作
	$ git remote add origin 远程库地址
	
 推送：$ git push origin master ( 需要填账号，密码)

 克隆：$ git clone  地址（远程库）

完整的复制到本地仓库；创建origin 远程地址别名；初始化本地库

拉取：pull = fetch +merge

$ git fetch origin master [远程库地址别名] [远程分支名]

$ git merge origin/mater [远程库地址别名] [远程分支名]

$ git pull  origin  master
```

拉取：pull = fetch +merge

$ git fetch origin master       [远程库地址别名][远程分支名]

$ git merge origin/mater       [远程库地址别名]/[远程分支名]

$ git pull  origin  master

协同开发时冲突：

要点：1.如果不是GitHub远程库最新版所做的修改，不能推送，必须先拉取

2.拉取后，如果进入冲突按照“分支冲突解决”方式解决即可

### 5.SSH登录：只有一个账号，推送采用SSH

```yaml
进入当前用户目 : $ cd~
删除 .ssh 目录： $ rm -rrf  .ssh
运行命令生成 .ssh目录： $ ssh-keygen -t  rsa - C  邮箱地址
进入 .ssh 目录查看文件列表：$ cd .ssh  ----> $ Is -IF
查看 id_rsa.pub 内容：$ cat  id_rsa.pub
复制id_rsa.put文件内容，登录GitHub ,点击用户头像------>Setting---->SSH and GPG----->

New SSH key
输入复制的秘钥信息--->回到Gitbash 创建远程地址别名

$ git remote add origin_ssh [git@github.com:atguigu2018ybuq/huashan.git](mailto:git@github.com:atguigu2018ybuq/huashan.git)

推送文件进行测试
```

## 三、Git图形化界面操作

### 1.Eclipse 工程初始化到本地库

工程 --->右键    Team --- share Project  --- git 

   git  repository settin  设置email ，name

  Label Decorations   标签说明

### 2.Eclipse 中忽略文件

     概念：Eclipse 特定文件

 	这些都是Eclipse为了管理我们创建的工程而维护的文件，和开发代码没有直接关系

.classpath文件

.project 文件

.setting 目录下所有文件 

#### 2.2为什么要忽略Eclipse特定文件呢？

 同一个团队中很难保证使用同样的IDE工具，不同工具的特定文件不同。

在~/.git config 文件中引入上述文件

[core] 

 Excludes=C:/Users/lenovo/java.gitignore

Target / .classpath /.project/ .setting/

注意：这里的路径一定要使用“/”，不能使用“\”

### 3.基本操作：

team---->Add to index ----Commit ----> git Staging ----->commit

 推送到远程库：切换到远程库账号

Eclipse 中：Team----> Remote --->push   (复制地址)

  Add  All  Branch spec

### 4.Oxygen Eclipse 克隆操作

导入工程  Import --->Git ---->clone URI  (local Destination 选择工作区目录)

 Import as  general  project

   Configure --->convert to maven project

### 5.Kepler Eclipse 克隆操作

      不同点：不能保存在当前eclipse工作区目录

   制定另一个工作区以外的目录

### 6.在eclipse中解决冲突  merge Tool

先pull ---->  解决冲突点  ------> commit

### 7. Git 工作流

       概念：在项目开发工程中使用Git方式

#### 7.2分类

集中式：以中央仓库作为项目所有修改的单点实体

GitFlow：未功能开发，发布准备和维护设置了独立的分支

ForKing：在GitFlow基础上，使用pull request 和fork

#### 7.3分支实战

   Team-----switch----other---remote tracking---hot_fix

---check out as   new branch

合并分支：切换为master     switch  to master

        Team ----->merge

## 四、Gitlab服务器环境搭建

https://www.cnblogs.com/shijunjie/p/8961977.html

