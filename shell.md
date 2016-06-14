# 输入输出

```shell
echo "something"
read VAL
echo "$VAL"
```

# 变量

> 加花括号是为了帮助解释器确定变量的边界

```shell
val="test"
echo $val
echo ${val}
for item in a b c d
do
  echo ${a}s
done
```

# 只读变量

```shell
val="test"
readonly val
```

# 删除变量

> 不能删除只读变量

```shell
val="test"
unset val
```

# 特殊变量

|变量|含义|
|----|----|
|$0|当前脚本的文件名|
|$n|传递给脚本或函数的参数|
|$#|传递给脚本或函数的参数个数|
|$*|传递给脚本或函数的所有参数|
|$@|传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同|
|$?|上个命令的退出状态，或函数的返回值|
|$$|当前Shell进程ID。对于 Shell 脚本，就是这些脚本所在的进程ID|

## $*和$@的区别

### 当被双引号包含时，"$*" 会将所有的参数作为一个整体，"$@" 会将各个参数分开。

> /bin/bash test.sh a b c d

```shell
for item in $*
do
  echo $item
done
# a
# b
# c
# d

for item in $@
do
  echo $item
done
# a
# b
# c
# d

for item in "$*"
do
  echo $item
done
# a b c d

for item in "$@"
do
  echo $item
done
# a
# b
# c
# d
```

# 命令替换

```shell
DATE=`date`
echo $DATE

USERS=`who | wc -l`
echo $USERS

UP=`date; uptime`
echo $UP
```

# 变量替换

|形式|说明|
|----|----|
|${var}|变量本来的值|
|${var:-word}|如果变量 var 为空或已被删除(unset)，那么返回 word，但不改变 var 的值|
|${var:=word}|如果变量 var 为空或已被删除(unset)，那么返回 word，并将 var 的值设置为 word|
|${var:?message}|如果变量 var 为空或已被删除(unset)，那么将消息 message 送到标准错误输出，可以用来检测变量 var是否可以被正常赋值。若此替换出现在Shell脚本中，那么脚本将停止运行|
|${var:+word}|如果变量 var 被定义，那么返回 word，但不改变 var 的值|

# 算术运算符

> 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2

> 乘号(*)前边必须加反斜杠(\)才能实现乘法运算

> 条件表达式要放在方括号之间，并且要有空格，例如 [$a==$b] 是错误的，必须写成 [ $a == $b ]

```shell
a=20
b=30

val=`expr $a + $b`

val=`expr $a - $b`

val=`expr $a \* $b`

val=`expr $b / $a`

val=`expr $b % $a`

if [ $a == $b ]
then
  echo "equal"
fi

if [ $a != $b ]
then
  echo "not equal"
fi
```

# 关系运算符

> 关系运算符只支持数字，不支持字符串，除非字符串的值是数字

```shell
if [ $a -eq $b ]
then
  echo "equal"
else
  echo "not equal"
fi

if [ $a -ne $b ]
then
  echo "not equal"
else
  echo "equal"
fi

if [ $a -gt $b ]
then
  echo "greater than"
else
  echo "not greater than"
fi

if [ $a -lt $b ]
then
  echo "less than"
else
  echo "not less than"
fi

if [ $a -ge $b ]
then
  echo "greater than or equal to"
else
  echo "not greater than or equal to"
fi

if [ $a -le $b ]
then
  echo "less than or equal to"
else
  echo "not less than or equal to"
fi
```

# 布尔运算符

|运算符|说明|举例|
|------|----|----|
|!|非运算|[ ! false ] 返回 true|
|-o|或运算|[ $a -lt 20 -o $b -gt 100 ] 返回 true|
|-a|与运算|[ $a -lt 20 -a $b -gt 100 ] 返回 false|

# 字符串运算符

```shell
$a="abc"
$b="def"
```

|运算符|说明|举例|
|------|----|----|
|=|检测两个字符串是否相等|[ $a = $b ] 返回 false|
|!=|检测两个字符串是否不等|[ $a != $b ] 返回 true|
|-z|检测字符串长度是否为0|[ -z $a ] 返回 false|
|-n|检测字符串长度是否不为0|[ -n $a ] 返回 true|
|str|检测字符串是否非空|[ $a ] 返回 true|

# 文件测试运算符

|运算符|说明|
|------|----|
|-b file|检测文件是否是块设备文件|
|-c file|检测文件是否是字符设备文件|
|-d file|检测文件是否是目录|
|-f file|检测文件是否是普通文件（既不是目录，也不是设备文件）|
|-g file|检测文件是否设置了 SGID 位|
|-k file|检测文件是否设置了粘着位(Sticky Bit)|
|-p file|检测文件是否是命名管道|
|-u file|检测文件是否设置了 SUID 位|
|-r file|检测文件是否可读|
|-w file|检测文件是否可写|
|-x file|检测文件是否可执行|
|-s file|检测文件是否不为空（文件大小是否大于0）|
|-e file|检测文件（包括目录）是否存在|

# 注释

## 如果在开发过程中，遇到大段的代码需要临时注释起来，每一行加个#符号太费力了，可以把这一段要注释的代码用一对花括号括起来，定义成一个函数，没有地方调用这个函数，这块代码就不会执行，达到了和注释一样的效果。

# 字符串

## 单引号的限制

### 1. 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的

### 2. 单引号字串中不能出现单引号（对单引号使用转义符后也不行）

## 双引号的优点

### 1. 双引号里可以有变量

### 2. 双引号里可以出现转义字符

## 拼接字符串

```shell
str="a"

out1="this is $str"
out2="this is "$str
out3=$out1$out2
out4="$out1 $out2"
```

## 获取字符串长度

```shell
string="abcd"
echo ${#string}
```

## 提取子字符串

```shell
string="abcd"
echo ${string:1:1}  # b，索引从0开始
```

## 查找子字符串

```shell
string="abcd"
echo `expr index "string" c`  # 3，索引从1开始
```

# 定义数组

### 在Shell中，用括号来表示数组，数组元素用“空格”符号分割开

### 可以不使用连续的下标，而且下标的范围没有限制

```shell
array_name=(value0 value1 value2)

array_name=(
value0
value1
value2)

# 单独定义各个分量

array_name[0]=value0
array_name[1]=value1
array_name[2]=value2
```

## 读取数组

```shell
value=${array_name[2]}

# 读取所有元素
echo ${array_name[*]}
echo ${array_name[@]}
```

## 获取数组长度

```shell
len=${#array_name[*]}
len=${#array_name[@]}

# 获取单个元素长度
len_2=${#array_name[2]}
```

# printf

- printf 命令不用加括号
- format-string 可以没有引号，但最好加上，单引号双引号均可。
- 参数多于格式控制符(%)时，format-string 可以重用，可以将所有参数都转换。
- arguments 使用空格分隔，不用逗号。

```shell
# format-string为双引号
$ printf "%d %s\n" 1 "abc"
1 abc
# 单引号与双引号效果一样 
$ printf '%d %s\n' 1 "abc" 
1 abc
# 没有引号也可以输出
$ printf %s abcdef
abcdef
# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
$ printf %s abc def
abcdef
$ printf "%s\n" abc def
abc
def
$ printf "%s %s %s\n" a b c d e f g h i j
a b c
d e f
g h i
j
# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
$ printf "%s and %d \n" 
and 0
# 如果以 %d 的格式来显示字符串，那么会有警告，提示无效的数字，此时默认置为 0
$ printf "The first program always prints'%s,%d\n'" Hello Shell
-bash: printf: Shell: invalid number
The first program always prints 'Hello,0'
```

# case

```shell
read num
case $num in
  1)
  echo "num is 1"
  ;;
  2)
  echo "num is 2"
  ;;
  3)
  echo "num is 3"
  ;;
  *)
  echo "other"
  ;;
esac
```

## 参数例子

```shell
option="$1"
case ${option} in
  -f)
  FILE="$2"
  echo "file name is $FILE"
  ;;
  -d)
  DIR="$2"
  echo "dir name is $DIR"
  ;;
  *)
  exit 1
  ;;
esac
```

# for

### 列表是一组值（数字、字符串等）组成的序列，每个值通过空格分隔。每循环一次，就将列表中的下一个值赋给变量

### in 列表是可选的，如果不用它，for 循环使用命令行的位置参数

## 输出列表

```shell
for loop in 1 2 3 4 5 6
do
  echo $loop
done
```

## 输出字符串的字符

```shell
for c in "string"
do
  echo $c
done
```

## 输出特定文件

```shell
for FILE in $HOME/.bash*  # 以.bash开头
do
  echo $FILE
done
```

# while

## 输出1到5

```shell
count=0
while [ $count -lt 5 ]
do
  $count=`expr $count + 1`
  echo $count
done
```

## 读取键盘

```shell
echo "type ctrl+d to stop"
while read FILE
do
  echo $FILE
done
```

# until

```shell
a=0
until [ ! $a -lt 10 ]
do
  echo $a
  $a=`expr $a + 1`
done
```

# break

### `break n`表示跳出第n层循环

# continue

### `continue n`表示跳到第n层循环的下一次循环

# 函数

### 先定义后使用

### function关键字可加可不加

### 函数返回值，可以显式增加return语句；如果不加，会将最后一条命令运行结果作为返回值

### Shell 函数返回值只能是整数，一般用来表示函数执行成功与否，0表示成功，其他值表示失败。如果 return其他数据，比如一个字符串，往往会得到错误提示：“numeric argument required”

### 如果一定要让函数返回字符串，那么可以先定义一个变量，用来接收函数的计算结果，脚本在需要的时候访问这个变量来获得函数返回值

> 外部变量存储，或者$(function_namme)

### 调用函数只需要给出函数名，不需要加括号

### 用#?访问函数调用返回的整数值

### 如果你希望直接从终端调用函数，可以将函数定义在主目录下的 .profile 文件，这样每次登录后，在命令提示符后面输入函数名字就可以立即调用

## 删除函数

```shell
unset .f function_name
```

# 函数参数

### 在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...

### $10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数

```shell
# 调用
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

# 文件包含

### 被包含脚本不需要有执行权限

```shell
. filename
source filename
```

