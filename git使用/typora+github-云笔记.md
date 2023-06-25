## 1、下载typora

下载地址：<font color='bule'>https://www.typora.io/</font>

## 2.git下载及安装

以上两个步骤具体见教程：https://github.com/yusenyi123/notebook/blob/master/Typora%E4%BD%BF%E7%94%A8/Typora%2Bgithub-%E4%BA%91%E7%AC%94%E8%AE%B0%E6%9C%AC1.0.md

## 3.构建本地仓库

进入待作为笔记仓库的目录，使用该命令初始化仓库

~~~shell
git init
~~~

然后在远程仓库中创建master分支，然后与本地分支建立联系

~~~shell
git remote add <远端名称> <仓库路径>//自己复制
git remote add origin git@gitee.com:czbk_zhang_meng/git_test.git
~~~

如果在远端分支创建时做了修改，如生成了readme文件，一定要先执行如下命令同步到本地，然后再往远端提交本地仓库

~~~shell
git pull --rebase origin master//同步远程仓库与本地库

git push origin master
~~~

## 4.照片单独存放

使用PicGo把图片上传到图床的软件，typora现在的版本可以和这个该软件配合轻松自动上传

![image-20230625154501985](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230625154501985.png)

PicGo+Github使用教程：https://blog.csdn.net/yefcion/article/details/88412025