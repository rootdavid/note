## 通过案例学习Shell

### 计算两个日期之间相隔的天数

#### 知识点

#### date 命令

`-s` 获取时间戳

`-d` 显示某个特定时间

常用格式化选项

- +%Y 年
- +%m 月
- +%d 日
- +%H 时
- +%M 分
- +%S 秒
- +%w 周几（0代表周日）

格式化命令

```bash
date +"%Y-%m-%d %H:%M:%S"
```

#### 变量获取

`$(command)` 返回这条命令的输出（可嵌套）

#### expr命令

命令用于求表达式的值

- expr会在stdout中输出结果。如果为逻辑关系表达式，则结果为真时，stdout输出1，否则输出0。
- expr的exit code：如果为逻辑关系表达式，则结果为真时，exit code为0，否则为1。

#### 错误记录

`$(date -d "$date1" +%s)` 错写为括号内加双引号，只需要在引用变量部分加引号

### 当前系统是基于哪个Linux发行版

#### 知识点

##### if (which rpm &> /dev/null)

通过重定向为空判断which命令是否能执行

##### 比较符

if语句执行比较的时候条件最好写在[]里

- -eq ：equal（相等）
- -ne ：not equal（不等）

- -gt ：greater than（大于）
- -ge ：greater than or equal（大于或等于）

- -lt ：less than（小于）

- -le ：less than or equal（小于或等于）

##### $[xxx]

`$[xxx]` 是 shell 中的算术展开语法，用于进行数学计算。不过这种语法已经过时了，建议使用更现代的 `$((xxx))` 语法

### 循环迭代的例子

#### 查找可执行文件

##### 知识点

###### IFS=:

指定 `:` 为字符串分隔符，默认空格为分隔符

###### if [ -x file ]

判断是否为可执行文件

#### 创建多个用户账户

##### 知识点

###### useradd -c "$name" -m "$loginname"

添加用户命令 `-c` 是备注，`-m` 是创建家目录

###### while IFS=, read -r loginname name

读取一行文字，使用 `,` 分隔分别赋值 `loginname` `name`

###### done <- file

文件内容重定向到 `while` 循环体内

### 测试主机连通性

#### 知识点

##### while getopts t: opt

表示 `-t` 选项需要一个参数值，如果没有 `:` 就不需要参数

##### $OPTARG

获取选项的参数值

##### shift $[ $OPTIND - 1 ]

`$OPTIND` 默认值是1，解析后参数后会+1

##### $#

表示命令参数数量

##### $@

表示每个参数，使用 for 语句可以直接取到默认用空格分隔的值

##### ping -q -c 3 ip

`-q` 表示静默

`-c n` 发送的n次数据包

##### read命令

`-t xxx` 设置超时时间为xxx

`-p xxx` 输出提示语xxx

`-n xxx` 限制输入为xxx个字符

##### if [ -z xxx ]

判断是否为0个字符，是则返回真

##### if [ -s xxx ]

判断文件是否存在且为空，存在不为空返回真

##### if [ -r xxx ]

判断文件是否可读，可读返回真

##### cat $filename | while read line

文件输入流通过管道使用 `read` 命令读取每一行

### 批量导入数据至MySQL

#### 知识点

##### cat >> $outfile << EOF

等待命令行输入，遇到EOF结尾后重定向至$outfile变量文件

##### ${1}

第一个参数

### 捕获脚本信号，然后将其置于后台运行

#### 知识点

##### if [ -O xxx ]

判断该文件是否属于当前用户，属于返回真

##### trap命令

用于捕获和处理信号(signal)

`"" 信号列表` 忽略信号列表

`-- 信号列表` 重置信号列表到默认行为

##### source $scriptToRun > $scriptOutput & 

后台执行某个脚本重定向到某个文件上

### 函数

#### 知识点

##### 格式

```bash
[function] func_name() {  # function关键字可以省略
    语句1
    语句2
    ...
}
```

##### 返回值

不写return默认返回 `return 0`

`$?` 返回return值

##### 函数的输入参数

`$1` 表示第一个输入参数，`$2` 表示第二个输入参数，依此类推

### sed命令

#### 初识sed

格式：`sed options script file`

​	`-e script` 在处理输入时加入script中执行的命令

​	`-f flie` 在处理输入时加入file中执行的命令

​	`-n` 不在为每条命令产生输出，而是等待打印`p`命令

**常用命令**

- 替换 
  - `sed 's/dog/cat/' data1.txt`
  - `sed -e 's/brown/red/; s/dog/cat/' data1.txt`
  - `sed -f script1.sed data1.txt`

#### gwak编辑器

格式 `gawk options program file`

​	`-F fs` 指定行中划分数据字段的字段分隔符

​	`-f file` 从指定文件中读取 gawk 脚本代码

​	`-v var=value` 定义 gawk 脚本中的变量及其默认值

​	`-L [keyword]` 指定 gawk 的兼容模式或警告级别

**常用命令**

`gawk -F: '{print $1}' /etc/passwd` 根据分隔符`:`打印每行第一个数据字段的数据

`gawk -F: -f script2.gawk /etc/passwd` 从文件中读取脚本

`gawk` 脚本文件执行多条命令不需要分号

#### 替换shebang

##### 知识点

`sed '1s!/bin/sh!/bin/bash!' OldScripts/testAscript.sh`

`1s!` 第一处找到该字符串并使用`!`分隔

##### -sn

`-s` 将所有文件作为单独的流

`-n` 不显示输出

##### grep -l "/bin/sh" xxx

命令用于搜索含有 "/bin/sh" 字符串的文件

-`l` 

- 只列出包含匹配项的文件名
- 不显示具体匹配的内容
- 每个文件只列出一次，即使文件中有多处匹配

##### $(basename $filename)

令替换结构，用于提取文件路径中的文件名部分

### Sed进阶

#### 知识点

##### 替换脚本代码分析

```bash
#!/bin/bash

tempfile=$2

sed -n '{
1N; N;
s/ //g; s/\t//g;
s/\n/\a/g; p;
s/\a/\n/; D}' $1 >> $tempfile

sort $tempfile | uniq -d | sed 's/\a/\n/g'

rm -i $tempfile

exit
```

第一行的下一行再下一行，***总共三行***，全局去除空格和制表符，将连续三行合并（用\a分隔）输出，下一行\a换换行符再删除，后排序

#### sort $tempfile | uniq -d | sed 's/\a/\n/g'

默认按字母排序，并去重

