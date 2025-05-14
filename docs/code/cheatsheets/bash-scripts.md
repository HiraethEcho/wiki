---
title: Bash Scripts
toc: true
tags:
    - linux
    - handbook
    - tools
date: 2024-12-31
---

# Bash Scripts

## intro

### run

You can run the script in the following ways:

`./hello_world.sh`

`bash hello_world.sh`.

### she-bang

First line of shell Scripts usually is the `she-bang`:

```
#!/bin/sh
```

Means using `/bin/sh` to run the script.

但是`/bin/sh`通常是符号链接，在不同机器上实际上是不同的shell，有时会有以外情况。所以建议写成

```
#!/bin/bash
```

### 注释

以 # 开头的行就是注释，会被解释器忽略。


多行注释可以使用以下格式：

```sh
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

以上例子中，: 是一个空命令，用于执行后面的 Here 文档， <<'EOF' 表示开启 Here 文档，COMMENT 是 Here 文档的标识符，在这两个标识符之间的内容都会被视为注释，不会被执行。

EOF 也可以使用其他符号:

```sh

: <<'COMMENT'
这是注释的部分。
可以有多行内容。
COMMENT

:<< '
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!

```

## variables

### define

We can define a variable by using the syntax `variable_name=value`. To get the value of the variable, add `$` before the variable.

```bash
#!/bin/bash
# A simple variable example
greeting=Hello
name=Tux
echo $greeting $name
```

注意，变量名和等号之间不能有空格

字符串：

```sh
my_string='Hello, World!my_string="Hello, World!"
```

```
RUNOOB="www.runoob.com"
LD_LIBRARY_PATH="/bin/"
_var="123"
var2="abc"
```

```sh
for file in \` ls / etc \`
for file in $ (ls / etc)

```

数组：

```
my_array=(1 2 3 4 5)
```

只读变量

```sh
#!/bin/bash

myUrl="https://www.google.com"
readonly myUrl
myUrl="https://www.runoob.com"
```

使用 unset 命令可以删除变量。语法：

```
unset variable_name
```

### 变量类型

**字符串变量：** 在 Shell中，变量通常被视为字符串。

你可以使用单引号 ' 或双引号 " 来定义字符串，例如：

```
my_string='Hello, World!'
my_string="Hello, World!"
```

单引号字符串的限制：

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字符串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

双引号的优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符

拼接字符串

```sh
your\_name = "runoob"
\# 使用双引号拼接
greeting = "hello, " $your\_name "!"
greeting\_1 = "hello, ${your\_name}!"
echo $greeting $greeting\_1

\# 使用单引号拼接
greeting\_2 = 'hello, ' $your\_name '!'
greeting\_3 = 'hello, ${your\_name}!'
echo $greeting\_2 $greeting\_3
```

输出结果为：

```
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```

获取字符串长度

```sh
string = "abcd"
echo ${#string} \# 输出 4
```

### 数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。

类似于 C 语言，数组元素的下标由 0 开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：

```
数组名=(值1 值2 ... 值n)
```

例如：

```
array_name=(value0 value1 value2 value3)
```

或者

```
array_name=(
value0
value1
value2
value3
)
```

还可以单独定义数组的各个分量：

```
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

可以不使用连续的下标，而且下标的范围没有限制。

读取数组元素值的一般格式是：

```
${数组名[下标]}
```

例如：

```
valuen=${array_name[n]}
```

使用 @ 符号可以获取数组中的所有元素，例如：

```
echo ${array_name[@]}
```

获取数组长度的方法与获取字符串长度的方法相同，例如：

```sh
\# 取得数组元素的个数
length = ${#array\_name\[@\]}
\# 或者
length = ${#array\_name\[\*\]}
\# 取得数组单个元素的长度
length = ${#array\_name\[n\]}
```

### 运算

Arithmetic Expressions:

Below are the operators supported by bash for mathematical calculations:

| Operator | Usage          |
| -------- | -------------- |
| +        | addition       |
| \-       | subtraction    |
| \*       | multiplication |
| /        | division       |
| \*\*     | exponentiation |
| %        | modulus        |

Numeric Comparison logical operators

Comparison is used to check if statements evaluate to `true` or `false`. We can use the below shown operators to compare two statements:

| Operation             | Syntax        | Explanation                        |
| --------------------- | ------------- | ---------------------------------- |
| Equality              | num1 -eq num2 | is num1 equal to num2              |
| Greater than equal to | num1 -ge num2 | is num1 greater than equal to num2 |
| Greater than          | num1 -gt num2 | is num1 greater than num2          |
| Less than equal to    | num1 -le num2 | is num1 less than equal to num2    |
| Less than             | num1 -lt num2 | is num1 less than num2             |
| Not Equal to          | num1 -ne num2 | is num1 not equal to num2          |

### 参数

| 名称 | 含义                                                                  |
| ---- | --------------------------------------------------------------------- |
| $#   | 传给脚本的参数个数                                                    |
| $0   | 脚本本身的名字                                                        |
| $1   | 传递给该shell脚本的第一个参数                                         |
| $2   | 传递给该shell脚本的第二个参数                                         |
| $@   | 传给脚本的所有参数的列表                                              |
| $\*  | 以一个单字符串显示所有向脚本传递的参数，与位置变量不同，参数可超过9个 |
| $$   | 脚本运行的当前进程ID号                                                |
| $?   | 显示最后命令的退出状态，0表示没有错误，其他表示有错误                 |

假设执行 `./test.sh a b c` 这样一个命令，则可以使用下面的参数来获取一些值：

- `$0`  
  对应 _./test.sh_ 这个值。如果执行的是 `./work/test.sh` ， 则对应 _./work/test.sh_ 这个值，而不是只返回文件名本身的部分。
- `$1`  
  会获取到 a，即 `$1` 对应传给脚本的第一个参数。
- `$2`  
  会获取到 b，即 `$2` 对应传给脚本的第二个参数。
- `$3`  
  会获取到 c，即 `$3` 对应传给脚本的第三个参数。 `$4` 、 `$5` 等参数的含义依此类推。
- `$#`  
  会获取到 3，对应传入脚本的参数个数，统计的参数不包括 `$0` 。
- `$@`  
  会获取到 "a" "b" "c"，也就是所有参数的列表，不包括 `$0` 。
- `$*`  
  也会获取到 "a" "b" "c"， 其值和 `$@` 相同。但 `"$*"` 和 `"$@"` 有所不同。  
  `"$*"` 把所有参数合并成一个字符串，而 `"$@"` 会得到一个字符串参数数组。
- `$?`  
  可以获取到执行 `./test.sh a b c` 命令后的返回值。  
  在执行一个前台命令后，可以立即用 `$?` 获取到该命令的返回值。  
  该命令可以是系统自身的命令，可以是 shell 脚本，也可以是自定义的 bash 函数。

当执行系统自身的命令时， `$?` 对应这个命令的返回值。  
当执行 shell 脚本时， `$?` 对应该脚本调用 `exit` 命令返回的值。如果没有主动调用 `exit` 命令，默认返回为 0。  
当执行自定义的 bash 函数时， `$?` 对应该函数调用 `return` 命令返回的值。如果没有主动调用 `return` 命令，默认返回为 0。

下面举例说明 `"$*"` 和 `"$@"` 的差异。假设有一个 `testparams.sh` 脚本，内容如下：

```bash
#!/bin/bash

for arg in "$*"; do
    echo "****:" $arg
done
echo --------------
for arg in "$@"; do
    echo "@@@@:" $arg
done
```

这个脚本分别遍历 `"$*"` 和 `"$@"` 扩展后的内容，并打印出来。

执行 `./testparams.sh` 脚本，结果如下：

```bash
$ ./testparams.sh This is a test
****: This is a test
--------------
@@@@: This
@@@@: is
@@@@: a
@@@@: test
```

可以看到， `"$*"` 只产生一个字符串，for 循环只遍历一次。  
而 `"$@"` 产生了多个字符串，for 循环遍历多次，是一个字符串参数数组。

**注意** ：如果传入的参数多于 9 个，则不能使用 `$10` 来引用第 10 个参数，而是要用 `${10}` 来引用。

即，需要用大括号 `{}` 把大于 9 的数字括起来。  
例如， `${10}` 表示获取第 10 个参数的值，写为 `$10` 获取不到第 10 个参数的值。  
实际上， `$10` 相当于 `${1}0` ，也就是先获取 `$1` 的值，后面再跟上 0。  
如果 `$1` 的值是 "first"，则 `$10` 的值是 "first0"。

在 bash 中，可以使用 `$#` 来获取传入的命令行或者传入函数的参数个数。  
要注意的是， `$#` 统计的参数个数不包括脚本自身名称或者函数名称。  
例如，执行 `./a.sh a b` ，则 `$#` 是 2，而不是 3。

## pairs

`{}`,`[]`,`()`,<code>``</code>

### {} 花括号

**常规用法**

大括号拓展。(通配(globbing))将对大括号中的文件名做扩展。在大括号中，不允许有空白，除非这个空白被引用或转义。第一种：对大括号中的以逗号分割的文件列表进行拓展。如 touch {a,b}.txt 结果为a.txt b.txt。第二种：对大括号中以点点（..）分割的顺序文件列表起拓展作用，如：touch {a..d}.txt 结果为a.txt b.txt c.txt d.txt

```
# ls {ex1,ex2}.sh
ex1.sh  ex2.sh
# ls {ex{1..3},ex4}.sh
ex1.sh  ex2.sh  ex3.sh  ex4.sh
# ls {ex[1-3],ex4}.sh
ex1.sh  ex2.sh  ex3.sh  ex4.sh
```

代码块，又被称为内部组，这个结构事实上创建了一个匿名函数 。与小括号中的命令不同，大括号内的命令不会新开一个子shell运行，即脚本余下部分仍可使用括号内变量。括号内的命令间用分号隔开，最后一个也必须有分号。{}的第一个命令和左括号之间必须要有一个空格。

**几种特殊的替换结构**

```
${var:-string}
${var:+string}
${var:=string}
${var:?string}
```

1. `${var:-string}`和`${var:=string}`:若变量var为空，则用在命令行中用string来替换`${var:-string}`，否则变量var不为空时，则用变量var的值来替换`${var:-string}`；对于`${var:=string}`的替换规则和`${var:-string}`是一样的，所不同之处是`${var:=string}`若var为空时，用string替换`${var:=string}`的同时，把string赋给变量var： `${var:=string}`很常用的一种用法是，判断某个变量是否赋值，没有的话则给它赋上一个默认值。

2. `${var:+string}`的替换规则和上面的相反，即只有当var不是空的时候才替换成string，若var为空时则不替换或者说是替换成变量 var的值，即空值。(因为变量var此时为空，所以这两种说法是等价的)

3. `${var:?string}`替换规则为：若变量var不为空，则用变量var的值来替换`${var:?string}`；若变量var为空，则把string输出到标准错误中，并从脚本中退出。我们可利用此特性来检查是否设置了变量的值。

在上面这五种替换结构中string不一定是常值的，可用另外一个变量的值或是一种命令的输出。

**四种模式匹配替换结构**

模式匹配记忆方法：

```
# 是去掉左边(在键盘上#在$之左边)
% 是去掉右边(在键盘上%在$之右边)
```

`#`和`%`中的单一符号是最小匹配，两个相同符号是最大匹配。

```
${var%pattern}
${var%%pattern
${var#pattern}
${var##pattern}
```

- 第一种模式：`${variable%pattern}`，这种模式时，shell在variable中查找，看它是否一给的模式pattern结尾，如果是，就从命令行把variable中的内容去掉右边最短的匹配模式
- 第二种模式： `${variable%%pattern}`，这种模式时，shell在variable中查找，看它是否一给的模式pattern结尾，如果是，就从命令行把variable中的内容去掉右边最长的匹配模式
- 第三种模式：`${variable#pattern}` 这种模式时，shell在variable中查找，看它是否一给的模式pattern开始，如果是，就从命令行把variable中的内容去掉左边最短的匹配模式
- 第四种模式： `${variable##pattern}` 这种模式时，shell在variable中查找，看它是否一给的模式pattern结尾，如果是，就从命令行把variable中的内容去掉右边最长的匹配模式
- 这四种模式中都不会改变variable的值，其中，只有在pattern中使用了\*匹配符号时，%和%%，#和##才有区别。结构中的pattern支持通配符，\*表示零个或多个任意字符，?表示仅与一个任意字符匹配，\[...\]表示匹配中括号里面的字符，\[!...\]表示不匹配中括号里面的字符。

```
# var=testcase
# echo $var
testcase
# echo ${var%s*e}
testca
# echo $var
testcase
# echo ${var%%s*e}
te
# echo ${var#?e}
stcase
# echo ${var##?e}
stcase
# echo ${var##*e}

# echo ${var##*s}
e
# echo ${var##test}
case
```

**字符串提取和替换**

```
${var:num}
${var:num1:num2}
${var/pattern/pattern}
${var//pattern/pattern}
```

- 第一种模式：`${var:num}`，这种模式时，shell在var中提取第num个字符到末尾的所有字符。若num为正数，从左边0处开始；若num为负数，从右边开始提取字串，但必须使用在冒号后面加空格或一个数字或整个num加上括号，如`${var: -2}`、`${var:1-3}`或`${var:(-2)}`。
- 第二种模式：`${var:num1:num2}`，num1是位置，num2是长度。表示从$var字符串的第$num1个位置开始提取长度为$num2的子串。不能为负数。
- 第三种模式：`${var/pattern/pattern}`表示将var字符串的第一个匹配的pattern替换为另一个pattern。
- 第四种模式：`${var//pattern/pattern}`表示将var字符串中的所有能匹配的pattern替换为另一个pattern。

```
[root@centos ~]# var=/home/centos
[root@centos ~]# echo $var
/home/centos
[root@centos ~]# echo ${var:5}
/centos
[root@centos ~]# echo ${var: -6}
centos
[root@centos ~]# echo ${var:(-6)}
centos
[root@centos ~]# echo ${var:1:4}
home
[root@centos ~]# echo ${var/o/h}
/hhme/centos
[root@centos ~]# echo ${var//o/h}
/hhme/cenths
```

- ${a} 变量a的值, 在不引起歧义的情况下可以省略大括号。
- $(cmd) 命令替换，和\`cmd\`效果相同，结果为shell命令cmd的输，过某些Shell版本不支持$()形式的命令替换, 如tcsh。
- $((expression)) 和\`exprexpression\`效果相同, 计算数学表达式exp的数值, 其中exp只要符合C语言的运算规则即可, 甚至三目运算符和逻辑表达式都可以计算。

- 单小括号，(cmd1;cmd2;cmd3) 新开一个子shell顺序执行命令cmd1,cmd2,cmd3, 各命令之间用分号隔开, 最后一个命令后可以没有分号。
- 单大括号，{ cmd1;cmd2;cmd3;} 在当前shell顺序执行命令cmd1,cmd2,cmd3, 各命令之间用分号隔开, 最后一个命令后必须有分号, 第一条命令和左括号之间必须用空格隔开。
  对 {} 和 () 而言, 括号中的重定向符只影响该条命令， 而括号外的重定向符影响到括号中的所有命令。

### `[]`

```sh
type [

[ is a shell builtin
```

This means that '[' is actually a program, just like ls and other programs, so it must be surrounded by spaces.

- bash 的内部命令，\[和test是等同的。如果我们不用绝对路径指明，通常我们用的都是bash自带的命令。if/test结构中的左中括号是调用test的命令标识，右中括号是关闭条件判断的。这个命令把它的参数作为比较表达式或者作为文件测试，并且根据比较的结果来返回一个退出状态码。if/test结构中并不是必须右中括号，但是新版的Bash中要求必须这样。
- Test和\[\]中可用的比较运算符只有==和!=，两者都是用于字符串比较的，不可用于整数比较，整数比较只能使用-eq，-gt这种形式。无论是字符串比较还是整数比较都不支持大于号小于号。如果实在想用，对于字符串比较可以使用转义形式，如果比较"ab"和"bc"：\[ ab \\< bc \]，结果为真，也就是返回状态为0。\[ \]中的逻辑与和逻辑或使用-a 和-o 表示。
- 字符范围。用作正则表达式的一部分，描述一个匹配的字符范围。作为test用途的中括号内不能使用正则。
- 在一个array 结构的上下文中，中括号用来引用数组中每个元素的编号。

### `[[]]`

- \[\[是 bash 程序语言的关键字。并不是一个命令，\[\[ \]\] 结构比\[ \]结构更加通用。在\[\[和\]\]之间所有的字符都不会发生文件名扩展或者单词分割，但是会发生参数扩展和命令替换。
- 支持字符串的模式匹配，使用=~操作符时甚至支持shell的正则表达式。字符串比较时可以把右边的作为一个模式，而不仅仅是一个字符串，比如\[\[ hello == hell? \]\]，结果为真。\[\[ \]\] 中匹配字符串或通配符，不需要引号。
- 使用\[\[... \]\]条件判断结构，而不是\[... \]，能够防止脚本中的许多逻辑错误。比如，&&、||、<和> 操作符能够正常存在于\[\[ \]\]条件判断结构中，但是如果出现在\[ \]结构中的话，会报错。比如可以直接使用if \[\[ $a!= 1 && $a!= 2 \]\], 如果不适用双括号, 则为if \[ $a -ne 1\] && \[ $a!= 2 \]或者if \[ $a -ne 1 -a $a!= 2 \]。
- bash把双中括号中的表达式看作一个单独的元素，并返回一个退出状态码。

```sh
if ($i<5)
if [ $i -lt 5 ]
if [ $a -ne 1 -a $a != 2 ]
if [ $a -ne 1] && [ $a != 2 ]
if [[ $a != 1 && $a != 2 ]]

for i in $(seq 0 4);do echo $i;done
for i in \`seq 0 4\`;do echo $i;done
for ((i=0;i<5;i++));do echo $i;done
for i in {0..4};do echo $i;done
```

### `()`

- **命令组** 。括号中的命令将会新开一个子shell顺序执行，所以括号中的变量不能够被脚本余下的部分使用。括号中多个命令之间用分号隔开，最后一个命令可以没有分号，各命令和括号之间不必有空格。
- **命令替换** 。等同于\`cmd\`，shell扫描一遍命令行，发现了$(cmd)结构，便将$(cmd)中的cmd执行一次，得到其标准输出，再将此输出放到原来命令。有些shell不支持，如tcsh。
- **用于初始化数组** 。如：array=(a b c d)

### `(())`

- 整数扩展。这种扩展计算是整数型的计算，不支持浮点型。((exp))结构扩展并计算一个算术表达式的值，如果表达式的结果为0，那么返回的退出状态码为1，或者 是"假"，而一个非零值的表达式所返回的退出状态码将为0，或者是"true"。若是逻辑判断，表达式exp为真则为1,假则为0。
- 只要括号中的运算符、表达式符合C语言运算规则，都可用在$((exp))中，甚至是三目运算符。作不同进位(如二进制、八进制、十六进制)运算时，输出结果全都自动转化成了十进制。如：echo $((16#5f)) 结果为95 (16进位转十进制)
- 单纯用 (( )) 也可重定义变量值，比如 a=5; ((a++)) 可将 $a 重定义为6
- 常用于算术运算比较，双括号中的变量可以不使用$符号前缀。括号内支持多个表达式用逗号分开。 只要括号中的表达式符合C语言运算规则,比如可以直接使用for((i=0;i<5;i++)), 如果不使用双括号, 则为for i in \`seq 0 4\`或者for i in {0..4}。再如可以直接使用if (($i<5)), 如果不使用双括号, 则为if \[ $i -lt 5 \]。

```sh
var=$((expression))
```

### <code>``</code> back ticks

var= commands

**Example**: Suppose we want to get the output of a list of mountpoints with `tmpfs` in their name. We can craft a statement like this: `df -h | grep tmpfs`.

To include it in the bash script, we can enclose it in back ticks.

```bash
#!/bin/bash

var=\`df -h | grep tmpfs\`
echo $var
```

## input

### in console

```sh
echo "input a"
read a
echo "input b"
read b

printf "a=%s,b=%s,sum is %s" $a $b $((a + b))
```

It is possible to give arguments to the script on execution.

`$@` represents the position of the parameters, starting from one.

```bash
#!/bin/bash

for x in $@
do
    echo "Entered arg is $x"
done
```

Run it like this:

`./script arg1 arg2`

### input file

Suppose we have a file `sample_file.txt`, We can read the file line by line and print the output on the screen.

```bash
#!/bin/bash

LINE=1

while read -r CURRENT_LINE
    do
        echo "$LINE: $CURRENT_LINE"
    ((LINE++))
done < "sample_file.txt"
```

## control

### if

- `if...then...fi` statements
- `if...then...else...fi` statements
- `if..elif..else..fi`
- `if..then..else..if..then..fi..fi..` (Nested Conditionals)

```sh
if [ ... ]
then
  # if-code
else
  # else-code
fi
```

or

```sh
if [ ... ] ; then
  # if-code
else
  # else-code
fi
```

conditions:

examples:

```sh
#!/bin/sh
if [ "$X" -lt "0" ]
then
  echo "X is less than zero"
fi
if [ "$X" -gt "0" ]; then
  echo "X is more than zero"
fi
[ "$X" -le "0" ] && \
      echo "X is less than or equal to  zero"
[ "$X" -ge "0" ] && \
      echo "X is more than or equal to zero"
[ "$X" = "0" ] && \
      echo "X is the string or number \"0\""
[ "$X" = "hello" ] && \
      echo "X matches the string \"hello\""
[ "$X" != "hello" ] && \
      echo "X is not the string \"hello\""
[ -n "$X" ] && \
      echo "X is of nonzero length"
[ -f "$X" ] && \
      echo "X is the path of a real file" || \
      echo "No such file: $X"
[ -x "$X" ] && \
      echo "X is the path of an executable file"
[ "$X" -nt "/etc/passwd" ] && \
      echo "X is a file which is newer than /etc/passwd"


read a
read b
read c

if [ $a == $b -a $b == $c -a $a == $c ] # -a means
then
echo EQUILATERAL

elif [ $a == $b -o $b == $c -o $a == $c ] # -o means or
then
echo ISOSCELES
else
echo SCALENE

fi
```

### loop

```sh

for i in {1..5}
do
    echo $i
done

for X in cyan magenta yellow
do
    echo $X
done
```

```sh
i=1
while [[ $i -le 10 ]] ; do
   echo "$i"
  (( i += 1 ))
done
```

## function

## Advance

### get-opts

example:

```sh
#!/bin/sh
docs="Usage: \
\n\tbash $0 [options] \
\nOptions: \
\n\t-N NAME: your name, required\
\n\t-a AGE: your age, required\
\n\t-v: verbose mode, optional\
\nExample: \
\n\t bash $0 -N balabala -a 24 "

usage() {
    echo -e $docs >&2
    exit 1
}

if [ $# -eq 0 ] || [ $1 == -h ]; then usage; fi

## 设置变量的默认值
verbose=n

## 检查变量参数是否缺失
check() {
    opt=$1
    arg=$2

    if [[ $arg =~ ^- ]] || [ ! $arg ]
    then
        echo "ERROR: -$opt expects an corresponding argument" >&2
        usage
    fi
}

## 循环解析脚本的所有positional parameters
while getopts :vN:a: opt
do
    case $opt in
    v)
        verbose=y
        ;;
    N)
        check N $OPTARG
        name=$OPTARG
        ;;
    a)
        check a $OPTARG
        age=$OPTARG
        ;;
    :)
        echo "ERROR: -$OPTARG expects an corresponding argument" >&2
        usage
        ;;
    \?) # shell里'?'具有匹配一位任意字符的作用，因此需要转义为普通字符
        echo "ERROR: unkown option -$OPTARG" >&2
        usage
        ;;
    esac
done

## 如果用户没有提供-N选项
if [ ! $name ]
then
    echo ERROR: option -N is required >&2
    usage
fi

## 如果用户没有提供-a选项
if [ ! $age ]
then
    echo ERROR: option -a is required >&2
    usage
fi

if [ $verbose == n ]
then
    echo "Merry Christmas! $name"
else
    echo "Hello, your name is $name, you are $age years old!"
    echo "Merry Christmas!!"
fi
```

### cron

Cron is a job scheduling utility present in Unix like systems. You can schedule jobs to execute daily, weekly, monthly or in a specific time of the day. Automation in Linux heavily relies on cron jobs.

Below is the syntax to schedule crons:

```bash
# Cron job example
* * * * * sh /path/to/script.sh
```

Here, `*` represents minute(s) hour(s) day(s) month(s) weekday(s), respectively.

Below are some examples of scheduling cron jobs.

| SCHEDULE       | SCHEDULED VALUE                                           |
| -------------- | --------------------------------------------------------- |
| 5 0 \* 8 \*    | At 00:05 in August.                                       |
| 5 4 \* \* 6    | At 04:05 on Saturday.                                     |
| 0 22 \* \* 1-5 | At 22:00 on every day-of-week from Monday through Friday. |

`crontab -l` lists the already scheduled scripts for a particular user.

[^1]: https://www.freecodecamp.org/news/shell-scripting-crash-course-how-to-write-bash-scripts-in-linux/

[^2]: https://www.freecodecamp.org/news/cron-jobs-in-linux/

[^3]: htts://blog.csdn.net/weixin_49114503/article/details/141360012

[^4]: htts://www.runoob.com/w3cnote/linux-shell-brackets-features.html

[^5]: http://blog.csdn.net/taiyang1987912/article/details/39551385

[^6]: https://www.shellscript.sh/

[^7]: https://www.runoob.com/linux/linux-shell-variable.html
