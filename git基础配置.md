```
git config --global user.name "xxxxxx"
git config --global user.email test@example.com

#别名代码直接复制使用即可
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'   # 命令的使用 git unstage fileA
git config --global alias.last    'log -1 HEAD'

git config --global core.autocrlf false   #禁用git自动转换换行符功能; 如果是windows且使用vs编辑器的话将值设置为input
git config --global core.safecrlf true    #不允许提交混合的换行符

git config --global core.quotepath false  #使git st等正常输出中文

// git config -l  # 可以获取上面的所有设置情况
```