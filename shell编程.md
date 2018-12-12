**shell script** 

---

[shell 变量](#shell-变量)

[shell 字符串](#shell-字符串)

[shell 数组](#shell-数组)

[shell 传递参数](#shell-传递参数)

[文件测试](#文件测试)

[test 命令](#test-命令)

​		[数值测试](#数值测试)

​		[字符串测试](#字符串测试)

​		[文件测试](#文件测试)

[循环](#循环)

[shell 函数](#shell-函数)

[shell 输入输出重定向](#shell-输入输出重定向)

[shell 文件包含](#shell-文件包含)

[shell 中的 export](#shell-中的-export)

[bash命令](#bash-命令)

---

shell 脚本编程是在根据需求向系统发布命令的过程，是实现计算机自动化工作的方法。

```sh
#!/bin/bash           # 指定解释器名称
```

保存为扩展名为 `.sh` (或者`.php`)，然后赋予脚本执行权限，并运行：

```sh
chmod +x ./test.sh
./test.sh     # ./ 指定当前目录，否则系统会在PATH中寻找同名程序
```

## shell 变量

 ```shell
url="https://github.com/DaDaDavinc"  # 注意=两边不能留空格
echo ${url}  # {}可以省略，只是为了帮助解释器识别边界，首选
readonly url # url设置为只读
unset url    # 删除变量，包括上面的只读变量

for skill in Ada Coffee Action Java; do
	echo "i am good at ${skill} script."
done
# 将循环输出 i am good at Ada/Coffee/Action/Java script...
 ```

## shell 字符串

字符串可以使用单引号 `‘ ’` 和双引号 `“ ”` 表示，首选双引号。因为双引号中可以存在变量，可以出现转义字符。

```sh
url="https://github.com/DaDaDavinc"
str="hello, my Github URL is \"${url}\" \n"
# test
echo ${str}  
# 输出 
# hello, my Github URL is https://github.com/DaDaDavinc.

# 拼接字符串,单引号，双引号，{}都行
myurl1="Github, ${url} ."
myurl2="Github, "$url" ."
```

## shell 数组

bash 只支持一维数组，且不限定数组长度。

```sh
arr1=(val0 val1 val2 val3)  # 或者
arr2=(
	val0
	val1
	val2
	val3
) # 或者
arr[0]=val0
arr[1]=val1

# 读取数组
${arr1[@]}       # @ 读取所有元素
value=${arr1[2]} # 读取数组arr1下标为2的元素并赋值

# 获取数组长度
length=${#arr1[@]} # @ 灵活可变
```

## shell 传递参数

在执行 shell 脚本时，向脚本传递参数，脚本参数获取格式为：` $n ` （n 表数字），`$0` 是文件名, `$1` 是脚本第一个参数，以此类推。

| 参数处理 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 将所有的参数显示成一个字符串                                 |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 将所有的参数显示成多个字符串                                 |
| $-       | 显示Shell使用的当前选项，与[set命令](http://www.runoob.com/linux/linux-comm-set.html)功能相同。 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

要在文本中显示 `$` / `“`， 应使用转义字符 `\$` / `\"`

## 文件测试

检测 Unix 文件的各种属性。

```sh
#!/bin/bash
file="~/.local/init.sh"
if [ -r ${file} ] ; then         # if 内容用 [ ]，注意两边留空
	echo -e "文件可读...\n"       # then 若不换行，需要加分号 ;
else 
	echo -e "文件不可读...\n"     # -e 开启转义
fi                              
# =====属性分析=====
# -b   --block，文件是否为块设备
# -c   --char, 字符设备文件
# -f   --file, 普通文件
# -r   --read, 是否可读
# -w   --write, 是否可写
# -x   --executable, 可执行
# -s   --space，是否为空
# -e   --exist, 是否存在
```

## test 命令

### 数值测试

```sh
#!/bin/bash
num1=100
num2=100
if test $[num1] -eq $[num2]; then
	echo "相等。"
else 
	echo "不等。"
fi
# =====参数=====
# -ne    -- not equal, 不相等则为真
# -gt    --greater, 大于则为真
# -ge    -- 大于等于
# -lt    -- 小于
# -le    --lower and equal, 小于等于
```

代码中的`[ ]` 执行基本的算数运算

### 字符串测试

```sh
str1="test"
str2="tesst"
if test ${str1} = ${str2}
then
	echo "相同。"
else 
	echo "不相同。"
fi
```

### 文件测试

```sh
#!/bin/bash
# 测试文件是否存在
if [ test -e /bin/bash ]   # [ ]可以省略
then 
	echo "文件已存在。"
else
	echo "文件不存在。"
fi
# =====参数=====
# -e    --exist
# -r    --exist and read
# -w    --write
# -x  	--executable
# -s    -- 文件存在且至少有一个字符
# -d  	--dir, 文件存在且为目录
# -f    --file, 文件存在且为普通目录
# -c	--char, 文件存在且为字符型特殊文件
# -b    --block, 块特殊文件
```

## 循环

```sh
# =====for=====
for val in 1 2 3 4 5
do 
	echo "The value is: ${val} ."
done
# The value is: 1 .
# The value is: 2 . ...

for str in "this is a string ."
do 
	echo ${str}
done
# 输出 this is a string.

# =====while=====
intval=1
while(( ${intval}<=5 ))
do 
	echo ${intval}
	let "intval++"
done
```

## shell 函数

```sh
#!/bin/bash
test() 
{
	echo "this is my first function."  
    echo "第一个参数：${1}"    # ${} 
    echo "第二个参数：${2}"    
    echo "第十个参数：${10}"
    echo "参数个数：$# "
    echo "作为一个字符串输出所有参数：$*"
}
test 1 2 3 4 5 6 7 8 9 32   # 调用函数 test 并传入参数
```

函数也可以写成：

```sh
function test(){ }    # function 是个说明符，可以省略
```



## shell 输入输出重定向

文件描述符 0 通常表示标准输入`STDIN` ，1 是标准输出 `STDOUT` ，2 是标准错误 `STDERR`。

| 命令             | 说明                                            |
| ---------------- | ----------------------------------------------- |
| command > file   | 输出重定向到 file                               |
| command < file   | 输入重定向到 file，即从文件中获取输入，替代终端 |
| command >>  file | 以追加的重定向                                  |
| n > file         | n 为文件描述符                                  |
| n >> file        | 追加文件描述 n 到 file                          |
| n >& m           | 将输出文件m和n合并                              |
| n <& m           | 将输入文件m和n合并                              |
| << tag           | 将开始标记tag和结束标记tag之间的内容作为输入    |

* 实例

```sh
wc -l file   # 统计 file 的行数 
# 输出 2 file
wc -l < file
# 输出 2 ，不会输出文件名，因为它仅仅知道file中的内容
# 从 infile 中获取输入，传递给command1，执行后将结果输入到 outfile 中
command1 < infile > outfile
```

## shell 文件包含

和其他语言一样，shell 也可以包含外部脚本文件，这样可以很方便的封装一些公用的代码作为一个独立的文件。

* 实例

首先创建两个 shell 脚本文件

1.  test1.sh

```sh
#!/bin/bash
url="https://github.com/DaDaDavinc"
```

2. test2.sh

```sh
#!/bin/bash
# 1. 使用 . 来包含当前目录下的test1.sh文件
. ./test1.sh
# 2. 使用 source 来包含
source ./test1.sh

echo "my github url: ${url}"
```

## shell 中的 export

* 功能说明

  **设置或显示环境变量**

* 语法

  ```sh
  export [-fnp] [变量名]=[变量设置值]
  ```

* 参数

  ```sh
  -f  --function 表示 [变量名] 中是函数名
  -n  -- 删除指定的变量
  -p  -- 列出所有的shell赋予程序的环境变量
  ```

* 其他

  在 shell 中执行程序是时，shell 会提供一组环境变量。export 可以新增/修改/删除 环境变量，供后续的程序使用。export 的权限仅限于此次登陆操作。注销或重启一个窗口，export 命令给出的环境变量都不存在了。

*   实例
```sh
export PATH=/bin/bash:$PATH   # 在PATH中添加新路径
```
## bash 命令

```sh
bash test.sh
sh test.sh
./test.sh
```
以上三个命令都可以执行 shell 脚本文件。运行一个 shell 脚本会启动另外一个命令解释器，每个 shell 脚本有效地运行在 `父shell` 的 `子进程` 里。这些子进程能够并行的执行 shell 脚本中的多个子任务。

**具体操作**

1. 在 bash 命令行中执行命令 `export PATH=xxx:$PATH` ，或者在脚本文件中添加这一行代码
2. 然后 source 一下该脚本文件，使设置的环境变量生效

