title: 如何在多台Linux服务器间自动远程执行各种命令
date: 2015-11-09 10:55:36
categories: Shell
tags:
---
> 应用分布在多台服务器上在带来高可用性的同时，也给软件开发测试人员带来了些许难题，根据错误分析日志、执行脚本等操作往往需要依次登录各个服务器才能完成，如果能够在自己的开发环境上自动登录多台服务器并执行命令或脚本、上传下载文件，将会提高工作效率。

<!--more-->
## Linux开发者
利用expect脚本实现自动地和交互式任务进行通信

```bash
#告诉操作系统脚本里的代码使用哪一个shell来执行
#!/usr/bin/expect -f 
set port 22
set user root
set host 192.168.28.30
set password root

#设置超时时间，单位秒  
set timeout -1 
#spawn是进入expect环境后才可以执行的expect内部命令                        
spawn ssh -D $port $user@$host "ifconfig" 
#根据提示自动发送yes和密码 
expect {                                   
"*yes/no" { send "yes\r"; exp_continue}
"*password:" { send "$password\r" }
}
expect "*#*"

#远程执行命令
send "ifconfig > /home/cfg \r"             
send "exit\r"
interact
spawn scp $host:/home/cfg ./
expect {
"*yes/no" { send "yes\r"; exp_continue}
"*assword:" { send "$password\r" }
}
expect eof
```

## Java开发者
使用基于Java的远程执行类库[SSHxcute](http://www.ibm.com/developerworks/cn/linux/1412_baoxh_java/index.html) 
> 貌似只能发送一次命令，所以可以用&&把几条命令连接起来。

## Python开发者
[paramiko](http://pythonpeixun.blog.51cto.com/7195558/1213929)是用python语言写的一个模块，遵循SSH2协议，支持以加密和认证的方式，进行远程服务器的连接。
使用paramiko可以很好的解决以下问题：1、使用windows客户端；2、远程连接到Linux服务器，查看上面的日志状态，批量配置远程服务器，文件上传，文件下载等。