#### 书写格式 
```bash
1.  BEGIN  {  statements  }
2.  END {  statements  }
3.  expression  {  statements  }
4.  /regular  expression1/  {  statements  }
5.  compound  pattern {  statements  }
6.  pattern1 ,  pattern2 {  statements  }

# expressions, with constants, variables, assignments,  function  calls, etc.
print expression-list
printf  (format,  expression-list)
if  (expression)  statement
if  (expression)  statement  else statement
while  (expression)  statement
for  (expression  ;  expression  ;  expression)  statement
for  (variable  in array)  statement
do statement  while  (expression)
break
continue
next
exit
exit expression
{  statements  }
```

#### 传递外部变量 -v
>传入多个变量使用多个-v 
```bash
awk -v host=$HOSTNAME 'BEGIN{print "hostname is: "host}'  ==> hostname is: hellokitty
echo 1,2 |awk -F',' -v OFS='|' '{print $1,$2}'

awk中使用shell中的变量
一："'$var'"
这种写法大家无需改变用'括起awk程序的习惯，是老外常用的写法。如：
var="test"
awk 'BEGIN{print "'$var'"}'  # awk 'BEGIN{print "'       $var       '"}'
这种写法其实际是双括号变为单括号的常量，传递给了awk。
如果var中含空格，为了shell不把空格作为分格符，便应该如下使用：
var="this is a test"
awk 'BEGIN{print "'"$var"'"}'  # awk 'BEGIN{print "'  "$var"      '"}'
二：'"$var"'   #未测试成功
这种写法与上一种类似。如果变量含空格，则变为'""$var""'较为可靠。  awk 'BEGIN{print '"$var"' }'  # awk 'BEGIN{print '   ""$var""      ' }'
三：export变量，使用ENVIRON["var"]形式，获取环境变量的值  
例如：
var="this is a test"; export var;
awk 'BEGIN{print ENVIRON["var"]}'
四：可以使用awk的-v选项 （如果变量个数不多，个人偏向于这种写法）
例如：
var="this is a test"
awk -v awk_var="$var" 'BEGIN {print awk_var}'
这样便把系统变量var传递给了awk变量awk_var。
awk向shell变量传递值
“由awk向shell传递变量”，其思想无非是用awk(sed/perl等也是一样)输出若干条shell命令，然后再用shell去执行这些命令。
eval $(awk 'BEGIN{print "var1='str1';var2='str2'"}')
或者eval $(awk '{printf("var1=%s; var2=%s; var3=%s;",$1,$2,$3)}' abc.txt)
之后可以在当前shell中使用var1，var2等变量了。
echo "var1=$var1 ----- var2=$var2"



```

#### 获取环境变量
awk 'BEGIN { print ENVIRON["USER"] }'  == > root


#### awk脚本  -f scripfile ：
> 将awk执行体放入独立的文件

```bash
$ cat main.awk
/101/{print "\047 Hello! \047"}   #\047代表单引号
{print $1,$2}
{total=$4+$5+$6;
 avg=total/3;
 print $1,avg;}
 
$ echo 1,2 |awk -F',' -v OFS='|'  -f main.awk
```

#### 允许间隔正则表达式的使用,如 [[:alpha:]]
-W re-interval or --re-inerval ：允许间隔正则表达式的使用

#### awk里调用系统命令
```bash
$ echo 1504170547|awk  '{"date -d @\""$1"\" +%F-%T"|getline xxx;print xxx}'
2017-08-31-17:09:07

echo 1464259591**xiaoming**2|awk -F'**' '{IFS="**";OFS="**";"date" " " "-d" " \"@" $1 "\" " "+%F-%T"|getline xxx;print xxx,$2,$3}'  
2016-05-26-18:46:31**xiaoming**2  
```



##### 内置变量
```bash
awk 'BEGIN{print ARGC}' 1 2  ==>3   #参数个数: 'awk' 为第0个参数

awk 'BEGIN{for (i in ARGV) print ARGV[i]}' 12 34   #参数字典,从0开始
  awk  #第0个参数
  12   #第1个参数
  34   #第2个参数


awk '{print ARGIND,$0}' 10.txt 15.txt  #当前文件的位置(从1开始算),将会在每行打印文件位置
awk 'ARGIND==1 {a[$0]} ARGIND>1&&!($0 in a) {print $0}' a b  #查找在b中,但不在a中的行
awk 'ARGIND==1 {a[$0]}  
     ARGIND>1 && !($0 in a) {print $0}' one two   #拆行的写法

ERRNO 最后一个系统错误的描述。
FIELDWIDTHS 字段宽度列表(用空格键分隔)。
FILENAME 当前文件名。
FNR 同NR，但相对于当前文件。
FS 字段分隔符(默认是任何空格)。
IGNORECASE 如果为真，则进行忽略大小写的匹配。
NF 当前记录中的字段数。
NR 当前记录数。
OFMT 数字的输出格式(默认值是%.6g)。
OFS 输出字段分隔符(默认值是一个空格)。
ORS 输出记录分隔符(默认值是一个换行符)。
RLENGTH 由match函数所匹配的字符串的长度。
RS 记录分隔符(默认是一个换行符)。
RSTART 由match函数所匹配的字符串的第一个位置。
SUBSEP 数组下标分隔符(默认值是/034)。

格式化 
awk 'BEGIN{print CONVFMT}' ==> %.6g
awk 'BEGIN {
         printf "CONVFMT=%s, num=%f, str=%s\n", CONVFMT, 12.11, 12.11 }'
==> CONVFMT=%.6g, num=12.110000, str=12.11

awk 'BEGIN {
         CONVFMT="%d";         // 通过更改CONVFMT，我们可以定义自己的转换格式：
         printf "CONVFMT=%s, num=%f, str=%s\n", CONVFMT, 12.11, 12.11 }'  
==> CONVFMT=%d, num=12.110000, str=12


```

#### 三元表达式 
awk '{max = ($1 > $3) ? $1: $3: print max}' test    

#### 正则的写法:
```bash
a:  /re/   #正则写在//中  使用~ , !~,不能用 =、<、>等 ;如：$0 ~ /[0-9]+$/
b: BEGIN  {  digits  =  "A[0-9]+$"  }  $2 ~ digits
 
    BEGIN  {
    sign =  "[+-]?"
    decimal=  "[0-9]+[.]?[0-9]*"
    fraction=  "[.][0-9]+"
    exponent=  "([eE]"  sign "[0-9]+)?"   #空格表示拼接
    number= "^"  sign "("  decimal  "|"  fraction  ")"  exponent  "$"     #拼接了一个很长的正则
    }     $0  ~ number
    
c:  $0 ~ /(\+|-)[0-9]+/   等价于    $0  ~ "(\\+|-)[0-9]+"  


gawk专用正则表达式元字符，不适合unix版本的awk。
（1）\Y ：匹配一个单词开头或者末尾的空字符串。
（2）\B：匹配单词内的空字符串。
（3）\<：匹配一个单词的开头的空字符串，锚定开始。
（4）\> ：匹配一个单词的末尾的空字符串，锚定末尾。
（5）\w ：匹配一个字母数字组成的单词。
（6）\W ：匹配一个非字母数字组成的单词。
（7）\‘：匹配字符串开头的一个空字符串。
（8）\' ：匹配字符串末尾的一个空字符串。
```

#### 元字符
```bash
\b  backspace
\f  formfeed
\n  newline Oine feed)
\r  carriage  return
\t  tab
\ddd  octal value ddd, where ddd  is  1  to  3  digits between 0  and  7
\c  any  other character  c  literally  (e.g., \\  for  backslash, \" for  ")  #得到原始字符
```

#### 流程控制
```bash
# if - else
awk '$2 > 5 { n = n + 1; pay = pay + $2 * $3 ;print $0,$2 * $3 }
END { if (n > 0)
        print n, "employees, total pay is", pay,
              "average pay is", pay/n   #输出可以写在多行,且不必用续行符
      else
        print "no employees are paid more than $6/hour"
    }'  text
=>
Mark  11.00  20  220
Mary  5.50  22  121
2 employees, total pay is 341 average pay is 170.5

# while
awk '{i  =  1
    while  (i  <=  2)  {
        printf("\t%.2f\n", $2  *  (1  +  $2) ^i )
        i  =  i  +  1
    }
}'

#do ... while ...
{
    count=1
    do {
        print "I get printed at least once no matter what"
    } while ( count != 1 )
}

#for
awk '{for  (i  =  1;  i  <= $3;  i =i  +  1) printf("\t%.5f\n", $1  *  (1  +  $2) ^ i) }'
0.1 0.1 3   #分别是: $1 $2 循环次数
        0.11000
        0.12100
        0.13310
1 2 3
        3.00000
        9.00000
        27.00000
```

#### 数组array
```bash
awk '{line[NR]=$0  }  #将每一行加入line数组     删除array项:   delete fooarray[1]
END { i  = NR
    while  (i  >  0)  {
        print  line[i]
        i = i  - 1
    }
}' text

可改写为for：　END{　for  (i  =  NR;  i  >  0; i  =  i  - 1)  print line[i] }
```
#### 换行
```bash
awk中换行等同于分号; 用\ 转移换行实现续行.
awk '{print   #换行意味着结束,将会输出整个文件内容 
$1                 #这里识别为单独的变量,已经超出上边print的控制范围
$2}' text       #这条执行等同于 awk '{print ; $1;$2}' test
 
awk '{print \
$1   
$2}' text   #输出了每一列, $2并没有输出 awk '{print $1;$2}' test
 
awk '{print \   #续行
$1,                  #,表示这一行还没结束
$2}' text         #输出了预计的结果  awk '{print $1,$2}' test

awk '{print 
$1,     #添加逗号导致了报错
$2}' text    # awk '{print ;    $1, ;   $2}' test  # 逗号后跟上了分号,导致语法错误 //不是很好理解
```

#### 九九乘法表
```bash
seq 9 | sed 'H;g' | awk -v RS='' '{for(i=1;i<=NF;i++)printf("%dx%d=%d%s", i, NR, i*NR, i==NR?"\n":"\t")}'  
== >
1x1=1
1x2=2   2x2=4
1x3=3   2x3=6   3x3=9
1x4=4   2x4=8   3x4=12  4x4=16
1x5=5   2x5=10  3x5=15  4x5=20  5x5=25
1x6=6   2x6=12  3x6=18  4x6=24  5x6=30  6x6=36
1x7=7   2x7=14  3x7=21  4x7=28  5x7=35  6x7=42  7x7=49
1x8=8   2x8=16  3x8=24  4x8=32  5x8=40  6x8=48  7x8=56  8x8=64
1x9=9   2x9=18  3x9=27  4x9=36  5x9=45  6x9=54  7x9=63  8x9=72  9x9=81
```

#### 执行系统命令、管道
```bash
16、awk 'BEGIN {"date"|getline d; print d}'  
       通过管道把date的执行结果送给getline，并赋给变量d，然后打印。 

通过getline命令交互输入name，并显示出来。 
awk 'BEGIN {system("echo \"Input your name:\""); 
        getline d;
         print "\nYour name is",d,"\b!\n"}'   #\b退格
       
#从文件读取内容
awk 'BEGIN {FS=":"; while(getline< "/etc/passwd" >0) { if($1~"050[0-9]_") print $1}}' 
awk 'BEGIN {FS=":";OFS="\t"; while(getline< "/etc/passwd" >0) { if($3>500) print $3,$1}}'  
==> 65534   nobody
     1000    aaa 
```

```bash
内建函数:
#参考: 
http://www.cnblogs.com/chengmo/archive/2010/10/08/1845913.html
http://www.cnblogs.com/mywolrd/archive/2012/04/27/2472916.html

length(s):  length("nihao")=>5 , awk '{print length($1),$0}' text
atan2(y,x)  arctangent  of y/x  in  the  range  -π  to π
cos(x)       指数e的x次方
exp(x)
int(x)
log(x)
rand()   随机数[0,1)
sin(x)
sqrt(x)
srand(x)  关于x的随机数


index(s,t)      #返回s中字符串t的第一位置   #第一个字符索引为0
awk 'BEGIN {print index("Bunny","ny")}' =>4
length(s)       #返回s长度
split(s,a,fs)   #在fs上将s分成序列a 
match(s,r)      #测试s是否包含匹配r的字符串
sprintf(fmt,exp) #返回经fmt格式化后的exp 
sub(r,s)        #用$0中最左边最长的子串代替s ,该字符串是r
gsub(r,s)       #在整个$0中用s替代r
gsub(r,s,t)     #在整个t中用s替代r
substr(s,p)     #返回字符串s中从p开始的后缀部分 
substr(s,p,n)   #返回字符串s中从p开始长度为n的后缀部分
tolower(s)       #返回字符串并且将所有字符转换成小写
toupper(s)       #返回字符串并且将所有字符转换成大写
#gsub函数详解:
gsub(regular expression, subsitution string, target string);简称 gsub（r,s,t) ==>
a: 返回替换的次数0-~  是一个condition statement(0为假,其他为真)
b: t为指定的字段或者省略($0)
c: 同时会做替换的操作

# echo "a b c 2011-11-22 a:d" | awk 'gsub(/-/,"",$4){print $4}' #可以看出gsub是条件语句
==> 20111122
# echo "a b c 2011-11-22 a:d" | awk 'gsub(/-/,"",$4)' #省略action时,默认输出该行
==> a b c 20111122 a:d
# echo "a b c 2011-11-22 a:d" | awk '$4=gsub(/-/,"",$4)'
==> a b c 2 a:d   #由于gsub返回次数,所以这里$4变成了2

awk 'gsub(/abc/,"x"){cost++;print $0}  END {print "The total is $" cost " filename"}' 15  #匹配/0/的行,将/x/替换为x,打印出替换后的该行, 并统计次数

awk 'BEGIN{print ""}gsub(/.+/,"BB",$2)gsub(/.+/,"AA",$1)  #两个gsub之间如果添加;将会输出两遍
{if ($4>1000&&$4<2000) c1+=$4; 
else if ($4>2000&&$4<3000) c2+=$4; 
else if ($4>3000&&$4<4000) c3+=$4; 
else c4+=$4; } 
END {printf "c1=[%d];c2=[%d];c3=[%d];c4=[%d]\n",c1,c2,c3,c4}' text

split (string, array, field separator) 
#将string按照分隔符得到一个array(相当与python list),下标0,1,2,3...
#field separator可选,默认为FS
echo "12:34:56"| awk '{split($0,a,":");for (i in a) print i,a[i]}'

substr(s,p,n) 返回字符串s中从p开始长度为n的后缀部分
#n可选,不指定时为到结尾
substr($3,10,8)  --->  表示是从第3个字段里的第10个字符开始，截取8个字符结束.
substr($3,6)     --->  表示是从第3个字段里的第6个字符开始，一直到结尾
awk 'BEGIN {print index("Bunny","ny")}' =>4

match
$ awk 'BEGIN {print match("ANCD", /d/)}'   => 0 
$ awk 'BEGIN {print match("ANCD", /C/)}'   => 3 
$ awk 'BEGIN {print match("Bunny","ny")}'  => 4
$ awk '$1=="Lulu" {print match($1, "u")} datafile  => 4  #
$ awk 'BEGIN {print match('xabcd','ab')}'  => 1 #单引号时也会返回,暂时不知怎么回事

printf sprintf的区别:
awk 'BEGIN{printf("%d",12)}'   #直接输出
awk 'BEGIN{print sprintf("%d",12)}'  #不会输出,需要使用print
格式化规则: http://www.cnblogs.com/mywolrd/archive/2012/04/27/2472916.html

#通过exit在某条件时退出，但是仍执行END操作。 
awk '{gsub(/\$/,"");gsub(/,/,"");  #gsub放入了{}中,就会只仅仅替换(因为只是返回了两个数字,没有实际输出的操作)
if ($4>3000&&$4<4000) exit; 
else c4+=$4; } 
END {printf "c1=[%d];c2=[%d];c3=[%d];c4=[%d]\n",c1,c2,c3,c4}' file 

通过next在某条件时跳过该行，对下一行执行操作。 
awk '{gsub(/\$/,"");gsub(/,/,""); 
if ($4>3000) next; 
else c4+=$4; } 
END {printf "c4=[%d]\n",c4}' file

# 重定向输出>
1. 把file1、file2、file3的文件内容全部写到fileall中，格式为打印文件并前置文件名。 
awk '{ print FILENAME,$0 }' file1 file2 file3 >fileall
2. 把合并后的文件重新分拆为3个文件。并与原文件一致。 
awk ' $1 != previous { close(previous); previous=$1 }
                                   {print substr($0,index($0," ") +1)  >$1}'  fileall  # >追加

#时间、时间戳互转
awk 'BEGIN{print mktime("2017 12 10 1 1 1"),strftime("%F %T",1512838861)}'  
awk '{gsub("-"," ",$1); print mktime($1" 0 0 0")}' a.TXT
awk '{s=gensub(/file_|\.log/,"","g",FILENAME);sub("_"," ",s);cmd1="date -d \""s"\" \"+%s\"";cmd1|getline t;cmd2="date -d @"t++" \"+%F %T\"";cmd2|getline T;print T,$0}' file
```

#### 引号问题
```bash
在awk中调用系统变量必须用单引号，如果是双引号，则表示字符串 
awk引用外部变量
http://hi.baidu.com/liheng_2009/item/6466a4c0e087222447d5c0c8
以下两个链接给了更多的讨论：
http://www.linuxsir.org/bbs/thread121709.html
http://bbs.chinaunix.net/thread-1381166-1-1.html

Flag=abcd 
$ awk 'BEGIN{print "$Flag"}'    结果为$Flag
以下四种方式都可以正常引用
$ awk 'BEGIN{print "'"$Flag"'"}'  结果为abcd   #变量两边各添加一个单引号(该单引号用双引号引起来)
$ awk 'BEGIN{print "'$flag'"}'
$ awk "BEGIN{print \"$Flag\"}"

$ awk BEGIN\{print\""$Flag"\"\}
$ awk  BEGIN\{print\"$flag\"\}  

$ awk '{print $1,$2,myname}' - v myname=$name myfile

详解: awk是要运行在shell环境中的。所以，写在awk中的命令，要先经过shell解析后，再交由awk来解释和执行。
$ str=Hello
$ awk 'BEGIN{print " '$str' "}'
=> Hello
# 看上去是双引号套单引号，其实真正的原因为：
# 这是shell的功能，shell对单引号和双引号，按从左到右的顺序成对匹配
# awk命令用单引号引起来，就是防止shell对其中内容进行解释
awk '{print " '$str' "}' file
实际上就是2部分
1: awk '{print " '
2: '"}'
即awk对2个单引号内的命令起作用。
至于$str就被shell正常解释为变量str的值。
所以，如果str=hello，则经解释后成为，awk {print "hello"} file
而如果str=hello world，则解释时，在解释前一部分：awk {print " 后，在替换了变量后，变成了hello world，当shell读到hello和world中间的空格时，认为这是IFS，于是，把他们放在于不同的域中，这样解释成了：
awk BEGIN{print "hello
world"}两部分。
按照上面的解释，就可以这么来修改，比如
a)$ awk 'BEGIN{print " ' "$a" ' "}'
或者
b)$ awk "BEGIN{print \"$a\"}"
或者
c)$ awk BEGIN\{print\""$a"\"\}
对于a，解释成为：
awk BEGIN{print "hello world"} #因为$a在替换后，还在“”中包括中，所以当成了一个字符串处理。
```

#### 自定义函数(可以在''内的任意位置)
```
function function_name(argument1, argument2, ...)
{
    function body   # 可以return
}

awk '
function b()
    {
    print "b.in.$1="$1;   #来源与file的第一列
    }
{
v=100; y=200
print "a.in.v="v;  # 
print "a.in.y="y;
a(y)b()
print "a.out.v="v;
print "a.out.y="y;
}
function a(y)  #带参数
    {
    print "(a)v="v;  #全局参数v
    v=v+$1+y;  # 100 + 200 + 123
    y=300;     #局部的y更改不会影响到主题
    print "更改后的v: " v
    print "更改后的y: " y
    }'   file
 
a.in.v=100
a.in.y=200
(a)v=100
更改后的v: 423
更改后的y: 300
b.in.$1=123   #$1来自于文件
a.out.v=423
a.out.y=200  #y的值并没有变化

函数内定义的变量,不会传递给主题
变量v的值在函数a中发生了变化，并体现在了主函数中
变量y是作为函数的a的形参，在函数a中对y的修改无法体现在主函数中
函数在调用时要用(),并且可以不加分号。
```


```bash
awk '$1 > 5 && $2 < 10' test                         # && 、 ||
awk '{if(NF>1){$1="";print $0}}' test.txt           # 去第一列
awk '/hello1/,/hello2/ {print $2}' file  #处理第一次出现hello到第一次出现 hello2之间的行的内容
awk '{print $1$2}' file <==> awk '{print $1 $2}' file  #$1与$2之间的空格可以省略，输出的样式中将合并
netstat -ant|awk '/^tcp/ {++a[$NF]} END {for (i in a) print i,a[i]}' # a[$NF]+=1
awk 'BEGIN{print "\""}'   #输出双引号
awk 'BEGIN{print "'\''"}' #输出单引号
awk 'BEGIN{print "\047Hello!\047"}'  #输出单引号(ascii码八进制是47)
awk 'BEGIN {max = 0} {if ($1>max) max=$1 fi} END {print "Max=", max}' data  #求最大值，不严谨(负数?)
{ field = $NF} END { print field } #最后一行的最后一列
{ nf= nf + NF } END { print nf } #统计域数
{ for (i = NF; i > 0; i = i - 1) printf("%s ", $i) ; printf ( "\n" ) }                 #倒序每一行的域
{sum=0;for (i=1;i<=NF;i++) sum=sum+$i; print $0 " sum is: " sum } #计算每一行的和
{ for (i = 1; i <= NF; i = i + 1) sum= sum+ $i } END { print sum }          #计算每一行每一列的sum
{for (i=1;i<=NF;i++) if($i <0) $i=-$i; print }                                            #将每一列的负数取相反数
awk 'FNR <= 3 {print FILENAME ": " $0}' num  text                                    #读取每个文件的前3行
echo a b c d|awk 'BEGIN {one=1;two=2} {print $(one+two)}'     #打印了$3, 注意这里运算的写法
awk '$3 > 10 { print $0} ; $2>4.2 {print $0}' text        #多条件; 分号都可以省略  

#统计文本
{ nc = nc + length($0) + 1
 nw = nw + NF
}
END {print NR, "lines," nw, "words,", nc, "Characters" }
=> 6 lines, 18 words, 77 characters


#字符串的比较
'$1 == "Susie" ' #字符串要用双引号
echo 2 3 1|awk '/[3-9]/ {print $0}' #/正则/, 检测整行是否匹配

用for和if显示日期 
awk 'BEGIN {
    for(j=1;j<=12;j++) {
        flag=0;
        printf "\n%d月份\n",j;
        for(i=1;i<=31;i++)
        {
            if (j==2&&i>28) flag=1;  #flag来控制输出,会有一定的浪费
            if ((j==4||j==6||j==9||j==11)&&i>30) flag=1;
            if (flag==0) {printf "%02d.%02d ",j,i}
        }
    }
}'

```

#### 实操: 统计当天每个ip访问的时长
```bash
#cat file
211.103.220.197 - - [28/Aug/2009:16:56:16 +0800] 'GET /dyanmicStat?time=16:58:57 ' 200 9 0.019
211.103.220.197 - - [28/Aug/2009:16:56:18 +0800] 'GET /dyanmicStat?time=16:58:59 ' 200 9 0.019
211.103.220.197 - - [28/Aug/2009:16:56:20 +0800] 'GET /dyanmicStat?time=16:59:01 ' 200 9 0.019
211.103.220.197 - - [28/Aug/2009:16:56:22 +0800] 'GET /dyanmicStat?time=16:59:03 ' 200 9 0.019
58.33.241.108 - - [29/Aug/2009:17:26:11 +0800] 'GET /index ' 200 20845 0.120
58.33.241.108 - - [29/Aug/2009:17:26:14 +0800] 'GET /dyanmicStat?time=17:28:53 ' 200 9 0.016
58.33.241.108 - - [29/Aug/2009:17:26:16 +0800] 'GET /dyanmicStat?time=17:28:55 ' 200 9 0.017
58.33.241.108 - - [29/Aug/2009:17:26:18 +0800] 'GET /dyanmicStat?time=17:28:57 ' 200 9 0.017
58.33.241.108 - - [29/Aug/2009:17:26:20 +0800] 'GET /dyanmicStat?time=17:28:59 ' 200 9 0.018
awk '
BEGIN{
    FS=" +|[[]";
    print "IP地址\t\t\t访问时间\t\t\t\t访问时长"
}
function cal(){
split(s_time,M,":");
split(e_time,N,":");
time=(N[2]-M[2])*60*60+(N[3]-M[3])*60+N[4]-M[4];
if (ip)
    print ip"\t"s_time"-"e_time"\t"int(time/60)"分"time%60"秒"
}
$1 !=ip {
    cal();
    ip=$1;s_time=$5}
{e_time=$5}
END{
    cal()
}
' file
==> 
IP地址                  访问时间                                访问时长
211.103.220.197 28/Aug/2009:16:55:14-28/Aug/2009:16:56:22       1分8秒
58.33.241.108   29/Aug/2009:17:26:11-29/Aug/2009:17:34:01       7分50秒
