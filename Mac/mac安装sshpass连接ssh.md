## mac安装sshpass连接ssh

**安装背景：**  ssh连接远程服务器居然要输入密码，每次都要输入感觉很难受，为了偷懒，所以安装sshpass明文自动输入密码登录



### 安装sshpass

试图使用homebrew安装
> brew install sshpass  
> Error: No available formula with the name "sshpass"  
> We won't add sshpass because it makes it too easy for novice SSH users to
> ruin SSH's security.

发现并不可以，没办法，使用homewbrew强制安装
> brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb

好了，到这里就安装完成了，编辑.bash_profile文件设置别名即可
> sshpass -p '123456' ssh username@xxx.xxx.xxx.xxx

