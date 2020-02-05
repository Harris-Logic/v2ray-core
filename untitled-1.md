# Untitled

### Linux下Nodejs安装（完整详细） 原

[顽  顽Shi](https://my.oschina.net/blogshi) 发布于 2014/05/05 23:08 字数 454 阅读 32.9W 收藏 72 点赞 14 [ 评论 19](https://my.oschina.net/blogshi/blog/260953#comments)

    很久之前安装过windows下以及Mac下的node，感觉还是很方便的，不成想今天安装linux下的坑了老半天，特此记录。  


    首先去官网下载代码，这里一定要注意安装分两种，一种是Source Code源码，一种是编译后的文件。我就是按照网上源码的安装方式去操作编译后的文件，结果坑了好久好久。  


![](http://static.oschina.net/uploads/space/2014/0505/225758_u2gO_723632.png)

    注意看好你下载的是什么文件！！！对应的安装方式不同啊，亲。  


（一） 编译好的文件

    简单说就是解压后，在bin文件夹中已经存在node以及npm，如果你进入到对应文件的中执行命令行一点问题都没有，不过不是全局的，所以将这个设置为全局就好了。  


```text
cd node-v0.10.28-linux-x64/bin
ls
./node -v
```

    这就妥妥的了，node文件夹具体放在哪，叫什么名字随你怎么定。然后设置全局：  


```text
ln -s /home/kun/mysofltware/node-v0.10.28-linux-x64/bin/node /usr/local/bin/node
ln -s /home/kun/mysofltware/node-v0.10.28-linux-x64/bin/npm /usr/local/bin/npm
```

    这里/home/kun/mysofltware/这个路径是你自己放的，你将node文件解压到哪里就是哪里。  


（二）通过源码编译

    这种方式你下载的文件是Source code，我不太喜欢这种方式。。。主要是麻烦  


```text
#  tar xvf node-v0.10.28.tar.gz 
#  cd node-v0.10.28 
#  ./configure 
# make 
# make install 
# cp /usr/local/bin/node /usr/sbin/ 

查看当前安装的Node的版本 
# node -v 

v0.10.28
```

（三）apt-get  


    还有一种就是shell提示的apt-get方式，我之前就被这种方式坑了。。。强烈不推荐啊  


```text
sudo apt-get install nodejs
sudo apt-get install npm
```

    这么装完你会发现,node命令好使，nodejs命令可以用。。。

