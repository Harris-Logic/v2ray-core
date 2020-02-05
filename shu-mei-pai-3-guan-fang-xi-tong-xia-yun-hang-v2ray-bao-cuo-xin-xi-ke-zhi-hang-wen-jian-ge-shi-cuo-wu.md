# 树莓派3 官方系统下运行v2ray，报错信息可执行文件格式错误。

## 树莓派3 官方系统下运行v2ray，报错信息，求解答 \#526

运行环境是树莓派3官方提供的系统，干净的，首次运行r2ray。  
r2ray的版本是v.2.33 ,下载的是v2ray-linux-arm64.zip  
下载完成后，unzip解压缩，  
赋权chmod +x v2ray  
然后准备运行  
./v2ray -config=config.json  
结果是错误信息  
-bash: ./v2ray: cannot execute binary file: Exec format error  
不知道是什么原因，求解答，谢谢。

错误信息表示可执行文件格式错误。 树莓派官方系统似乎是 32 位的。请尝试 32 位 ARM 版本。

#### 

