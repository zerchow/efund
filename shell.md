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

