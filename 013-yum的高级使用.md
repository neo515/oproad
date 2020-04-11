#### 利用yum下载rpm包
`yum -y install --downloadonly --downloaddir=/tmp/ lrzsz `

- 默认保存位置 /var/cache/yum/x86_64/(find查找吧)
- 需要安装yum-plugin-downloadonly yumdownloader vlock (centos7默认自带)
- 需要另外安装 yum-utils

#### 常用命令
`yum localinstall pptpd-1.3.4-2.rhel5.i386.rpm --nogpgcheck` #不检查签名  
`yum deplist libcurl`   #获取依赖信息  
`yum remove \*.i\?86`   #通配

#### 查询yum中指定rpm包含的文件
`repoquery -q -l bind-utils`
- 类似于rpm -ql,但是无需先安装该包
- 该工具需要另外安装 yum-utils




#### yum update和upgrade的区别
首先yum update和yum upgrade的功能类似的，都是将需要更新的package更新至软件源中的最新版。

唯一不同是：yum upgrade会删除旧版本的package，而yum update则会保留。

注意！==如果你的某些软件依赖旧版本的package，请使用yum update。==
