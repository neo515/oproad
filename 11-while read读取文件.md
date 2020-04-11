- 方式一： read和exec结合使用。
```bash
exec <FILE 
while read line 
do 
    cmd 
done 
```

- 方式二：
```bash
cat FILE_PATH |while read line 
do 
    cmd 
done 
```

- 方式三：
```bash
while read line 
do 
    cmd 
done <FILE
```
 

举例
```bash
cat  ip.txt
10.1.1.11 root 123
10.1.1.22 root 111
10.1.1.33 root 123456
10.1.1.44 root 54321

写法1：
cat ip.txt | while read ip user pass
do
    echo "$ip--$user--$pass"
done

写法2：
while read ip user pass
do
    echo "$ip--$user--$pass"
done < ip.txt

#使用IFS作为分隔符读文件
#说明：默认情况下IFS是空格，如果需要使用其它的需要重新赋值
例如：
# cat test
chen:222:gogo
jie:333:hehe

# cat test.sh
#!/bin/bash
oldIFS=$IFS
IFS=:
cat test | while read a1 a2 a3
do
    echo "$a1--$a2--$a3"
done
IFS=oldIFS
```