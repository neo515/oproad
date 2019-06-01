#### #1 centos添加官方svn yum源
```bash
#支持centos6/7, svn1.7-1.11
#相应修改对应的url即可
vim /etc/yum.repos.d/wandisco-svn.repo
[WandiscoSVN]
name=Wandisco SVN Repo
baseurl=http://opensource.wandisco.com/centos/6/svn-1.7/RPMS/$basearch/   # 相应修改系统版本、svn版本号
enabled=1
gpgcheck=1
```
```bash
#导入rpm签名
wget http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco
gpg --quiet --with-fingerprint ./RPM-GPG-KEY-WANdisco     #检查签名文件,可以不做
rpm --import ./RPM-GPG-KEY-WANdisco
```
