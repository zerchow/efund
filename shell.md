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

