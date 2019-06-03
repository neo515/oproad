- 默认sshd服务的日志保存为 ==/var/log/secure==

#### 修改sshd服务配置文件
- 1.修改sshd_config
```bash
vim /etc/ssh/sshd_config
#SyslogFacility AUTH
#SyslogFacility AUTHPRIV
#LogLevel INFO
SyslogFacility  local1
```
- 2.更改日志服务配置rsyslog.conf
```bash
# vim /etc/rsyslog.conf
local1.* /var/log/sshd.log
```

- 3.重启日志服务和sshd服务
```bash
systemctl restart  rsyslog
systemctl restart  sshd
```