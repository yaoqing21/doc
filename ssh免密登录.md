# ssh免密登录

让一台主机可以免密登录多台`remote host`，只需要保证主机上有一对公钥和私钥，`remote host`上有对应的公钥即可。并且密钥对只需要生成一次。

**ssh免密登录原理**

![20220310164206](https://pic-1304668038.cos.ap-nanjing.myqcloud.com/img/20220328211605.jpg)

1. 首先需要保证主机和`remote host` 上都有`ssh`应用程序并且处于开启状态。
2. 用`ssh-keygen -t rsa`命令，一路回车，会在用户目录下的`.ssh`目录中生成`id_rsa`(私钥)和`id_rsa.pub`(公钥)。主机想要登录到`remote host`上就需要把私钥放在主机上，公钥复制到`remote host`上。
3. 在remote host上将`id_rsa.pub`的内容追加至`authorized_keys`文件里，使用命令` cat id_rsa.pub >> authorized_keys`。没有`authorized_keys`文件时，需要自己创建。
4. 使用`ssh -v 主机名@ip地址`来验证连接过程，这个命令会将连接的整个过程打印出来。主机名可以用`whoami`得到，`ip`地址可以通过`ifconfig`命令查询到。

**总结：**一台主机登录多台`remote host`只需要一对秘钥就可以啦，保证主机上持有私钥，公钥复制到远程主机的`authorized_keys`文件里。`ssh`连接`github`也是同样如此，将本机的公钥放到你的`GitHub`账号里面的`ssh key`就可以了，不需要多次生成。