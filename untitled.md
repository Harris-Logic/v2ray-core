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

