---
```bash
# cat getserviceconfpath
#!/bin/bash
#用于获取服务的service文件的路径
svrname=$1
systemctl status $svrname |grep 'Loaded:' |awk -F'[;(]' '{print $2}'
```

---
#### 运行级别
```bash
#1 更改运行级别
systemctl set-default  < xxx.target >     # 更改系统默认启动方式
systemctl set-default  multi-user.target  # 图形界面
systemctl set-default  graphical.target:  # 终端界面

#2 查看运行级别
systemctl get-default

#3 切换到运行级别3
systemctl isolate multi-user.target
systemctl isolate runlevel3.target
ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target  # 默认启动运行级别3

#4 切换到运行级别5
systemctl isolate graphical.target
systemctl isolate runlevel5.target   
ln -sf /lib/systemd/system/graphical.target  /etc/systemd/system/default.target  # 默认启动运行级别5

#5 系统默认各运行级别设定
ls -Xl /lib/systemd/system/runlevel*.target
/lib/systemd/system/runlevel0.target -> poweroff.target
/lib/systemd/system/runlevel1.target -> rescue.target
/lib/systemd/system/runlevel2.target -> multi-user.target
/lib/systemd/system/runlevel3.target -> multi-user.target
/lib/systemd/system/runlevel4.target -> multi-user.target
/lib/systemd/system/runlevel5.target -> graphical.target
/lib/systemd/system/runlevel6.target -> reboot.target
```

---
#### centos7下的实现
```bash
chkconfig sshd on   #chkconfig依然可以使用.但是被转发
Note: Forwarding request to 'systemctl enable sshd.service'.
ln -s '/usr/lib/systemd/system/sshd.service' '/etc/systemd/system/multi-user.target.wants/sshd.service'   # 原理

systemctl disable sshd.service  # 关闭ssh
rm '/etc/systemd/system/multi-user.target.wants/sshd.service'   

```

---
#### 服务管理
任务         | 旧指令                   | 新指令
---          | ---                      | ---
列出服务     | chkconfig #7下也可用(sysV) | systemctl list-unit-files\|grep enabled
树形列出     | 　　                      | systemd-cgls    
启动一个服务 | service postfix start  | systemctl start   postfix.service
关闭一个服务 | service postfix stop   | systemctl stop    postfix.service
重启一个服务 | service postfix restart| systemctl restart postfix.service
开机启动     | chkconfig --level 3 httpd on  |systemctl enable  httpd.service
不开机启动   | chkconfig --level 3 httpd off |systemctl disable httpd.service
是否开机启动 | chkconfig --list postfix      |systemctl is-enabled postfix.service
状态         | service httpd status          |systemctl status httpd.service （服务详细信息） 
　　　       | 　　　　                       |systemctl is-active httpd.service（仅显示是否 Active)
当前服务状态(而非配置状态) |　　　　   　　      | systemctl list-units --type=service
服务的配置状态             |chkconfig --list |systemctl list-unit-files --type=service\|grep enable
