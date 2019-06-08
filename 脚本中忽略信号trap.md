格式:trap "command" signal1 signal2 ...  #command中可以加入变量。
>忽略信号   #放到脚本的最开始

```bash
trap '' INT            #将无法用kill -2、ctrl+C 来杀掉进错
trap "echo '忽略-2、-15信号'" INT TERM  #无法被kill杀掉,但可以被-9信号杀掉

```

shell输出伪信号
- EXIT  #从函数中退出或者脚本执行完毕
- ERR   #命令执行执行不成功
- DEBUG #脚本每一条命令执行前执行,可用于脚本测试,跟踪脚本执行情况。
```bash
trap "echo $aa" DEBUG  #扑捉DEBUG,用于脚本调试
trap 'echo "before execute line:$LINENO,a=$a,b=$b,c=$c"' DEBUG
```
