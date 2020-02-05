# Mac OSX终端走shadowsocks代理



`shadowsocks`设置为：

* 打开`shadowsocks`
* 自动代理模式
* 服务器（香港阿里云）

以`zsh`作为说明

```text
➜  ~ vim ~/.zshrc  
```

添加如下代理配置:

```text
# proxy list
alias proxy='export all_proxy=socks5://127.0.0.1:1080'
alias unproxy='unset all_proxy'
```

`:wq`保存退出

```text
➜  ~ source ~/.zshrc
```

使用`proxy`前先查看下当前的`ip`地址：

```text
➜  ~ curl ip.cn
当前 IP：112.64.xxx.xx 来自：上海市 联通
```

或者

```text
~ curl cip.cc
IP	: 140.206.97.42
地址	: 中国  上海

数据二	: 上海市 | 联通

URL	: http://www.cip.cc/140.206.97.42
```

执行:

```text
➜  ~ proxy
➜  ~ curl ip.cn
当前 IP：47.89.xx.xxx 来自：香港特别行政区 阿里云
```

如果ip.cn不能用，可以换个类似的站点查询

```text
~ curl cip.cc
IP	: 45.78.47.19
地址	: 美国  加利福尼亚

数据二	: 美国 | 加利福尼亚州洛杉矶市 IT7 Networks

URL	: http://www.cip.cc/45.78.47.19
```

没问题，终端走了代理，`brew update`顺畅了- -

如果不需要走代理，执行：

```text
➜  ~ unproxy   
➜  ~ curl ip.cn
当前 IP：112.64.xxx.xx 来自：上海市 联通
```

### proxychains-ng

```text
➜  ~ brew install proxychains-ng
Updating Homebrew...
```

由于`OSX`升级后的`SIP`限制，在`proxychains.conf`文件中设置`ss`的`socks5`代理，无效了。解决办法是在重启后，在`Recovery mode`下关闭`SIP`，但对于强迫症来说，不能忍\(安全问题\)。详见  
[rofl0r/proxychains-ng/issues/78](https://github.com/rofl0r/proxychains-ng/issues/78)

```text
➜  ~ proxychains4 curl ip.cn
[proxychains] config file found: /usr/local/Cellar/proxychains-ng/4.12/etc/proxychains.conf
[proxychains] preloading /usr/local/Cellar/proxychains-ng/4.12/lib/libproxychains4.dylib
当前 IP：112.64.xxx.xx 来自：上海市 联通
```

配置文件`/usr/local/Cellar/proxychains-ng/4.12/etc/proxychains.conf`:

```text
 111 [ProxyList]
 112 # add proxy here ...
 113 # meanwile
 114 # defaults set to "tor"
 115 #socks4     127.0.0.1 9050
 116 socks5  127.0.0.1 1080
```

**更新2017.07.21**

`osx`下使用`brew`安装`google-chrome`时：

```text
% brew cask install google-chrome
==> Satisfying dependencies
==> Downloading https://dl.google.com/chrome/mac/stable/GGRO/googlechrome.dmg

curl: (6) Could not resolve host: dl.google.com
Error: Download failed on Cask 'google-chrome' with message: Download failed: https://dl.google.com/chrome/mac/stable/GGRO/googlechrome.dmg
Error: Install incomplete.
```

通过设置`terminal`的`http`代理解决：

```text
% export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087;
```

**更新**

我们可以在shadowsocks server端查看日志：

```text
sudo tail -f /var/log/shadowsocks.log
```

日志如下图：

[![](https://raw.githubusercontent.com/mrdulin/pic-bucket-01/master/20190823010510.png)](https://raw.githubusercontent.com/mrdulin/pic-bucket-01/master/20190823010510.png)

用其中一条日志做说明：

```text
2019-08-22 17:02:14 INFO     connecting mtalk.google.com:5228 from 58.247.150.247:61603
```

含义是：时间戳 日志级别 connecting 连接的目标服务器URL地址 from 客户端地址:端口

客户端地址就是我本机了，通过本机启动的shadowsocks客户端连接到shadowsocks服务端。

查看本机外网IP:

```text
☁  quick-start [master] ⚡  curl ip.gs
Current IP / 当前 IP: 58.247.150.247
ISP / 运营商:  ChinaUnicom
City / 城市: Shanghai Shanghai
Country / 国家: China
IP.GS is now IP.SB, please visit https://ip.sb/ for more information. / IP.GS 已更改为 IP.SB ，请访问 https://ip.sb/ 获取更详细 IP 信息！
Please join Telegram group https://t.me/sbfans if you have any issues. / 如有问题，请加入 Telegram 群 https://t.me/sbfans 

  /\_/\
=( °w° )=
  )   (  //
 (__ __)//
```

Current IP: 58.247.150.247，和shadowsocks服务端日志打印出来的客户端地址一致

参考：

* [https://wiki.archlinux.org/index.php/Proxy\_server](https://wiki.archlinux.org/index.php/Proxy_server)
* [https://zh.wikipedia.org/wiki/SOCKS](https://zh.wikipedia.org/wiki/SOCKS)





#### 

#### [@mrdulin](https://github.com/mrdulin) 执行 `proxy` 之后, `curl ip.cn` 的结果: curl: \(7\) Failed to connect to 127.0.0.1 port 1080: Connection refused在前面加上 sudo 后, 获取的 ip 还是本地运营商的. 这是怎么回事呢? [@Huang-Libo](https://github.com/Huang-Libo) 请确保本地代理服务器的地址和端口是`127.0.0.1:1080` 请打开ss,进行代理服务器设置 我试了下，退出ss，就是你这个错误，因为本地代理服务器没有启动。 [@Ericva](https://github.com/Ericva) 1.本地的ss是客户端，你的VPS上的ss是服务端，客户端的服务器配置中，地址和端口填写你VPS的ip地址和ss服务端运行后的端口号，这样客户端就连接到你的vps的ss服务了。 [![image](https://user-images.githubusercontent.com/17866683/33050928-6d40fe3a-cea2-11e7-8584-f818ce4c5689.png)](https://user-images.githubusercontent.com/17866683/33050928-6d40fe3a-cea2-11e7-8584-f818ce4c5689.png) ss客户端启动后，可以修改客户端服务的端口号，不一定是1080，使用switchomega，proxifier等代理切换软件时，指定ss的ip和端口是客户端的。 [![image](https://user-images.githubusercontent.com/17866683/33050878-34187c28-cea2-11e7-9dcb-2f21cc7046ca.png)](https://user-images.githubusercontent.com/17866683/33050878-34187c28-cea2-11e7-9dcb-2f21cc7046ca.png) 2.你的服务器配置ip或者端口不正确，客户端ss没有链接到VPS上的ss服务端，或者是你VPS上的ss服务端没有启动或是出了问题。如下图，我随便输入了一个ip和端口，执行proxy后，结果显示“curl: \(52\) Empty reply from server”[![](https://camo.githubusercontent.com/b566c22f8df19b9240016c94fcf37779a7753e03/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b665463677931666c70686a797a7238666a33307773307438676c782e6a7067)](https://camo.githubusercontent.com/b566c22f8df19b9240016c94fcf37779a7753e03/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b665463677931666c70686a797a7238666a33307773307438676c782e6a7067) 3.proxychains-ng和通过在`.zshrc`或者`.bashrc`中指定代理`alias`是两种终端走代理的方式，没有依赖关系。



代理已经能显示外国的地址，但是ping [www.google.com](http://www.google.com/) 的时候还是ping不通，请问知道为什么吗  
[@liushazm](https://github.com/liushazm)  
ss代理是基于tcp或者udp协议，而ping是走的icmp协议因此在ss下不能ping通google  
参考:  
[https://stackoverflow.com/questions/5274934/use-ping-through-socks-server](https://stackoverflow.com/questions/5274934/use-ping-through-socks-server)

