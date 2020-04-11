##### 最佳实践
1、定义数组时, ==元素按行书写==  
2、如果遇到带==有空格的元素,用引号引起来==  
3、for循环中使用"${list[@]}",会将带引号的行看成一个元素  
4、==不同的shell的处理方式不同==,这里只考虑了bash

- @/*(不带引号时), 总是按空格或者换行拆分元素,
- "@" 带引号时,  数组定义中引号将会影响拆分元素
- "*" 带引号时,  数组会被识别成一个元素
- ==\* 不推荐使用==,  @完全可以替代*工作

##### 从三个方面考虑  
- array的自身处理方式
- array传递到shell后shell对array的处理方式(shell有无""包裹)
- 对于[@]、[*],array自身传递给shell的方式不一样  


```
list=(
13580561176      # 这是注释
"123     456"    # 分为带引号 和不带引号两种
888      999
)

echo ${#list[@]} ==> 4
echo ${#list[@]} ==> 4
echo ${#list[*]} ==> 4
echo ${#list[*]} ==> 4

for m in  ${list[@]};  do echo $m; echo '***********'; done # 5
echo '------------------------'
for m in "${list[@]}"; do echo $m; echo '***********'; done  # 数组定义元素不带引号:5个元素; 带引号:4个元素
echo '------------------------'
for m in  ${list[*]};  do echo $m; echo '***********'; done # 恒为5个元素
echo '------------------------'
for m in "${list[*]}"; do echo $m; echo '***********'; done  # 恒为1个元素
```

```
[root@dockersvr ~]# list=(
> 13580561176      # 这是注释
> "123     456"    # 分为带引号 和不带引号两种
> )
[root@dockersvr ~]# 
[root@dockersvr ~]# echo ${list[@]}
13580561176 123 456  # 3个元素
[root@dockersvr ~]# echo "${list[@]}"
13580561176 123     456    # 两个元素(制表符依然存在)
[root@dockersvr ~]# echo ${list[*]}  
13580561176 123 456   # 3个元素
[root@dockersvr ~]# echo "${list[*]}"
13580561176 123     456
[root@dockersvr ~]# for i in "${list[*]}";   #被当成了一个元素
> do
> echo $i
> done
13580561176 123 456
[root@dockersvr ~]# for i in 13580561176 "123     456"; do echo $i; done 
13580561176
123 456
[root@dockersvr ~]# for i in 13580561176 123     456; do echo $i; done  
13580561176
123
456
[root@dockersvr ~]# 

```