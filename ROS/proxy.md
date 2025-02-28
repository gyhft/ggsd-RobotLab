#### 1.下载

下载好Clash for Linux,
https://github.com/zhaoweih/Clash-Copy

然后解压，点击进去，输入命令./start.sh

再输入 source /etc/profile.d/clash.sh

再输入proxy_on

这样，就开启代理了。

#### 2.开启全局代理

使用HTTP代理，输入命令

```C++
sudo gedit ~/.bashrc

//  输入这个，保存
export  http_proxy= http://127.0.1.1:7890
export  https_proxy= http://127.0.1.1:7890
export no_proxy=127.0.0.1,localhost

//最后
source ~/.bashrc

```


