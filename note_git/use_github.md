# git

*记录一些git命令*

### git 撤销本地修改

[Git 丢弃本地修改](https://www.jianshu.com/p/565306500575)

git checkout -- path

### git 将本地项目和线上仓库绑定

```git
git init
git add .
git commit -m "备注"
git remote add origin 远程仓库地址
git push -u origin master
```

+ 这样之后会出现两个分支，很难受

```git
git checkout main
git merge master --allow-unrelated-histories
git push
```

+ 这样就合并了两个分支但是并没有删除master分支，我们可以去github的页面上删除

**参考**

[git 将本地项目关联到远程仓库](https://blog.csdn.net/zzzzlei123123123/article/details/97622912)



### git ignore

新增.gitinore文件

#### 如何让新增加的 .gitignore 文件生效

PS:  新增 .gitignore 文件后，若该操作未能实现忽略提交，是因为.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。

使用以下命令使其生效

```git
git rm -r --cached .  #清除缓存!!最后有个点
git add . #重新trace file
git commit -m "update .gitignore" #提交和注释
git push origin master #可选，如果需要同步到remote上的话
```



### 生成自定义名字的ssh密钥

```
$ ssh-keygen -t rsa -C “your email” -f id_rsa_xx
```





##### 参考

[详解gitignore的使用方法，让你尽情使用git add .](https://www.cnblogs.com/techflow/p/13801136.html)

[关于idea的gitignore[文件](https://www.eolink.com/news/tags-985.html)编写及解决ignore文件不生效问题](https://www.eolink.com/news/post/41562.html)