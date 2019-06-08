rpm -qpc xxx.rpm  #查看rpm包的配置文件，没有就不显示  
rpm -qpR xxx.rpm  #查看依赖关系


#rpm包签名文件   
rpm -q gpg-pubkey    #查询已导入的gpg  
rpm -e --allmatches gpg-pubkey-xxxx    #删除所有         

#rpmdb数据库
rpmdb --initdb  #初始化rpm数据库  
rpmdb --rebuilddb #从已安装的包头文件反向重建rpm数据库

#rpmdb数据库修复  
rm -f /var/lib/rpm/__db*
rpm -vv --rebuilddb
