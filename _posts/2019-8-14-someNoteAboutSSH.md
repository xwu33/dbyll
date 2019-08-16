---
layout: post
title: Some Notes about SSH and AWS
categories: [Internet, SSH]
tags: [SSH]
fullview: false
comments: false
shortinfo: This page is going to mark some useful information about the SSH incase I forgot someday.
---
#### Connecting the AWS service using BASH on Windows
When connecting AWS using BASH, we should notice that the bash program should be run as administrator otherwise it will deny the connection and report 0555 code.

#### Root of Ubuntu on Windows
The Root of Ubuntu on Windows will be located in "C:\Users\scao7\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs\root"

#### using shadowsocks set up vpn
```bash
# 获取root权限
sudo -s
# 更新apt-get
apt-get update
# 安装 python 的 包管理工具 pip
apt-get install python-pip
# 安装shadowsocks
pip install shadowsocks
# 创建 shadowsocks 目录
mkdir /etc/shadowsocks
# 创建并编辑我们的配置文件
vim /etc/shadowsocks/config.json
```
##### wirte json file
```json
{
# 配置服务器 ip，这里填写 0.0.0.0
"server":"0.0.0.0",
# 配置端口号及各自的密码
# 这里我用的多用户配置，大家也可以采用单用户配置
# 端口号如果不懂可以选择跟我相同的，密码修改为自己想设置的密码就好
"port_password":{
     "20610":"MII**********QEA"
     },
# 超时时间 这个不用修改
"timeout":300,
# 加密算法
"method":"aes-256-cfb",
# true 或者 false 开启后能降低延迟 建议开启
"fast_open":false
}
```
##### shadowsocks command
```bash
# 启动
ssserver -c /etc/shadowsocks/config.json -d start
# 停止
ssserver -c /etc/shadowsocks/config.json -d stop
# 重新启动
ssserver -c /etc/shadowsocks/config.json -d restart
```
```bash
#check if server running
ps -aux | grep ssserver
```

##### Change the AWS security group
Add two more rule to set the port to what you set and the souce from 0.0.0.0/0 and ::/0

#### Best way to connect AWS on windows
Using bash on windows to connect the AWS will have a lot of problems the best way is to use Putty to connect the a AWS instance. It must be a instance with a .pem file. First convert the .pem file to .ppk file. uplade the .ppk file under SSH -> Auth. Second copy the DNS form AWS and paste to the host name. and open it. It should works fine.

#### Ubuntu 18.04 solving the problem of shadowsocks start error
如官网中所描述，这是由于在openssl 1.1.0中废弃了 EVP_CIPHER_CTX_cleanup() 函数而引入了 EVE_CIPHER_CTX_reset() 函数所导致的：
EVP_CIPHER_CTX was made opaque in OpenSSL 1.1.0. As a result, EVP_CIPHER_CTX_reset() appeared and EVP_CIPHER_CTX_cleanup() disappeared. EVP_CIPHER_CTX_init() remains as an alias for EVP_CIPHER_CTX_reset().
因此，可以通过将 EVP_CIPHER_CTX_cleanup() 函数替换为 EVP_CIPHER_CTX_reset() 函数来解决该问题。具体解决方法如下：
1、根据错误信息定位到文件 /home/feng/.local/lib/python3.6/site-packages/shadowsocks/crypto/openssl.py 。
2、搜索 cleanup 并将其替换为 reset 。
3、重新启动 shadowsocks, 该问题解决。
