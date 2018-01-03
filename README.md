# dockerized cowrie

一、项目介绍
[cowrie](http://www.micheloosterhof.com/cowrie/) 是基于[kippo](https://github.com/desaster/kippo).的中级交互蜜罐。
此项目是将cowrie制作成docker镜像，包含定制该蜜罐的配置文件以及build docker镜像的Dockerfile。
此项目作为多蜜罐项目 **[Multi-Honeypots]** 的一个蜜罐部件。

二、威胁数据

Cowire蜜罐捕获的攻击主要是SSH 密码猜解，远程命令执行，以及捕获到的一些攻击过程中使用的样本和脚本。
所有的log和样本都将保存在/data/cowrie/ 目录下。
日志记录了攻击的全貌，主要以json格式输出至 /data/cowrie/log/cowrie.json，记录内容如下：

cowrie.session.connect：     有连接进来记录session,src_ip,sensorid timestamp
cowrie.login.success：       登陆成功记录session,usr,pwd,timestamp
cowrie.login.failed:         登陆失败记录session，usr,pwd,timestamp
cowrie.command.success:      成功执行命令记录session,timestamp,input
cowrie.command.failed:       失败执行命令记录session,timestamp,input
cowrie.session.file_download:下载文件记录session,timestamp,url,outfile,shasum
cowrie.session.file_upload:  上传文件记录
                             session,timestamp,url,outfile,shasum
cowrie.session.input：       输入记录session,timestamp,realm,input
cowrie.client.version:       version
cowrie.client.size:          更新client size update termsize(width height)
cowrie.session.closed：      关闭连接记录endtime的timestamp
cowrie.log.closed：          记录tty log session ttylog size
cowrie.client.fingerprint:   指纹溯源 session username fingerprint

捕获的恶意样本以哈希值重命名保存至 /data/cowrie/downloads
为避免磁盘空间占用过大，多蜜罐系统默认24小时重启一次，且重启后清除
/data/下的数据，并在清除之前将所有数据导入到已部署的MySQL数据库中，
样本打包发送至FTP server或SMB server中

附在公网上90天cowrie捕获到的威胁数据统计报告dashboard
![Cowrie Dashboard](https://raw.githubusercontent.com/douwanhu/docker-cowrie/master/doc/dashboard.png)

