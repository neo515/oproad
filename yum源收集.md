-----
##### docker-ce源
```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo   #可能无法访问
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo #替代官方yum源

```
----
##### kubernetes源(aliyun)
```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```

----
##### epel源
```bash
yum install epel-release   # 如果当前系统的源没有epel包就需另下载
http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm  #centos7
http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm  #centos6
yum repolist      #检查是否已添加至源列表
```

----
##### mysql源
```bash
https://dev.mysql.com/downloads/repo/yum/   #打开页面下载对应系统的rpm包
rpm -ivh mysql57-community-release-el6-11.noarch.rpm 
```

----
##### openstack源
```bash
[openswitch]
name= openswitch
baseurl=https://repos.fedorapeople.org/openstack/EOL/openstack-icehouse/epel-6/
enabled=1
gpgcheck=0
```

----
##### svn源
#支持centos6/7, svn1.7-1.11
#相应修改对应的url即可
```bash
vim /etc/yum.repos.d/wandisco-svn.repo
[WandiscoSVN]
name=Wandisco SVN Repo
baseurl=http://opensource.wandisco.com/centos/6/svn-1.7/RPMS/$basearch/
enabled=1
gpgcheck=1
```

----
##### 高版本内核kernel
```bash
http://elrepo.org/tiki/tiki-index.php
https://github.com/jiafeicat/follow-me-install-kubernetes-cluster/blob/v1.8.x/01.%E7%B3%BB%E7%BB%9F%E5%88%9D%E5%A7%8B%E5%8C%96%E5%92%8C%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F.md
```