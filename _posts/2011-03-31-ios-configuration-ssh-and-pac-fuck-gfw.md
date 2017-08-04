---
layout: post
title:  "iOS配置SSH/PAC翻墙"
date:   2011-03-31 21:22:49 +0800
categories: css
---
我的测试环境:

iphone 3GS 4.1 港版 JB by limera1n

1.通过cydia安装OpenSSH(必须)和MobileTerminal , 无MobileTerminal可以使用其他的ssh工具, 如iSSH, pTerm, SSH-Terminal, 破解版可从apptrackr.org上获得, 以下以iSSH 4.5.1为例, 因为我的MobileTerminal有点问题.(当然iSSH的功能不限于本文,功能很强悍的说).

2.一个可用的ssh帐号,免费的不好找,收费的大概不到10RMB每月.

3.配置iSSH, 增加一个configuration, Host连接本机:127.0.0.1, port默认就好, login用root, 密码就是你的密码, 其他全部默认. 如图

4.Save之后主界面就会出现刚刚添加的bob. 点击它就可以进入terminal界面.

5.输入命令 `ssh -D 7070 用户名@主机名`, 如要指定端口命令结尾加上 -p 端口号.

如果不想每次输入这长串命令可以新建个扩展名字sh的文件,文件权限755(命令: chmod 755 文件), 里面填写那段命令,每次执行这个文件就行了,不过密码还是要输入的.当然也有办法记住密码

6.Home键离开iSSH, 配置pac. 设置->WiFi->WiFi info->Http 代理->自动. 然后URL中填写pac文件的地址. 如果文件放在本机,地址可以为file://private/var/root/iphone.pac,扩展名无所谓.

PAC文件可以留下email我发给你,或者猛击[这里](https://skydrive.live.com/?cid=DE6A2DF8B8EA9D15&id=DE6A2DF8B8EA9D15%21120&sc=documents# "iphone.pac")(iphone.pac). 也可以从[这里](http://autoproxy2pac-alfa.appspot.com/)获得,不过这里的pac是被base64encode过的,据我测试不能直接用在iphone上,需要decode后使用

奇怪的是我的路径里private得修改成pravate,不然不起作用,其实根本没有这个目录,很奇怪求解释.

URL也可以是”http://***.com/*.pac”.

7.打开你的youtube, twitter, facebook吧.
