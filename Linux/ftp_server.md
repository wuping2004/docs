# FTP Server

步骤一： 安装 vsftpd

远程连接并登录到 Linux 实例。

运行以下命令安装 vsftpd。

yum install -y vsftpd
图片20

出现下图表示安装成功。

图片21

运行以下命令打开及查看etc/vsftpd。

cd /etc/vsftpd
ls
安装 vsftpd

说明：

/etc/vsftpd/vsftpd.conf是核心配置文件。

/etc/vsftpd/ftpusers 是黑名单文件，此文件里的用户不允许访问 FTP 服务器。

/etc/vsftpd/user_list是白名单文件，是允许访问 FTP 服务器的用户列表。
运行以下命令设置开机自启动。

systemctl enable vsftpd.service
运行以下命令启动 FTP 服务。
systemctl start vsftpd.service
运行以下命令查看 FTP 服务端口。

netstat -antup | grep ftp
启动

步骤二： 配置 vsftpd

vsftpd 安装后默认开启了匿名 FTP 的功能，使用匿名 FTP，用户无需输入用户名密码即可登录 FTP 服务器，但没有权限修改或上传文件。

文本介绍了以下几个配置 vsftpd 的方法以及相关的参数说明，您可以根据具体需要进行参考。

配置匿名用户上传文件权限
配置本地用户登录
vsftpd.conf 的配置文件参数说明
配置匿名用户上传文件权限

修改 vsftpd.conf 的配置文件的选项，可以赋予匿名 FTP 更多的权限。

修改/etc/vsftpd/vsftpd.conf：

运行vim /etc/vsftpd/vsftpd.conf。

按键 “i” 进入编辑模式。

将写权限修改为write_enable=YES。

将匿名上传权限修改为anon_upload_enable=YES。

按键 “Esc” 退出编辑模式，然后按键“：wq” 保存并退出文件。

配置文件

运行以下命令更改 /var/ftp/pub 目录的权限，为 FTP 用户添加写权限，并重新加载配置文件。

chmod o+w /var/ftp/pub/
systemctl restart vsftpd.service
目录权限

配置本地用户登录

本地用户登录就是指用户使用 Linux 操作系统中的用户账号和密码登录 FTP 服务器。

vsftpd 安装后默只支持匿名 FTP 登录，用户如果试图使用 Linux 操作系统中的账号登录服务器，将会被 vsftpd 拒绝，但可以在 vsftpd 里配置用户账号和密码登录。具体步骤如下：

运行以下命令创建 ftptest 用户。

useradd ftptest
运行以下命令修改 ftptest 用户密码。

passwd ftptest
修改 ftptest 用户密码

修改/etc/vsftpd/vsftpd.conf：

运行vim /etc/vsftpd/vsftpd.conf。

按键 “i” 进入编辑模式。

将是否允许匿名登录 FTP 的参数修改为anonymous enable=NO。

将是否允许本地用户登录 FTP 的参数修改为local_enable=YES。

按键 “Esc” 退出编辑模式，然后按键“：wq” 保存并退出文件。

修改/etc/vsftpd/vsftpd.conf

vsftpd.conf 的配置文件参数说明

运行命令 cat /etc/vsftpd/vsftpd.conf 查看配置文件内容。

用户登录控制

参数	说明
anonymous_enable=YES	接受匿名用户
no_anon_password=YES	匿名用户login时不询问口令
anon_root=(none)	匿名用户主目录
local_enable=YES	接受本地用户
local_root=(none)	本地用户主目录
用户权限控制

参数	说明
write_enable=YES	可以上传(全局控制)
local_umask=022	本地用户上传文件的umask
file_open_mode=0666	上传文件的权限配合umask使用
anon_upload_enable=NO	匿名用户可以上传
anon_mkdir_write_enable=NO	匿名用户可以建目录
anon_other_write_enable=NO	匿名用户修改删除
chown_username=lightwiter	匿名上传文件所属用户名
步骤三： 设置安全组

搭建好 FTP 站点后，您需要在实例的安全组的入方向添加一条放行 FTP 端口的规则，具体步骤参见 添加安全组规则，具体配置可以参考 安全组规则的典型应用_FTP。
