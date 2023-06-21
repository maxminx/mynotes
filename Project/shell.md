[[program语言]]


##### 1：语法

1:echo

单引号和双引号，句中有其一，则需要其二将字符串包围起来

echo "I'm sunyc"

输出内容和后接命令输出内容在同一行

echo -n "current time=";date;

2:变量复制，等号左右不能有空格

3：反引号，将shell命令的输出赋值

```shell
mdate=`date +%y%m%d`#output format likes 230328
```

4:可以输入重定向的命令grep,wc，sort等等

5：数学计算

整数计算：

```bash
var=`expr 2+4`#expr方式
var=$[2+4]#$[]方式
```

浮点数计算

```bash
var=`echo "scale=3; 2+4"|bc`#sacle定义输出几位的小数点
```

```bash
var1=2
var2=3
var=`bc<<EOF
scale=3
$var1+$var2
EOF
`
echo $var
```

6：选择结构化

```bash
if command#只有command推出码是0时，才执行then
then
    commands
fi
```

```bash
#另一种格式
if command;then
    commands
fi
```

```bash
#if-else
if command;then
    commands
else
    commands
fi
```

```bash
#if-elif
if command;then
    commands
elif command2;then
    commands
fi
```

test命令，用来判断条件，其符号形式为[]

```bash
if test condition;then
    commands
fi

if [condition];then
    commands
fi
```

test用来比较数值;不能用来比较浮点数，可以将浮点数保存为字符串的形式来比较

| command         | description | command         | description |
| --------------- | ----------- | --------------- | ----------- |
| [var1 -eq var2] | 是否相等        | [var1 -le var2] |             |
| [var1 -ge var2] | 是否大于等于      | [var1 -lt var2] |             |
| [var1 -gt var2] | 是否大于        | [var1 -ne var2] |             |

(())，复杂的数学表达，内部不需转义

if (($val+1>9))

test用来比较字符串

| command | description | command  | description |
| ------- | ----------- | -------- | ----------- |
| =       |             | !=       |             |
| >       | 前需有\转义      | -n str1  | 是否非空        |
| <       | 前需有\转义      | -z str11 | 是否为空        |

[[]]，可以进行模板匹配

if 双括号$USR = r*

test用来比较文件

| command         | description           | command         | description |
| --------------- | --------------------- | --------------- | ----------- |
| -d file         |                       | -r file         |             |
| -f file         |                       | -w file         |             |
| -e file         |                       | -x file         |             |
| file1 -nt file2 | file1 newer to file2  | file1 -ot file2 |             |
| -s file         | 是否存在并非空。应用：删除文件前判断    |                 |             |
| -d "$file"      | 避免变量file中有空格，双引号会看成整体 |                 |             |

逻辑：与、或

| []\|\|[] | []&&[] |
| -------- | ------ |

case关键字

```bash
case $var in
rich|bara)
    commands;;
pattern3)
    commands;;
*)
    commands;;
esac
```

7：循环结构

```bash
for var in list
do
    commands
done
#or
for var in list;do
    commands
done#  
```

在list中的默认分隔符是：空格、制表符、换行；在双引号内，即使有分隔符，也会把双引号范围看作一个整体。

若list中有单引号，则需要转义字符\，或双引号把单引号包括起来

修改默认的分隔符变量

```bash
IFS.old=$IFS
IFS=$'\n:;"'//分隔符变为：换行符、冒号、分号、双引号
commands;
IFS=$IFS.old
```

C语言风格的for

```bash
for((a=1,b=2; a<10; a++,b--))
do 
    echo "$a-$b"
done
```

while关键字

```bash
var=10
while [$var -gt 1]
do
    echo $var
    var=$[$var-1]
done
```

while包含多个测试命令时，只有最后的测试状态码决定循环的结束

```bash
while echo $var
      [$var -gt 1]//此测试状态码决定循环的结束
do
    commands
done
```

until关键字，满足条件则停止，与while相反

```bash
var=100
until [$var -qe 0]
do
    var=$var-1
done
```

break n终止当前for循环，n默认是1；当n=2，则是终止双层for循环

continue

最后done可以接管道|，或重定向>

```bash
while []
do 
    commands
done|sort#或者done>result.txt
```

8：用户输入

输入参数：$0-程序名；\$1-\$9；\${10} - \${100000}

计量参数\$#

```bash
var=$#
lastVar=$var//1
lastVar=${!#}//与1处同义，花括号内不能用$符号
```

抓取输入参数

```bash
$*//把多个输入参数看着整体一个str
$@//把多个输入参数看着str数组，可以用for遍历
```

移动变量

```bash
shift n//n默认为1;删除参数1到参数n，参数n+1后面的参数迁移n位
```

##### 2：常用工具