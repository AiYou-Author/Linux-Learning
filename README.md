# Linux系统学习

# 1. Linux系统简介









## 1.1 linux文件目录

绝对路径:由根目录(/)开始写起的文件名或目录名称。

相对路径:相对于目前路径的文件名写法,反正开头不是 /
就属于相对路径的写法。

- . ：代表当前的目录，也可以使用 ./ 来表示
- .. ：代表上一层目录，也可以 ../ 来代表











## 1.2 用户管理与文件权限

#### 三个**核心文件**

- /etc/passwd:

  用户名:密码(x):UID:GID:用户信息说明栏:用户目录:Shell

  UID:

  ​	- 管理员(root)：具有系统所有权限(0)

  ​	- 系统用户：管理系统运行服务(1~999)

  ​	- 普通用户：具有系统一部分权限(1000以上)  

- /etc/shadow:

  用户名：加密密码：最后一次修改时间：最小修改时间间隔：密码有效期：密码需要变更前的警告天数：密码过期后的宽限时间：账号失效时间：保留字段

- /etc/group:

  组名:群组密码:GID(x):此群组支持的账号名称


#### 文件权限

| 用户权限     | 用户组权限   | 其他用户权限 |
| ------------ | ------------ | ------------ |
| 第1、2、3位  | 第4、5、6位  | 第7、8、9位  |
| r、w、x      | r、w、x      | r、w、x      |
| 读、写、执行 | 读、写、执行 | 读、写、执行 |



## 1.3 命令行与Shell



```shell
AiYou@Ubuntu:~$
```

- AiYou：“@”符号的左侧，它表示的是当前登录用户，上图使用的是 embedfire用户登录。
- @ ：分隔符号，可理解为at，表示embedfire用户at主机dev上。
- Ubuntu ：当前系统的主机名。
- “:” ：分隔符号。
- “~” ：冒号后表示用户当前所在的目录，此处的波浪线表示当前用户的家目录，即“~”的含义为/home/AiYou目录。
- “$”：命令提示符，Linux 用这个符号标识登录的用户权限等级。如果是超级用户，提示符就是“#”，如果是普通用户，提示符就是“$”。



### 常用命令归纳

命令行其实就是包含在/bin目录下的文件

通过 which 语句可以查询命令行的具体位置

#### 查询命令:

##### man

```shell
man [要查询的内容]
man ls
```







#### 目录操作类

##### cd：

- “~”：波浪号，表示为当前用户的 home 目录
- “.”：一个英文句号，表示当前目录
- “..” ：两个英文句号，表示当前目录的上一层目录
- “/” ：斜杠符号，表示为根目录
- “-” ：减号，不是目录，但作为cd命令的参数时可以返回上一次cd切换前的目录。

```shell
cd [目录名]

cd test
cd /home/embedfire/test

cd /           # 切换至根目录
ls             # 列出当前目录的内容（根目录）
cd -           # 返回至上一次的目录（家目录）
cd ..          # 切换至上一级目录
ls             # 列出当前目录的内容（home目录）
cd embedfire   # 切换至embedfire目录（请把embedfire换成自己的用户名）
```





##### ls:	输出当前目录下的内容

- -a:显示所有文件及目录（Is内定将文件名或目录名称开头为“.”的视为隐藏档，不会列出）
- -l：注意这是字母L的小写，除文件名称外，将文件型态、权限、拥有者、文件大小等信息详细列出
- -t:将文件依建立时间之先后次序列出
- -A:同-a，但不列出“！”（当前目录）及“.（父目录）
- -R：若目录下有文件，则该目录下的文件也会列出，即递归显示

```
ls -l
```

![未找到图片](https://s2.loli.net/2022/05/22/teR3PpqHDkAs2rw.jpg)

第一字段：文件属性

​		第一个字符代表文件的类型，字符“-” 表示该文件是一个普通文件；字 符“d”是dirtectory(目录)的首字符，表示该文件是一个目录。后面的九个字符，每三个为一组，分别表示文件拥有者 的权限、文件所属组拥有的权限以及其他用户拥有的 权限。字符“r”代表的是读（read）权限，字符“w”代表的 是写（write）权限，字符“x”代表的是执行（execute）权限。

第二字段：链接占用的节点/子目录的个数

​		对于文件来说，则表示该文件所具有的硬连接数。某个文件的第二字段如果等于1的话，代表没有其他指向该文件的硬连接。

​		对于文件夹来说，则表示该文件夹下有多少个子目录

第三字段和第四字段：文件拥有者和文件所在的组

第五字段：文件所占用的空间(以字节为单位)

第六字段：最近访问（修改）时间

第七字段：文件/文件夹名称





##### pwd:	输出当前目录





##### mkdir:	创建新目录

```shell
mkdir [-p] 目录名


ls                   # 列出当前目录的内容，此时没有testdir目录
mkdir testdir        # 创建目录testdir
ls                   # 列出当前目录的内容，发现多了testdir
mkdir other/test     # 创建目录other/test，因为other不存在，报错
mkdir -p other/test  # 使用 -p 选项创建目录other/test
ls                   # 列出当前目录的内容，发现other目录
ls other             # 列出other目录的内容，发现test目录
```







##### rmdir：删除空目录

```shell
rmdir [-p] 目录名

rmdir testdir        # 删除testdir目录
rmdir -p other/test  # 删除other/test目录，若删除后other为空，把other目录也删除掉
```

命令rmdir –p other/test/是删除空子目录test，但由于参数p的作用下，删除了test目录之后， 目录other也为空目录，因此other目录也会被一起删除。假如没有参数p的话， 那么other目录依然会存在









##### mv:

#### 文本操作类

##### touch:	在当前目录创建不存在的文件

```shell
touch 文件名

touch helloworld     # 在当前目录下创建一个名为helloworld的文件
ls                   # 显示当前目录下的内容
```





##### cat:	将编辑器里的内容输出到终端

```shell
cat 文件名
cat test123.txt
```





##### echo:	把终端变量内容打印出来

```shell
echo "字符串"
echo 字符串
echo $变量名

echo "test"    # 打印字符串test
echo test      # 打印字符串test
echo $PATH     # 打印环境变量PATH
echo "$PATH"   # 打印环境变量PATH
```

```shell
#输出重定向到文件
echo "test"	#打印字符串
echo test > file.txt	#将test内容输出重定向值file.txt
echo abc >>  file.txt	#将abc输出追加至file.txt
```

![未找到图片](https://s2.loli.net/2022/05/22/GgmfilAZaedLIUS.png)







wc:







##### rm: 	删除一个或多个文件或目录

```shell
rm -i	#删除前会逐一询问
rm -r	#将目录及包含的子目录全部删除
rm -f	#忽略不存在的文件，无需逐一确认
```

ln:

cp：

tar:

find:

grep:

#### 用户管理类

useradd/adduser:

usermod：

userdel/deluser：

passwd：

groupadd/addgroup:

groupdel/delgroup:

su:

#### 操作权限类

##### sudo

```shell
cd /home             # 切换至/home目录
touch test.txt       # 尝试在当前目录创建test.txt文件
sudo touch test.txt  # 使用sudo增加权限，创建test.txt文件

# 输入密码时终端也不会随着输入而显示星号（*），输入完回车即可！
# 输入密码时终端也不会随着输入而显示星号（*），输入完回车即可！
# 输入密码时终端也不会随着输入而显示星号（*），输入完回车即可！
rm test.txt          # 尝试删除test.txt文件
sudo rm test.txt     # 使用sudo增加权限，删除test.txt文件

touch test.txt       # 提示没有权限
sudo !!              # sudo加两个感叹号，重新使用sudo权限执行上一条命令
```





chmod

chown

chgrp

#### 磁盘管理类

df:

du:

mount：

umount:

#### 网络操作类

ifconfig

ping

#### 控制终端类

##### clear:清屏

```shell
clear
```





#### 开关机命令

reboot：重启

poweroff：关机

- ​	Debian: Ubuntu ; deepin
- ​	Fedora: RHEL ; CentOS
- ​	其他









## 1.4 编辑器







### gedit编辑器

```shell
gedit 要打开或新建的文件名

gedit /etc/apt/sources.list         #以普通用户身份打开配置文件
sudo gedit /etc/apt/sources.list    #使用sudo增加权限打开配置文件
```













### Vi/Vim编辑器







#### 退出Vim

1. 按下退出键“Esc”，Vim会进入到“一般模式”。
2. 输入英文冒号“:”，Vim会进入到“命令行模式”。
3. 输入强制退出命令“q!”，即字母“q”及英文叹号“!”。
4. 按回车执行命令，会退出Vim，返回到终端

#### 输入内容

1. 按下退出键“Esc”进入“一般模式”。
2. 输入一般命令“i”，即直接按字母“i”，进入“插入模式”，如下图所示。
3. 随意输入一些内容。
4. 按下退出键“Esc”再次进入“一般模式”。
5. 输入英文冒号“:”，Vim会进入到“命令行”模式。
6. 输入保存退出命令“wq”。
7. 按回车执行命令，会退出Vim，返回到终端。、

#### Vim的三种模式

- 一般模式（normal mode）：一般模式用来浏览文本，查找内容，但是不可以编辑，在该模式下的键盘输入会被当成快捷键， 如复制粘贴等。打开Vim时，默认是工作在一般模式。
- 插入模式（insert mode）：插入模式下具有普通编辑器的功能，该模式下的键盘输入会被当成文本内容。
- 命令行模式（command-line mode）：命令行模式支持保存、退出、替换等命令，以及Vim的高级功能。

- 在任意模式下，我们可以通过按键“Esc”进入到一般模式。
- 在一般模式下，通过按键“a” “i” “o” “O” “r” “R”等可进入到插入模式。
- 在一般模式下，通过按键“:”可进入到命令行模式。

#### 插入模式

| 快捷键 | 功能描述                                            |
| ------ | --------------------------------------------------- |
| i      | 在当前光标所在位置插入文本                          |
| a      | 在当前光标所在位置的下一个字符插入文本              |
| o      | 在光标所在位置后插入新行                            |
| r      | 替换当前光标所在位置的字符                          |
| R      | 可以替换当前光标所在位置之后的字符，按下“Esc”则退出 |
| ESC    | 退出插入模式                                        |

#### 一般模式

| 快捷键                 | 功能描述             |                                                              |
| ---------------------- | -------------------- | ------------------------------------------------------------ |
| 光标移动               | k / ↑                | 光标向上移动                                                 |
|                        | j / ↓                | 光标向下移动                                                 |
|                        | h / ←                | 光标向左移动                                                 |
|                        | l / →                | 光标向右移动                                                 |
|                        | PageUp               | 向上翻页                                                     |
|                        | PageDown             | 向下翻页                                                     |
|                        | nG                   | 跳转到第n行                                                  |
| 文本查找与替换         | /word                | 在文件中搜索关键字word                                       |
|                        | n                    | 查找下一个关键字                                             |
|                        | N                    | 查找上一个关键字                                             |
|                        | :1,$s/word1/word2/gc | 将文本中的所有关键字word1用word2进行替换，需要用户进行确认。（使用:1,$s/word1/word2/g则直接全部替换）。这实际是运行在命令模式。 |
| 撤销重做               | u                    | 撤销上一步的操作，等价于Windows的Ctrl+Z                      |
|                        | Ctrl+r               | 重做上一步的操作。                                           |
| 删除、剪切、复制、粘贴 | d                    | 删除光标所选的内容                                           |
|                        | dd                   | 删除当前行                                                   |
|                        | ndd                  | 删除光标后n行                                                |
|                        | x                    | 剪切光标选中的字符                                           |
|                        | y                    | 复制光标所选的内容                                           |
|                        | yy                   | 复制当前行                                                   |
|                        | nyy                  | 复制当前行后n行                                              |
|                        | p                    | 将复制的数据粘贴在当前行的下一行                             |
|                        | P                    | 将复制的数据粘贴在当前行的上一行                             |
| 区块操作               | v                    | 选择多个字符                                                 |
|                        | V                    | 可以选择多行                                                 |
|                        | ctrl+v               | 可以选择多列                                                 |

#### 命令行模式

| 快捷键       | 功能描述                                           |
| ------------ | -------------------------------------------------- |
| w            | 保存文档                                           |
| w <filename> | 另存为以<filename>为文件名的文档                   |
| r <filename> | 读取文件名为filename的文档                         |
| q            | 直接退出软件，前提是文档未做任何修改               |
| q!           | 不保存修改，直接退出软件                           |
| wq           | 保存文档，并退出软件。                             |
| set nu       | 在行首加入行号                                     |
| set nonu     | 不显示行号                                         |
| set hlsearch | 搜索结果高亮显示                                   |
| ! command    | 回到终端窗口，执行command命令，按回车键可切回vim。 |















## 1.5 Shell脚本编程

#### shell命令的本质

内置命令/外部命令

```shell
type cd
type pwd
type ifconfig
```

![image-20220522160225153](https://s2.loli.net/2022/05/22/rMKHgD7txNQsPEA.png)



创建外部命令

```shell
sudo vi hello.c

vi:
#include <sdtio.h>

int main()
{
        printf("hello world!\n");
        return 0;
}

gcc hello.c -o hello
sudo mv hello /usr/bin/
#此时hello就成为了外部命令
```

```shell
aiyou@ubuntu:~$ echo $PATH
/usr/local/sbin
:/usr/local/bin
:/usr/sbin:/usr/bin
:/sbin
:/bin
:/usr/games
:/usr/local/games
:/snap/bin
```

命令：shell首先去内存去寻找内置命令，其次在PATH环境变量中寻找外部命令



##### Shell脚本语言和C语言一样吗?

- 编译型语言：利用编译器将源文件翻译为可执行文件
- 解释型语言：其源代码不是直接翻译成机器语言，而是先翻译成中间代码，再由解释器对中间代码进行解释运行

##### 常用的Shell解释器有哪些?

/etc/shells

```shell
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
```

##### Shell编程

```shell
sudo vi hello.sh

vi:
#!/bin/bash         #写一个解释器
echo "hello world"

sudo chmod 777 hello.sh		#修改执行方式
./hello.sh
```

##### Shell启动方式

- 当程序执行

```shell
./hello.sh
```

- 指定解释器运行

```
/bin/sh hello.sh
```

- source和.

```
source hello.sh
. hello.sh
```



#### Shell脚本语法

##### 定义变量

- variable=value
- variable='value'
- variable="value"

##### 使用变量

- $variable
- ${variable}

##### 将命令的结果赋值给变量

- variable=\`command`
- variable=$(command)

```shell
#!/bin/bash

var="1234 567"
var1='${var}aa'
echo "$var1"			
#输出结果为 ${var}aa
```

```shell
#!/bin/bash

var="1234 567"
var1="${var}aa"
echo "$var1"			
#输出结果为 1234 567aa
```

```shell
#!/bin/bash

var="1234 567"
var1=`pwd`
var1=$(p)
echo "$var1"			
#输出结果为 /home/aiyou/Shell
```

##### 特殊变量

| 变量      | 含义                                                         |
| --------- | ------------------------------------------------------------ |
| $0        | 当前脚本的文件名。                                           |
| $n（n≥1） | 传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是 $1，第二个参数是 $2。 |
| $#        | 传递给脚本或函数的参数个数。                                 |
| $*        | 传递给脚本或函数的所有参数。                                 |
| $@        | 传递给脚本或函数的所有参数。当被双引号`" "`包含时，$@ 与 $* 稍有不同. |
| $?        | 上个命令的退出状态或者获取函数返回值。                       |
| $$        | 当前 Shell 进程 ID。对于 Shell 脚本，就是这些脚本所在的进程 ID。 |

```shell
#!/bin/bash

echo "$0"			
#输出结果为 ./test.sh
```

```shell
#!/bin/bash

echo "$1"
echo "$2"
#./test.sh 10 11
#输出结果为
#10
#11
```

```shell
#!/bin/bash

echo "$1"
echo "$2"
echo "$#" #传递给脚本或函数的参数个数。
#./test.sh 10 11 12 13 14
#输出结果为
#10
#11
#5
```

```shell
#!/bin/bash

echo "$1"
echo "$2"
echo "$*" #传递给脚本或函数的所有参数。
#./test.sh 10 11 12 13 14
#输出结果为
#10
#11
#10 11 12 13 14
```

```shell
#!/bin/bash

echo "$1"
echo "$2"
exit 111

#echo $? 上个命令的退出状态或者获取函数返回值。
#111
```

```shell
#!/bin/bash

echo "$$" #当前 Shell 进程 ID。对于 Shell 脚本，就是这些脚本所在的进程 ID。

#./test.sh 
#3283
```

##### 读取从键盘输入的数据

read

```shell
#!/bin/bash

read -p "input a:" a
read -p "input b:" b

echo $a
echo $b
```

##### 退出当前进程

exit

```shell
exit 111	
```

##### 对整数进行数学运算

(())

```shell
#!/bin/bash

read -p "input a:" a 
read -p "input b:" b

#变量定义需要使用$,中间引用变量不需要加$
var=$((a+b))
```

##### 检测某个条件是否成立

test expression和[ expression ]

| 选 项       | 作 用                                  |
| ----------- | -------------------------------------- |
| -eq         | 判断数值是否相等                       |
| -ne         | 判断数值是否不相等                     |
| -gt         | 判断数值是否大于                       |
| -lt         | 判断数值是否小于                       |
| -ge         | 判断数值是否大于等于                   |
| -le         | 判断数值是否小于到等于                 |
| -z  str     | 判断字符串 str 是否为空                |
| -n  str     | 判断字符串str是否为非空                |
| =和==       | 判断字符串str是否相等                  |
| -d filename | 判断文件是否存在，并且是否为目录文件。 |
| -f filename | 判断文件是否存在，井且是否为普通文件。 |

```shell
#! /bin/bash

read -p "input a:" a
read -p "input b:" b

#前面部分成立，执行后部分
test $a -eq $b && echo "a=b"

#前面部分不成立，执行后部分
test $a -eq $b || echo "a!=b"
[ $a -eq $b ] || echo "a!=b"
```

##### 管道

command1 | command2

```shell
aiyou@ubuntu:~/Shell$ ls | grep test.sh 
test.sh
```

##### if语句

> if  condition
> then
> ​    statement(s)
> fi

```shell
if [ $a -eq $b ]
then
        echo "a=b"
fi
```

##### if else 语句

> if  condition
> then
> statement1
> else
> statement2
> fi

```shell
if [ $a -eq $b ]
then
        echo "a=b"
else
        echo "a!=b"
fi
```

##### if elif else 语句

> if  condition1
> then
> statement1
> elif condition2
> then
> ​    statement2
>
> ……
> else
> statementn
> fi

```shell
if [ $a -eq $b ]
then
        echo "a=b"
elif [ $a -gt $b ]
then
        echo "a>b"
else
        echo "a<b"
fi
```

##### case in语句

> case expression in
> ​    pattern1)
> ​        statement1
> ​        ;;
> ​    pattern2)
> ​        statement2
> ​        ;;
> ​    pattern3)
> ​        statement3
> ​        ;;
> ​    ……
> ​    *)
> ​        statementn
> esac

```shell
#! /bin/bash

read -p "input a:" a
#read -p "input b:" b

case $a in
        1)
                echo "a=1"
                ;;
        2)
                echo "a=2"
                ;;
        *)
                echo "a!=1&&a!=2"
                ;;
esac
```

##### for in 循环

> for variable in value_list
> do
> ​    statements
> done

value_list 

- 直接给出具体的值
- 给出一个取值范围
- 使用命令的执行结果
- 使用 Shell 通配符
- 使用特殊变量

```shell
for n in {1..10}
do
        echo "$n"
done
```

```shell
for n in $(ls /bin/*sh)
do
        echo "$n"
done
```

```shell
for n in $*
do
        echo "$n"
done
```

##### while 循环

> while condition
> do
> ​    statements
> done

```shell
n=0
while (( n<10 ))
do
        echo "$n"
        n=$((n+1))
done
```

##### 函数

> function name() {
> ​    statements
> ​    [return value]
> }

```shell
function my_name()
{
        echo "Aiyou"
}

my_name
```















## 1.6 linux环境变量

##### linux为什么需要环境变量?

##### 全局变量VS环境变量

![image-20220523101914800](https://s2.loli.net/2022/05/23/QhA8avoE2lwWLOf.png)

在终端直接定义的变量，无法在/bin/bash创建的子进程进行访问

export $变量 便可以在子进程进行访问主进程的变量



##### Shell 配置文件

与 Bash Shell 有关的配置文件主要有

1. **/etc/profile**
2. <u>*~/.bash_profile*</u>
3. <u>*~/.bash_login*</u>
4. <u>*~/.profile*</u>
5. **~/.bashrc**
6. <u>*/etc/bashrc*</u>
7. *<u>/etc/bash.bashrc</u>*
8. **/etc/profile.d/*.sh**



##### Shell 执行顺序

/etc/profiles->~/.profile(~/.bash_profile、~/.bash_login)

当启动系统时，便会执行/etc/profiles

只有当前用户登录时，才会执行~/.profile

重新打开终端后，不会执行/etc/profiles->~/.profile；只会执行/etc/bash.bashrc和~/.bashrc





##### 修改配置文件

修改这两个配置文件，可以使得不同进程使用同一个变量

全部用户、全部进程共享:/etc/bash.bashrc

一个用户、全部进程共享:~/.bashrc



![image-20220523102556495](https://s2.loli.net/2022/05/23/UTtFqfleh5NgCm7.png)

在/etc/bash.bashrc 中定义cd变量

![image-20220523102645674](https://s2.loli.net/2022/05/23/M2qPzm6fuOSnsKa.png)





##### shell启动方式对变量的影响

子shell进程中执行:/bin/bash和./     	此时返回主进程后无法访问子进程中的变量

当前进程中执行:source和.		



![image-20220523102823178](https://s2.loli.net/2022/05/23/fNPUhJtCgFRIjlm.png)



![image-20220523103110571](https://s2.loli.net/2022/05/23/nmMvuJlhOeSp3z7.png)













## 1.7 apt及deb软件安装包

了解Linux软件包的组成

| 文件类型     | 保存目录       |
| ------------ | -------------- |
| 普通程序     | /usr/bin       |
| root权限程序 | /usr/sbin      |
| 程序配置文件 | /etc           |
| 日志文件     | /var/log       |
| 文档文件     | /usr/share/doc |

Linux软件包

- 源码包

  优点:

  - 开源免费

  - 自由裁剪功能
  - 修改源代码

  缺点:

  - 安装步骤繁琐
  - 编译时间长
  - 新手无法解决编译问题

- 二进制包

  优点:

  - 简单易用
  - 安装速度快

  缺点:

  - 无法阅读修改源码
  - 无法裁剪功能
  - 依赖性强

### deb包以及apt

##### 概念

Debian、Ubuntu、Deepin等Linux发行版的软件安装包。

```shell
# 使用dpkg手动安装deb包的命令
sudo dpkg -i xxxx.deb
```

 使用apt能够自动从互联网的软件仓库中搜索、安装、升级、卸载软件，它会咨询软件仓库， 并能安装软件时的模块及依赖问题。

```shell
sudo apt-get install 软件名
```







### rpm包以及yum

##### 概念

RedHat，Fedora，Centos等Linux发行版的软件安装包。

```shell
rpm -ivh xxxx.rpm
```

使用yum安装软件的命令如下，同样地，它会自动下载并完成安装：

```shell
yum install 软件名
```





|                | Debian派系发行版 | Redhat派系发行版 |
| -------------- | ---------------- | ---------------- |
| 软件包         | deb              | rpm              |
| 基础包管理工具 | dpkg             | rpm              |
| 上层包管理工具 | apt              | yum              |





### apt工具

- apt-get工具：主要负责软件包的的安装、卸载以及更新等事务。
- apt-cache工具：用于查询软件包的相关信息。
- apt-config工具：用于配置所有apt工具。



```shell
sudo apt-get install 软件包名		#安装软件包
```

```
sudo apt-get remove 软件包名		#卸载软件
```



| 命令                 | 作用                       |
| -------------------- | -------------------------- |
| apt install 软件包名 | 安装指定的软件包           |
| apt remove 软件包名  | 卸载指定的软件包           |
| apt update           | 更新软件源列表             |
| apt search 软件包名  | 根据关键字搜索对应的软件包 |
| apt show 软件包名    | 显示软件包的相关信息       |
| apt list             | 根据名称列出所有的软件包   |





### dpkg工具

##### 概念

底层的包管理工具，主要用于对已下载到本地和已经安装的deb包进行管理

##### 常用命令

```c
安装软件:dpkg -i xxxx.deb
```

```c
查看安装目录:dpkg -L xxxx
```

```
显示版本:dpkg -l xxxx
```

```
详细信息:dpkg -s xxxx
```

```
罗列内容:dpkg -c xxxx.deb
```

```
卸载软件:dpkg -r xxxx
```

###### deb包文件结构分析

- DEBIAN目录:

  - control文件:

    - Package:软件名称

    - Version:版本

    - Section:软件类别

    - Priority:对系统的重要性

    - Architecture:支持的硬件平台

    - Maintainer:软件包的维护者
    - Description:对软件的描述

  - preinst文件 : 安装之前执行的shell脚本
  - postinst文件 : 安装之后执行的shell脚本
  - prerm文件：卸载之前执行的shell脚本
  - postrm文件: 卸载之后执行的shell脚本
  - copyright文件:版权声明
  - changlog文件: 修改记录

- 软件具体安装目录:

  ​	视实际需求





##### 构建一个helloworld的deb包

演示:dpkg -b

其他:

dpkg-buildpackage

checkinstall

...



1. 首先创建helloworld文件夹，在文件内创建./usr/bin/helloworld.sh

2. 使用build.sh文件 

   ```shell
   ./build.sh helloworld helloworld.deb    #第一个变量输入路径，第二个输入文件名
   ```

3. 生成deb包，使用dpkg -i命令安装

```shell
##build.sh


#!/bin/bash

version="0.1.2"
author="Emdebfire"
package_name="$2"
package_dir="$1"

mkdir -p ./$package_dir/DEBIAN/

cat <<EOF > ./$package_dir/DEBIAN/changelog
AUTHOR:$author
VERSION:$version 
DATE:$(date -R)
EOF


cat <<EOF > ./$package_dir/DEBIAN/copyright
******************************************************************
  * @attention
  *
  * 实验平台:野火  i.MX6ULL开发板 
  * 公司    :http://www.embedfire.com
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
* * ******************************************************************
EOF

cat <<EOF > ./$package_dir/DEBIAN/control
Source:embedfire
Package:${package_name%.*}
Version:$version
Section: debug
Priority: optional
Architecture: amd64
Maintainer:$author 
Description: Embedfire Tools
EOF

cat <<EOF > ./$package_dir/DEBIAN/postinst
#!/bin/sh
echo "******************************************************************"
echo "welcome to use $package_name!"
echo "******************************************************************"
EOF

sudo chmod 775 ./$package_dir/DEBIAN/postinst

dpkg -b $package_dir $package_name
```



##### apt命令和apt-get命令

- apt是新版的包管理工具
- 解决apt-get命令过于分散的问题
- apt默认属性对用户友好(进度条、提示升级包数)













# 2. Linux下开发应用程序











## 2.1 搭建NFS环境

#### Ubuntu安装NFS 服务端

```
sudo apt install nfs-kernel-server -y
```

#### 配置NFS 服务端

(1)、打开/etc/exports文件

```
sudo vim /etc/exports
```

(2)、添加配置信息

```
/home/embedfire/workdir *(rw,sync,no_root_squash)
```

(3)、创建共享文件夹

```
sudo mkdir -p /home/embedfire/workdir
```

- /home/embedfire/workdir：指定分享文件名。
- *:所有网段都可以读写
- rw:读写权限
- sync：资料同步写入到内存与硬盘中
- no_root_squash:root用户具有挂载目录的全部操作操作权限

(4)、更新exports配置

```
sudo exportfs -arv
```

(5)、查看NFS共享情况

```
showmount -e
```

#### 开发板安装NFS客户端

```
sudo apt install nfs-common -y
```

#### 查看NFS服务器共享目录

```
showmount -e +”NFS服务端IP”
```

#### 挂载NFS文件系统

```
sudo mount -t nfs ”NFS服务端IP”:/home/embedfire/workdir /mnt
```

- -t nfs：指定挂载的文件系统格式为nfs
- /home/embedfire/workdir：指定NFS服务器的共享目录
- /mnt：本地挂载目录





















## 2.2 GCC

```shell
#在主机上执行如下命令
gcc -v          #查看gcc编译器版本
which gcc       #查看gcc的安装路径
```

- -o：小写字母“o”，指定生成的可执行文件的名字，不指定的话生成的可执行文件名为a.out。
- -E：只进行预处理，既不编译，也不汇编。
- -S：只编译，不汇编。
- -c：编译并汇编，但不进行链接。
- -g：生成的可执行文件带调试信息，方便使用gdb进行调试。
- -Ox：大写字母“O”加数字，设置程序的优化等级，如“-O0”“-O1” “-O2” “-O3”， 数字越大代码的优化等级越高，编译出来的程序一般会越小，但有可能会导致程序不正常运行。



### GCC编译工具链

gcc编译器（预处理，编译） binutils工具集（汇编，链接）

```shell
#直接编译成可执行文件
gcc hello.c -o hello

#以上命令等价于执行以下全部操作
#预处理，可理解为把头文件的代码汇总成C代码，把*.c转换得到*.i文件
gcc –E hello.c –o hello.i

#编译，可理解为把C代码转换为汇编代码，把*.i转换得到*.s文件
gcc –S hello.i –o hello.s

#汇编，可理解为把汇编代码转换为机器码，把*.s转换得到*.o，即目标文件
gcc –c hello.s –o hello.o

#链接，把不同文件之间的调用关系链接起来，把一个或多个*.o转换成最终的可执行文件
gcc hello.o –o hello
```













## 2.3 ARM-GCC

- 编译器运行在X86_64架构平台上，编译生成X86_64架构的可执行程序
- 编译器运行在ARM架构平台上，编译生成ARM架构的可执行程序

这种编译器和目标程序都是相同架构的编译过程，被称为 **本地编译** 。

而当前我们希望的是编译器运行在x86架构平台上，编译生成ARM架构的可执行程序，这种编译器和目标程序运行在不同架构的编译过程，被称为 **交叉编译**。



```shell
#在主机上执行如下命令
sudo apt install gcc-arm-linux-gnueabihf

#安装完成后使用如下命令查看版本
arm-linux-gnueabihf-gcc -v  或 arm-linux-gnueabihf-gcc --version
```



GCC编译器命名格式

| 字段 | 含义           |
| ---- | -------------- |
| arch | 目标芯片架构   |
| os   | 操作系统       |
| gnu  | C标准库类型    |
| eabi | 应用二进制接口 |
| hf   | 浮点模式       |

​		以我们安装的arm-linux-gnueabihf-gcc编译器为例，表示它的目标芯片架构为ARM，目标操作系统为Linux，使用GNU的C标准库即glibc，使用嵌入式应用二进制接口（eabi），编译器的浮点模式为硬浮点hard-float。而另一种名为arm-linux-gnueabi- gcc的编译器与它的差别就在于是否带“hf”，不带“hf”表示它使用soft-float模式。



### 交叉编译

使用arm的gcc编译工具编译hello.c

```shell
#以下命令在主机上运行
#在hello.c程序所在的目录执行如下命令
arm-linux-gnueabihf-gcc hello.c -o arm-hello
```





### 查看开发板的glibc库

```shell
#使用readelf查看开发板的libc.so.6类型
readelf -h /lib/libc.so.6
```

![未找到图片2|](https://s2.loli.net/2022/05/27/hNZRPs2n9gGKxlT.png)

表示开发板的glibc库文件libc.so.6为ARM架构的hard-float类型库

其对应的gcc编译器为arm-linux-gnueabihf-gcc







### 静态编译程序

使用静态编译程序，可以在硬浮点平台运行软浮点类型程序

```shell
#以下命令在主机上运行
#使用不带hf的编译器对hello.c进行静态编译，生成的程序名为hello_static
sudo arm-linux-gnueabi-gcc hello.c -o hello_static --static
```

使用静态编译得到的hello_static程序比动态编译的hello大，这是因为它自身包含了库文件













## 2.4 Linux下的helloworld



![未找到图片](https://s2.loli.net/2022/05/28/8aAbinr6zwjlNKJ.png)

- 第一部分，上面浅绿色外框部分为程序的编译。
- 第二部分，浅黄色外框部分为Linux内核提供的服务。
- 第三部分，橘色外框部分为glibc库提供的服务。
- 第四部分，浅灰色外框部分，为用户程序。



1. 预处理hello.c,主要是处理程序里面的文件包含、处理宏定义、条件编译。
2. 把c文件编译成为汇编文件(.s)，其中进行了词法分析，语法分析，语义分析、生成中间代码、对代码进行优化等工作。
3. 把汇编文件(.s)编译成可重定位文件(.o)。
4. 把可重定位文件(.o)链接成为可执行文件，其中链接可分为静态链接和动态链接

   - 静态链接:在编译阶段就会把所有用到的库打包到自己的可执行程序中,其优点是具有较好的兼容性，不依赖外部环境，但是生成的程序比较大。

   - 动态链接:在应用程序运行时，链接器去加载外部的共享库，并完成共享库和动态编译程序之间的链接。不同的程序可以共用代码库，节省内存空间。
5. 控制台输入./hello命令后，Shell会创建一个新的进程来执行该程序。fork()函数就是用于创建一个新的进程的。这里的进程可以先简单理解为程序的容器。
6. exeve()函数可以理解为向上一步新建的进程，填充一个可执行程序(hello)。
7. sys_execve()函数为linux系统调用,被exeve()函数调用，这里的系统调用可以理解为是操作系统系统开放给用户的最底层接口。
8. do_exeve()函数是sys_execve()函数的核心。
9. load_elf_binary()函数会去文件系统中读取hello程序到内存，然后判断它是否是动态链接的可执行程序，如果不是，则进一步判断是否是静态链接的文件。
10. ld-linux-xx.so是glibc库中的动态连接器。如果hello程序是动态链接程序，该动态链接器会去加载共享库，并完成共享库和程序的链接工作， 然后准备真正开始执行hell程序。
11. 相反，如果hello程序是静态编译的程序，则无需再加载链接共享库，直接开始准备执行hello程序。
    - 第10和11步分别执行之后.都会开始执行hello程序，_start是程序的真正入口，而该符号在glibc中。也就是说程序的真正入口在glibc。
12. __libc_start_main()也是glibc中的函数，用于在执行用户程序前进行一些初始化工作。
13. 调用用户程序中的mian()函数，开始执行printf打印函数。
14. 程序执行完了之后，调用glibc库中的_exit()函数，来结束当前进程















## 2.5 Makefile



### Makefile三要素是什么?

目标、依赖、命令



### 三要素的关系

目标：依赖的文件或是其他目标

<tab>命令1

<tab>命令2





#### make和Makefile是什么关系？

make工具:找出修改过的文件，根据依赖关系，找出受影响的相关文件，最后按照规则单独编译这些文件。

Makefile文件:记录依赖关系和编译规则。

![未找到图片](https://s2.loli.net/2022/05/30/dlP4sw5Er1nZgpB.png)





创建一个include文件，在include中添加头文件

```shell
aiyou@ubuntu:~/makefile/part_1$ sudo vi Makefile

vi:
targeta:targetb targetc
	echo "targeta"

targetb:
	echo "targetb"

targetc:
	echo "targetc"


aiyou@ubuntu:~/makefile/part_1$ sudo make
echo "targetb"
targetb
echo "targetc"
targetc
echo "targeta"
targeta
aiyou@ubuntu:~/makefile/part_1$ sudo make targetb
echo "targetb"
targetb
aiyou@ubuntu:~/makefile/part_1$ sudo touch targetb
aiyou@ubuntu:~/makefile/part_1$ sudo make targetb
make: “targetb”已是最新。

```



### 伪目标

​	前面我们在Makefile中编写的目标，在make看来其实都是目标文件，例如make在执行 的时候由于在目录找不到targeta文件，所以每次make targeta的时候，它都会去执行targeta的命令，期待执行后能得到名为targeta的 同名文件。如果目录下真的有targeta、targetb、targetc的文件，即假如目标文件和依 赖文件都存在且是最新的，那么make targeta就不会被正常执行了，这会引起误会。

​	为了避免这种情况，Makefile使用“.PHONY”前缀来区分目标代号和目标文件，并且这种目 标代号被称为“伪目标”，phony单词翻译过来本身就是假的意思。

​	也就是说，只要我们不期待生成目标文件，就应该把它定义成伪目标



.PHONY:可以指定伪目标

```shell
#在Makefile文件前加入.PHONY:targetb
aiyou@ubuntu:~/makefile/part_1$ sudo make targetb
echo "targetb"
targetb
```





### Makefile管理项目

```shell
sudo vim mp3.c
```

```c
#include<stdio.h>

void play()
{
        printf("Play the music!\r\n");
}

void stop()
{
        printf("Stop the music!\r\n");
}
```

```shell
sduo vim main.c
```

```c
#include<stdio.h>

int main()
{
        play();
        stop();
        return 0;
}
```

```shell
sudo vim Makefile

vim:
mp3:main.o mp3.o
	gcc main.o mp3.o -o mp3
main.o:
	gcc -c main.c -o main.o
mp3.o:
	gcc -c mp3.c -o mp3.o
	
sudo make
./mp3
```



### 默认规则

![未找到图片05|](https://s2.loli.net/2022/05/30/uqKjiTVeShMW18s.png)

.o文件默认使用.c文件来进行编译







### Makefile的变量、模式匹配

#### 变量

##### 系统变量和自定义变量

```shell
sudo vim Makefile_test

vim:
A?=456			#?=,空赋值，只有当变量为空时才可以赋值
A+=123			#+=,追加赋值，此时A输出为“456 123”
B=$(A)			#=,延迟赋值
B:=$(A)			#:=,立即赋值
.PHONY:all

all:
	echo "$(CC)"	#输出系统变量
	echo "$(B)"
	
sudo make -f Makefile_test
```

```shell
VAR_A = FILEA
VAR_B = $(VAR_A)
VAR_C := $(VAR_A)
VAR_A += FILEB
VAR_D ?= FILED
.PHONY:check
check:
   @echo "VAR_A:"$(VAR_A)
   @echo "VAR_B:"$(VAR_B)
   @echo "VAR_C:"$(VAR_C)
   @echo "VAR_D:"$(VAR_D)
```

执行完make命令 后，只有VAR_C是FILEA。这是因为VAR_B采用的延时赋值，只有当调用时，才会进行 赋值。当调用VAR_B时，VAR_A的值已经被修改为FILEA FILEB，因此VAR_B的变量值也就等于FILEA FILEB











##### 自动化变量

```shell
sudo vim Makefile_test

vim:
.PHONY:all

all:targeta targetb
        echo "$<"  	#输出第一个依赖文件
        echo "$^"	#输出全部的依赖文件
        echo "$@"	#输出目标

targeta:

targetb:

aiyou@ubuntu:~/makefile/part_3$ sudo make -f Makefile_test 
echo "targeta"
targeta
```

| 符号 | 意义                                           |
| ---- | ---------------------------------------------- |
| $@   | 匹配目标文件                                   |
| $%   | 与$@类似，但$%仅匹配“库”类型的目标文件         |
| $<   | 依赖中的第一个目标文件                         |
| $^   | 所有的依赖目标，如果依赖中有重复的，只保留一份 |
| $+   | 所有的依赖目标，即使依赖中有重复的也原样保留   |
| $?   | 所有比目标要新的依赖目标                       |









#### 模式匹配

```makefile
CC=gcc
TARGET=mp3
OBJS=main.o mp3.o

$(TARGET):$(OBJS)
        $(CC) $^ -o $@

main.o:
        $(CC) -c main.c -o main.o

mp3.o:
        $(CC) -c mp3.c -o mp3.o
.PHONY:clean
clean:
        rm mp3
```







%:匹配任意多个非空字符

```makefile
CC=gcc
TARGET=mp3
OBJS=main.o mp3.o

$(TARGET):$(OBJS)
        $(CC) $^ -o $@

#main.o:
#       $(CC) -c main.c -o main.o

#mp3.o:
#       $(CC) -c mp3.c -o mp3.o

%.o:%.c
       $(CC) -c $^ -o $@


.PHONY:clean
clean:
        rm mp3

```



#### Makefile条件分支

```
ifeq (var1,var2)
...
else
...
endif
```

```
ifneq (var1,var2)
...
else
...
endif
```

```makefile
ARCH ?= x86

ifeq ($(ARCH),x86)
        CC=gcc
else
        CC=arm-linux-gnueabihf-gcc
endif
TARGET=mp3
OBJS=main.o mp3.o

$(TARGET):$(OBJS)
        $(CC) $^ -o $@

#main.o:
#       $(CC) -c main.c -o main.o

#mp3.o:
#       $(CC) -c mp3.c -o mp3.o

#%.o:%.c
#       $(CC) -c $^ -o $@


.PHONY:clean
clean:
        rm mp3
```



#### Makefile 常用函数

**1.patsbust**:

模式字符串替换

```
$(patsubst 匹配规则, 替换规则, 输入的字符串)
```

```shell
$(patsubst %.c, build_dir/%.o, hello_main.c )
#函数的输出为：
build_dir/hello_main.o
#执行如下函数
$(patsubst %.c, build_dir/%.o, hello_main.xxx )
#由于hello_main.xxx不符合匹配规则"%.c"，所以函数没有输出
```

```shell
vim:
.PHONY: all
        
all:
        echo "$(patsubst  %.c,%.o,x.c.c bar.c)"
        
aiyou@ubuntu:~/makefile/part_5$ sudo make -f Makefile_function 
echo "x.c.o bar.o"
x.c.o bar.o
```





**2.notdir**:

notdir函数用于去除文件路径中的目录部分

```
$(notdir 文件名)
```

例如输入参数“./sources/hello_func.c”，函数执行后 的输出为“hell_func.c”，也就是说它会把输入中的“./sources/”路径部分去掉，保留文件名





**3.wildcard**:

wildcard函数用于获取文件列表，并使用空格分隔开

```shell
#在sources目录下有hello_func.c、hello_main.c、test.c文件
#执行如下函数
$(wildcard sources/*.c)
#函数的输出为：
sources/hello_func.c sources/hello_main.c sources/test.c
```





**4.foreach**:

$(foreach VAR,LIST,TEXT)

dirs := a b c d  
files := $(foreach dir,$(dirs),$(wildcard $(dir)/*))

```makefile
dirs:= a b c d
files:= $(foreach dir,$(dirs),$(wildcard $(dir)/*))

.PHONY: all
        
all:
        echo "$(files)"
```

```makefile
ARCH ?= x86

ifeq ($(ARCH),x86)
        CC=gcc
else
        CC=arm-linux-gnueabihf-gcc
endif



TARGET=mp3
BUILD_DIR=build
SRC_DIR=module1 module2

SOURCES=$(foreach dir,$(SRC_DIR),$(wildcard $(dir)/*.c))
OBJS=$(patsubst %.c,$(BUILD_DIR)/%.o,$(notdir $(SOURCES)))
VPATH=$(SRC_DIR)


$(BUILD_DIR)/$(TARGET):$(OBJS)
        $(CC) $^ -o $@

#main.o:
#       $(CC) -c main.c -o main.o

#mp3.o:
#       $(CC) -c mp3.c -o mp3.o

$(BUILD_DIR)/%.o:%.c | create_build
        $(CC) -c $^ -o $@



.PHONY:clean create_build
clean:
        rm -r $(BUILD_DIR)
create_build:
        mkdir -p $(BUILD_DIR)
```

#### 解决头文件依赖

1、写一个头文件，并把头文件添加到编译器的头文件路径中。

gcc -I +"头文件"

2、实时检查头文件的更新情况，一旦头文件发生变化，应该要重新编译所有相关文件。

gcc -MM 

```makefile
ARCH ?= x86

ifeq ($(ARCH),x86)
        CC=gcc
else
        CC=arm-linux-gnueabihf-gcc
endif



TARGET=mp3
BUILD_DIR=build
SRC_DIR=module1 module2
INC_DIR=include
CFLAGS=$(patsubst %,-I%,$(INC_DIR))
INCLUDES=$(foreach dir,$(INC_DIR),$(wildcard $(dir)/*.h))

SOURCES=$(foreach dir,$(SRC_DIR),$(wildcard $(dir)/*.c))
OBJS=$(patsubst %.c,$(BUILD_DIR)/%.o,$(notdir $(SOURCES)))
VPATH=$(SRC_DIR)


$(BUILD_DIR)/$(TARGET):$(OBJS)
        $(CC) $^ -o $@

#main.o:
#       $(CC) -c main.c -o main.o

#mp3.o:
#       $(CC) -c mp3.c -o mp3.o

$(BUILD_DIR)/%.o:%.c $(INCLUDES) | create_build
        $(CC) -c $< -o $@ $(CFLAGS)



.PHONY:clean create_build
clean:
        rm -r $(BUILD_DIR)
create_build:
        mkdir -p $(BUILD_DIR)

```











## 2.6文件操作与系统调用





### 1.存储设备文件系统

Windows下的FAT32、NTFS、exFAT

Linux下常用的ext2、ext3和ext4的类型格式

- FAT32格式：兼容性好， STM32等MCU也可以通过Fatfs支持FAT32文件系统，大部分SD卡或U盘出厂 默认使用的就是FAT32文件系统。它的主要缺点是技术老旧，单个文件不能超过4GB，非日志型文件系统。
- NTFS格式：单个文件最大支持256TB、支持长文件名、服务器文件管理权限等，而且NTFS是日志型 文件系统。但由于是日志型文件系统，会记录详细的读写操作，相对来说会加快FLASH存储器的损 耗。文件系统的日志功能是指，它会把文件系统的操作记录在磁盘的某个分区，当系统发生故障时，能够 尽最大的努力保证数据的完整性。
- exFAT格式：基于FAT32改进而来，专为FLASH介质的存储器 设计（如SD卡、U盘），空间浪费少。单个文件最大支持16EB，非日志文件系统。
- ext2格式：简单，文件少时性能较好，单个文件不能超过2TB。非日志文件系统。
- ext3格式：相对于ext2主要增加了支持日志功能。
- ext4格式：从ext3改进而来，ext3实际是ext4的子集。它支持1EB的分区，单个文件最大支 持16TB，支持无限的子目录数量，使用延迟分配策略优化了文件的数据块分配，允许自主控制是否使用日志的功能。
- jffs2和yaffs2格式： jffs2和yaffs2是专为FLASH类型存储器设计的文件 系统，它们针对FLASH存储器的特性加入了擦写平衡和掉电保护等特性。由于Nor、NAND FLASH类 型存储器的存储块的擦写次数是有限的（通常为10万次），使用这些类型的文件系统可以减少对存储器的损耗。

### 2.伪文件系统

#### procfs文件系统

```shell
#查看CPU信息
cat /proc/cpuinfo
#查看proc目录
ls /proc
```

![未找到图片04](https://s2.loli.net/2022/06/01/xDdQ4cgoFV7MjrH.png)

/proc包含了非常多以数字命名的目录，这些数字就是进程的PID号

| 文件名      | 作用                                                         |
| ----------- | ------------------------------------------------------------ |
| pid*        | *表示的是进程的 PID 号，系统中当前运行的每一个进程都有对应的一个目录，用于记录进程所有相关信息。对于操作系统来说，一个应用程序就是一个进程 |
| self        | 该文件是一个软链接，指向了当前进程的目录，通过访问/proc/self/目录来获取当前进程的信息，就不用每次都获取pid |
| thread-self | 该文件也是一个软链接，指向了当前线程，访问该文件，等价于访问“当前进程pid/task/当前线程tid”的内容。。一个进程，可以包含多个线程，但至少需要一个进程，这些线程共同支撑进程的运行。 |
| version     | 记录了当前运行的内核版本，通常可以使用命令“uname –r”         |
| cpuinfo     | 记录系统中CPU的提供商和相关配置信息                          |
| modules     | 记录了目前系统加载的模块信息                                 |
| meminfo     | 记录系统中内存的使用情况，free命令会访问该文件，来获取系统内存的空闲和已使用的数量 |
| filesystems | 记录内核支持的文件系统类型，通常mount一个设备时，如果没有指定文件系统并且它无法确定文件系统类型时，mount会尝试包含在该文件中的文件系统，除了那些标有“nodev”的文件系统。 |

```shell
#在主机上执行如下命令，查看bash的PID号
ps
```

```shell
#在主机上执行如下命令
#把目录中的数字改成自己bash进程的pid号
ls /proc/3042
```

![未找到图片06|](https://s2.loli.net/2022/06/01/AQhXsGFBp5ywc9J.png)

| 文件名    | 文件内容                                                     |
| --------- | ------------------------------------------------------------ |
| cmdline   | 只读文件，记录了该进程的命令行信息，如命令以及命令参数       |
| comm      | 记录了进程的名字                                             |
| environ   | 进程使用的环境变量                                           |
| exe       | 软连接文件，记录命令存放的绝对路径                           |
| fd        | 记录进程打开文件的情况，以文件描述符作为目录名               |
| fdinfo    | 记录进程打开文件的相关信息，包含访问权限以及挂载点，由其文件描述符命名。 |
| io        | 记录进程读取和写入情况                                       |
| map_files | 记录了内存中文件的映射情况，以对应内存区域起始和结束地址命名 |
| maps      | 记录当前映射的内存区域，其访问权限以及文件路径。             |
| stack     | 记录当前进程的内核调用栈信息                                 |
| status    | 记录进程的状态信息                                           |
| syscall   | 显示当前进程正在执行的系统调用。第一列记录了系统调用号       |
| task      | 记录了该进程的线程信息                                       |
| wchan     | 记录当前进程处于睡眠状态，内核调用的相关函数                 |

```shell
#在主机上执行如下命令
#把目录中的数字改成自己bash进程的pid号
cat /proc/3042/comm
```

![未找到图片07|](https://s2.loli.net/2022/06/01/W326QO8wVxgCbuE.png)







#### sysfs文件系统

procfs是“任务管理器”，那sysfs同procfs一样，也是一 个伪文件系统

sys目录下的文件/文件夹向用户提供了一些关于设备、内核模块、文件系统以及其他内核组件的信息

![未找到图片08|](https://s2.loli.net/2022/06/01/UrPef1csHx4XFuR.jpg)

| 文件名  | 作用                                                         |
| ------- | ------------------------------------------------------------ |
| block   | 记录所有在系统中注册的块设备，这些文件都是符号链接，都指向了/sys/devices目录。 |
| bus     | 该目录包含了系统中所有的总线类型，每个文件夹都是以每个总线的类型来进行命名。 |
| class   | 包含了所有在系统中注册的设备类型，如块设备，声卡，网卡等。文件夹下的文件同样也是一些链接文件，指向了/sys/devices目录。 |
| devices | 包含了系统中所有的设备，到跟设备有关的文件/文件夹，最终都会指向该文件夹。 |
| module  | 该目录记录了系统加载的所有内核模块，每个文件夹名以模块命名   |
| fs      | 包含了系统中注册文件系统                                     |







### 3.虚拟文件系统

![未找到图片09|](https://s2.loli.net/2022/06/01/4UAhbsBnrfVo6lz.png)

上图解构如下：

- 应用层指用户编写的程序，如我们的hello.c。
- GNU C库（glibc）即C语言标准库，例如在编译器章节介绍的libc.so.6文件，它 包含了printf、malloc，以及本章使用的fopen、fread、fwrite等文件操作函数。
- 用户程序和glibc库都是属于用户空间的，本质都是用户程序。
- 应用层的程序和glibc可能会调用到“系统调用层（SCI）”的函数，这些函数 是Linux内核对外提供的函数接口，用户通过这些函数向系统申请操作。例如，C库 的printf函数使用了系统的vsprintf和write函数，C库的fopen、fread、fwrite分别 调用了系统的open、read、w rite函数，具体可以阅读glibc的源码了解。
- 由于文件系统种类非常多，跟文件操作相关的open、read、write等函数经过虚 拟文件系统层，再访问具体的文件系统。

​	为了使不同的文件系统共存， Linux内核在用户层与具体文件 系统之前增加了虚拟文件系统中间层，它对复杂的系统进行抽象化，对用户提供了统 一的文件操作接口。







### 4.Linux系统调用

​	系统调用（System Call）是操作系统提供给用 户程序调用的一组“特殊”函数接口API，文件操作就是其中一种类型。

- 进程控制：如fork、clone、exit 、setpriority等创建、中止、设置进程优先级的操作。
- 文件系统控制：如open、read、write等对文件的打开、读取、写入操作。
- 系统控制：如reboot、stime、init_module等重启、调整系统时间、初始化模块的系统操作。
- 内存管理：如mlock、mremap等内存页上锁重、映射虚拟内存操作。
- 网络管理：如sethostname、gethostname设置或获取本主机名操作。
- socket控制：如socket、bind、send等进行TCP、UDP的网络通讯操作。
- 用户管理：如setuid、getuid等设置或获取用户ID的操作。
- 进程间通信：包含信号量、管道、共享内存等操作。





#### 常用文件操作系统（C标准库）

##### 1. fopen函数

fopen库函数用于打开或创建文件，返回相应的文件流。它的函数原型如下：

```c
#include <stdio.h>
FILE *fopen(const char *pathname, const char *mode);
```

- pathname参数用于指定要打开或创建的文件名。
- mode参数用于指定文件的打开方式，注意该参数是一个字符串，输入时需要带双引号：
- “r”：以只读方式打开，文件指针位于文件的开头。
- “r+”：以读和写的方式打开，文件指针位于文件的开头。
- “w”：以写的方式打开，不管原文件是否有内容都把原内容清空掉，文件指针位于文件的开头。
- “w+”： 同上，不过当文件不存在时，前面的“w”模式会返回错误，而此处的“w+”则会创建新文件。
- “a”：以追加内容的方式打开，若文件不存在会创建新文件，文件指针位于文件的末尾。与“w+”的区别是它不会清空原文件的内容而是追加。
- “a+”：以读和追加的方式打开，其它同上。
- fopen的返回值是FILE类型的文件文件流，当它的值不为NULL时表示正常，后续的fread、fwrite等函数可通过文件流访问对应的文件。

##### 2. fread函数

fread库函数用于从文件流中读取数据。它的函数原型如下：

```c
#include <stdio.h> 
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream); 
```



stream是使用fopen打开的文件流，fread通过它指定要访问的文件，它从该文件中读取nmemb项 数据，每项的大小为size，读取到的数据会被存储在ptr指向的数组中。fread的返回值为成功读取的项数（项的单位为size）。

##### 3. fwrite函数

fwrite库函数用于把数据写入到文件流。它的函数原型如下：

```c
#include <stdio.h> 
size_t fwrite(void *ptr, size_t size, size_t nmemb, FILE *stream); 
```

它的操作与fread相反，把ptr数组中的内容写入到stream文件流，写入的项数为nmemb，每项 大小为size，返回值为成功写入的项数（项的单位为size）。



##### 4. fclose函数

fclose库函数用于关闭指定的文件流，关闭时它会把尚未写到文件的内容都写出。因为标准 库会对数据进行缓冲，所以需要使用fclose来确保数据被写出。它的函数原型如下：

```
#include <unistd.h> 
int close(int fd); 
```



##### 5. fflush函数

fflush函数用于把尚未写到文件的内容立即写出。常用于确保前面操作的数据被写 入到磁盘上。fclose函数本身也包含了fflush的操作。fflush的函数原型如下：

```c
#include <stdio.h> 
int fflush(FILE *stream); 
```



##### 6. fseek函数

fseek函数用于设置下一次读写函数操作的位置。它的函数原型如下：

```c
#include <stdio.h> 
int fseek(FILE *stream, long offset, int whence); 
```

其中的offset参数用于指定位置，whence参数则定义了offset的意义，whence的可取值如下：

- SEEK_SET：offset是一个绝对位置。
- SEEK_END：offset是以文件尾为参考点的相对位置。
- SEEK_CUR：offset是以当前位置为参考点的相对位置。

##### 实验代码分析

```c
#include <stdio.h>
#include <string.h>

//要写入的字符串
const char buf[] = "filesystem_test:Hello World!\n";
//文件描述符
FILE *fp;
char str[100];


int main(void)
{
   //创建一个文件
   fp = fopen("filesystem_test.txt", "w+");
   //正常返回文件指针
   //异常返回NULL
   if(NULL == fp){
      printf("Fail to Open File\n");
      return 0;
   }
   //将buf的内容写入文件
   //每次写入1个字节，总长度由strlen给出
   fwrite(buf, 1, strlen(buf), fp);

   //写入Embedfire
   //每次写入1个字节，总长度由strlen给出
   fwrite("Embedfire\n", 1, strlen("Embedfire\n"),fp);

   //把缓冲区的数据立即写入文件
   fflush(fp);

   //此时的文件位置指针位于文件的结尾处，使用fseek函数使文件指针回到文件头
   fseek(fp, 0, SEEK_SET);

   //从文件中读取内容到str中
   //每次读取100个字节，读取1次
   fread(str, 100, 1, fp);

   printf("File content:\n%s \n", str);

   fclose(fp);

   return 0;
}
```

#### 文件操作（系统调用）

Linux提供的文件操作系统调用常用的有open、write、read、lseek、close等。

##### 1. open函数

```C
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
```

Linux使用open函数来打开文件，并返回该文件对应的文件描述符。函数参数的具体说明如下：

- pathname：要打开或创建的文件名；
- flag：指定文件的打开方式，具体有以下参数，见下表 flag参数值。

表 flag参数值

| 标志位   | 含义                                                     |
| -------- | -------------------------------------------------------- |
| O_RDONLY | 以只读的方式打开文件，该参数与O_WRONLY和O_RDWR只能三选一 |
| O_WRONLY | 以只写的方式打开文件                                     |
| O_RDWR   | 以读写的方式打开文件                                     |
| O_CREAT  | 创建一个新文件                                           |
| O_APPEND | 将数据写入到当前文件的结尾处                             |
| O_TRUNC  | 如果pathname文件存在，则清除文件内容                     |

主模式：

- O_RDONLY:只读模式
- O_WRONLY:只写模式
- O_RDWR:读写，模式

副模式：

- O_CREAT:当文件不存在，需要去创建文件
- O_APPEND:追加模式
- O_DIRECT:直接IO模式
- O_SYNC:同步模式
- O_NOBLOCK:非阻塞模式



C库函数fopen的mode参数与系统调用open的flags参数有如下表中的等价关系。

表 fopen的mode与open的flags参数关系

| fopen的mode参数 | open的flags参数                 |
| --------------- | ------------------------------- |
| r               | O_RDONLY                        |
| w               | O_WRONLY \| O_CREAT \| O_TRUNC  |
| a               | O_WRONLY \| O_CREAT \| O_APPEND |
| r+              | O_RDWR                          |
| w+              | O_RDWR \| O_CREAT \| O_TRUNC    |
| a+              | O_RDWR \| O_CREAT \| O_APPEND   |

- mode：当open函数的flag值设置为O_CREAT时，必须使用mode参数来设置文件 与用户相关的权限。mode可用的权限如下表所示，表中各个参数可使用“| ”来组 合。

表 文件权限

| \          | 标志位  | 含义                                     |
| ---------- | ------- | ---------------------------------------- |
| 当前用户   | S_IRUSR | 用户拥有读权限                           |
| \          | S_IWUSR | 用户拥有写权限                           |
| \          | S_IXUSR | 用户拥有执行权限                         |
| \          | S_IRWXU | 用户拥有读、写、执行权限                 |
| 当前用户组 | S_IRGRP | 当前用户组的其他用户拥有读权限           |
| \          | S_IWGRP | 当前用户组的其他用户拥有写权限           |
| \          | S_IXGRP | 当前用户组的其他用户拥有执行权限         |
| \          | S_IRWXG | 当前用户组的其他用户拥有读、写、执行权限 |
| 其他用户   | S_IROTH | 其他用户拥有读权限                       |
| \          | S_IWOTH | 其他用户拥有写权限                       |
| \          | S_IXOTH | 其他用户拥有执行权限                     |
| \          | S_IROTH | 其他用户拥有读、写、执行权限             |





##### 2. read函数

```C
#include <unistd.h>
ssize_t read(int fd, void *buf, size_t count);
```

read函数用于从文件中读取若干个字节的数据，保存到数据缓冲区buf中，并返 回实际读取的字节数，具体函数参数如下：

- fd：文件对应的文件描述符，可以通过fopen函数获得。另外，当一个程序 运行时，Linux默认有0、1、2这三个已经打开的文件描述符，分别对应了标准输入、标准输出、标准错误输出，即可以直接访问这三种文件描述符；
- buf：指向数据缓冲区的指针；
- count：读取多少个字节的数据。





##### 3. write函数

```c
#include <unistd.h>
ssize_t write(int fd, const void *buf, size_t count);
```

write函数用于往文件写入内容，并返回实际写入的字节长度，具体函数参数如下：

- fd：文件对应的文件描述符，可以通过fopen函数获得。
- buf：指向数据缓冲区的指针；
- count：往文件中写入多少个字节。





##### 4. close函数

```C
int close(int fd);
```

当我们完成对文件的操作之后，想要关闭该文件，可以调用close函数，来关闭该fd文件描述符对应的文件。







##### 5. lseek函数

lseek函数可以用与设置文件指针的位置，并返回文件指针相对于文件头 的位置。其函数原型如下：

```C
off_t lseek(int fd, off_t offset, int whence);
```

它的用法与flseek一样，其中的offset参数用于指定位置，whence参数则定义了offset的意义，whence的可取值如下：

- SEEK_SET：offset是一个绝对位置。
- SEEK_END：offset是以文件尾为参考点的相对位置。
- SEEK_CUR：offset是以当前位置为参考点的相对位置。





##### 6.sync函数

页缓存和回写

功能

强制把修改过的页缓存区数据写入磁盘

头文件

```
#include <unistd.h>
```

函数原型

```
void sync(void);
```

返回值

无









##### 实验代码

```C
#include <sys/stat.h>
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <string.h>

//文件描述符
int fd;
char str[100];


int main(void)
{
   //创建一个文件
   fd = open("testscript.sh", O_RDWR|O_CREAT|O_TRUNC, S_IRWXU);
   //文件描述符fd为非负整数
   if(fd < 0){
      printf("Fail to Open File\n");
      return 0;
   }
   //写入字符串pwd
   write(fd, "pwd\n", strlen("pwd\n"));

   //写入字符串ls
   write(fd, "ls\n", strlen("ls\n"));

   //此时的文件指针位于文件的结尾处，使用lseek函数使文件指针回到文件头
   lseek(fd, 0, SEEK_SET);

   //从文件中读取100个字节的内容到str中，该函数会返回实际读到的字节数
   read(fd, str, 100);

   printf("File content:\n%s \n", str);

   close(fd);

   return 0;
}
```













#### 常用头文件

在后面的学习中我们常常会用到以下头文件，此处进行简单说明，若想查看具体的头文件内容，使用locate命令找到该文件目录后打开即可：

- 头文件stdio.h：C标准输入与输出（standard input & output）头文件，我们经常使用的打印函数printf函数就位于该头文件中。
- 头文件stdlib.h：C标准库（standard library）头文件，该文件包含了常用的malloc函数、free函数。
- 头文件sys/stat.h：包含了关于文件权限定义，如S_IRWXU、S_IWUSR，以 及函数fstat用于查询文件状态。涉及系统调用文件相关的操作，通常都需要用到sys/stat.h文件。
- 头文件unistd.h：UNIX C标准库头文件，unix，linux系列的操 作系统相关的C库，定义了unix类系统POSIX标准的符号常量头文件，比如Linux标准的输入文件描述符（STDIN），标准输出文件描述符（STDOUT），还有read、write等系统调用的声明。
- 头文件fcntl.h：unix标准中通用的头文件，其中包含的相关函数有 open，fcntl，close等操作。
- 头文件sys/types.h：包含了Unix/Linux系统的数据类型的头文件，常用的有size_t











## 2.7 控制LED灯设备





#### 驱动程序

本质：为硬件设备创建相应的设备节点文件

创建设备文件时，规定好设备文件的使用方式。







#### 应用程序

根据驱动程序规定的设备文件使用方式去控制硬件







#### 控制硬件设备步骤

1、找出硬件设备所对应的设备节点文件

两个地方：

- /dev目录下

  对驱动程序熟悉的工程师可以使用，一个设备节点文件控制硬件全部特性

- /sys目录下

  业余工程师使用，一个设备节点文件只控制硬件的一个特性

  严格来说，它下面的文件是Linux内核导出到用户空间的硬件操作接口

2、找出驱动程序规定的设备文件使用方式







#### LED灯程序

设备节点文件：/sys/class/leds

往brightness文件写入一个数值，就能控制led灯的亮度

led亮度值:0~255





```c
#include <unistd.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>


#define RLED "/sys/class/leds/red/brightness"
#define GLED "/sys/class/leds/green/brightness"
#define BLED "/sys/class/leds/blue/brightness"

int main()
{
        int res=0;
        int r_fd,g_fd,b_fd;

        printf("This is the led demo\n");
    
        r_fd=open(RLED,O_WRONLY);
        if(r_fd<0){
                printf("Fail to open device!\n");
                exit(1);
            }
    
            g_fd=open(GLED,O_WRONLY);
            if(g_fd<0){
                    printf("Fail to open device!\n");
                    exit(1);
            }
    
            b_fd=open(BLED,O_WRONLY);
            if(b_fd<0){
                    printf("Fail to open device!\n");
                    exit(1);
            }
    
            while(1)
            {
                    write(r_fd,"255",3);
                    printf("1\n");
                    sleep(1);
                    write(r_fd,"0",1);
                    printf("0\n");
                    sleep(1);
          	}
     close(r_fd);
            close(g_fd);
            close(b_fd);
    
            return 0;
    
}    


```









## 2.8 控制蜂鸣器

GPIO子系统

需要手动导出控制蜂鸣器的GPIO操作接口

引脚:GPIO1_19，1代表组号，19是组内引脚编码

Linux系统引脚编号规则:(组号-1)*32+组内引脚编码。

GPIO1_19在Linux内核的引脚编号为19



![image-20220605163124070](https://s2.loli.net/2022/06/05/CQBnSrpUXEfWods.png)

导出gpio子系统硬件操作接口方法：

/sys/calss/gpio/export,把引脚编号写进去。

```shell
echo 19 > /sys/class/gpio/export
#查看目录的变化，增加了gpio19目录
ls /sys/class/gpio/
#把gpio19从用户空间中取消导出
echo 19 > /sys/class/gpio/unexport
#查看目录变化，gpio19目录消失了
ls /sys/class/gpio/
```

![image-20220605163310443](https://s2.loli.net/2022/06/05/mVlOqvyBtjHiYMJ.png)



gpio19/direction:控制芯片引脚的输入输出模式。

- in代表输入
- out代表输出

gpio19/value:控制输出电平

- 1代表高电平
- 0代表低电平





```c
#include <string.h>
#include <sys/stat.h>
#include <unistd.h>
#include <fcntl.h>
#include "includes/bsp_beep.h"


int beep_init(void)
{
   int fd;
   //index config
   fd = open("/sys/class/gpio/export", O_WRONLY);
   if(fd < 0)
      return 1 ;

   write(fd, BEEP_GPIO_INDEX, strlen(BEEP_GPIO_INDEX));
   close(fd);

   //direction config
   fd = open("/sys/class/gpio/gpio" BEEP_GPIO_INDEX "/direction", O_WRONLY);
   if(fd < 0)
      return 2;

   write(fd, "out", strlen("out"));
   close(fd);

   return 0;
}

int beep_deinit(void)
{
   int fd;
   fd = open("/sys/class/gpio/unexport", O_WRONLY);
   if(fd < 0)
      return 1;

   write(fd, BEEP_GPIO_INDEX, strlen(BEEP_GPIO_INDEX));
   close(fd);

   return 0;
}


int beep_on(void)
{
   int fd;

   fd = open("/sys/class/gpio/gpio" BEEP_GPIO_INDEX "/value", O_WRONLY);
   if(fd < 0)
      return 1;

   write(fd, "1", 1);
   close(fd);

   return 0;
}

int beep_off(void)
{
   int fd;

   fd = open("/sys/class/gpio/gpio" BEEP_GPIO_INDEX "/value", O_WRONLY);
   if(fd < 0)
      return 1;

   write(fd, "0", 1);
   close(fd);

   return 0;
}
```







## 2.9 检测按键输入

key按键的设备文件:

```
/dev/input/by-path/platform-gpio-keys-event
```

#### input子系统

- 按键
- 键盘
- 鼠标
- 触摸屏
- ...

#### input_event

- time:用来记录输入时间的时间戳
- type：用来记录输入事件的类型，比如按键输入类型、坐标输入类型.特殊类型:EV_SYN，同步事件，提醒我们及时处理已经发生的完成输入事件
- code:记录输入类型的具体事件代号，比如键盘发生按键输入类型事件时，记录键盘那个值被按下了。
- value：用来记录事件的具体值。比如在按键输入类型事件里，value值为1代表按键被按下，value值为0代表按键被松开















# 3. 系统编程







## 3.1 进程

```
#include <stdio.h>
#include <unistd.h>

int main()
{
        pid_t i;
        printf("befor fork!\r\n");
        i=fork();
        printf("after fork=%d\r\n",i);
        return 0;
}

```





### 启动新进程

在Linux中启动一个进程有多种方法，比如可以使用system()函数， 也可以使用fork()函数去启动

#### system()启动进程

system()函数是C标准库中提供的，它主要是提供了一种调用其它程序的简单方法。 

```c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    pid_t result;

    printf("This is a system demo!\n\n");

    /*调用 system()函数*/
    result = system("ls -l");

    printf("Done!\n\n");

    return result;
}
```

#### fork()函数启动进程

头文件

```
#include <unistd.h>
```

函数原型

```
pid_t fork(void);
```

执行fork函数后，会返回两次

在旧进程中返回时，返回值为新进程的PID

新进程返回为0



### 子进程偷梁换柱

#### exec函数族

​	exec系列函数，这个系列函数主要是用于替换进程的执行程序， 它可以根据指定的文件名或目录名找到可执行文件，并用它来取代原调用进程的数据段、代码段和堆栈段， 在执行完之后，原调用进程的内容除了进程号外，其他全部被新程序的内容替换。



常用后缀:

```
l：代表以列表形式传参(list)
v：代表以矢量数组形式传参(vector)
p：代表使用环境变量Path来寻找指定执行文件
e：代表用户提供自定义的环境变量
```

头文件：

```
#include <unistd.h>
```

函数原型:

```
int execl(const char *path, const char *arg, ...)
```

​	execl()函数用于执行参数path字符串所代表的文件路径（必须指定路径）， 接下来是一系列可变参数，它们代表执行该文件时传递过去的 `argv[0]、argv[1]… argv[n]` ， 最后一个参数必须用空指针NULL作为结束的标志。

```C
#include <stdio.h>
#include <unistd.h>
int main()
{
        pid_t result;

        result = fork();

        if(result==0)
        {
                execl("/bin/ls","ls","-l",NULL);
                printf("error\r\n");
                return 1;
        }
        return 0;
}
```

​	exec系列函数是直接将当前进程给替换掉的， 当调用exec系列函数后，当前进程将不会再继续执行， 所以示例程序中的“ **error** ”将不被输出，因为当前进程已经被替换了，一般情况下， exec系列函数函数是不会返回的，除非发生了错误。出现错误时， exec系列函数将返回-1，并且会设置错误变量errno。

```
int execlp(const char *file, const char *arg, ...)
```

```
#include <stdio.h>
#include <unistd.h>

int main()
{
        pid_t result;
        result = fork();
        if(result>0)
        {
                execlp("ls","ls","-l",NULL);
                printf("error\r\n");
                return 1;
        }

        return 0;
}
```



```
int execv(const char *path, char *const argv[])
```

```
#include <stdio.h>
#include <unistd.h>

int main()
{
        pid_t result;
        char *arg[]={"ls","-l",NULL};
        result = fork();

        if(result>0)
        {
                execv("/bin/ls",arg);
                printf("error\r\n");
                return 1;
        }

        return 0;
```



```
int execve(const char *path, char *const argv[],char *const envp[])
```

```C
#include <stdio.h>
#include <unistd.h>

int main()
{
        pid_t result;
        char *arg[]={"env",NULL};
        char *env[]={"PATH=/tmp","name=embedfire",NULL};
        result = fork();

        if(result>0)
        {
                execve("/usr/bin/env",arg,env);
                printf("error\r\n");
                return 1;
        }

        return 0;

}
```

返回值:

成功：不返回

失败：-1

- 名称包含 l 字母的函数（execl、execlp和execle）接收参数列表“list”作为调用程序的参数。
- 名称包含 p 字母的函数（execvp 和 execlp）可接受一个程序名作为参数， 它会在当前的执行路径和环境变量“PATH”中搜索并执行这个程序（即可使用相对路径）； 名字不包含p字母的函数在调用时必须指定程序的完整路径（即要求绝对路径）。
- 名称包含 v 字母的函数（execv、execvp 和 execve）的子程序参数通过一个数组“vector”装载。
- 名称包含 e 字母的函数（execve 和 execle）比其它函数多接收一个指明环境变量列表的参数， 并且可以通过参数envp传递字符串数组作为新程序的环境变量， 这个envp参数的格式应为一个以 NULL 指针作为结束标记的字符串数组， 每个字符串应该表示为“environment = virables”的形式。



#### 要点总结

- l后缀和v后缀必须两者选其一来使用

- p后缀和e后缀是可选的，可用可不用
- 组合后缀的相关函数还有很多，可自己进一步了解

exce函数有可能执行失败，需要预防

- 新程序的文件路径出错
- 传参或者是自定义环境变量时，没有加NULL
- 新程序没有执行权限









### 终止进程



```c
#include<unistd.h>
void _exit(int state);

#include <stdlib.h>
void exit(int state);
```

exit()和_exit()函数都是用来终止进程的，当程序执行到exit()或__exit()函数时， 进程会无条件地停止剩下的所有操作，清除包括PCB在内的各种数据结构，并终止当前进程的运行。

![proces014](https://s2.loli.net/2022/06/09/IdYEfLV73NatjXC.png)

_exit()函数的作用最为简单：直接通过系统调用使进程终止运行， 当然，在终止进程的时候会清除这个进程使用的内存空间，并销毁它在内核中的各种数据结构； 

exit()函数则在这些基础上做了一些包装，在执行退出之前加了若干道工序： 比如exit()函数在调用exit系统调用之前要检查文件的打开情况， 把文件缓冲区中的内容写回文件，这就是“清除I/O缓冲”。











### 等待进程

```
pid_t wait(int *wstatus);
```

​	wait()函数在被调用的时候，系统将暂停父进程的执行，直到有信号来到或子进程结束， 如果在调用wait()函数时子进程已经结束，则会立即返回子进程结束状态值。 子进程的结束状态信息会由参数wstatus返回，与此同时该函数会返子进程的PID， 它通常是已经结束运行的子进程的PID

通过宏定义来判断子进程退出的状态：

- WIFEXITED(status) ：如果子进程正常结束，返回一个非零值
- WEXITSTATUS(status)： 如果WIFEXITED非零，返回子进程退出码
- WIFSIGNALED(status) ：子进程因为捕获信号而终止，返回非零值
- WTERMSIG(status) ：如果WIFSIGNALED非零，返回信号代码
- WIFSTOPPED(status)： 如果子进程被暂停，返回一个非零值
- WSTOPSIG(status)： 如果WIFSTOPPED非零，返回一个信号代码





```c
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main()
{
    pid_t pid, child_pid;
    int status;

    pid = fork();                  //(1)

    if (pid < 0) {
        printf("Error fork\n");
    }
    /*子进程*/
    else if (pid == 0) {                  //(2)

        printf("I am a child process!, my pid is %d!\n\n",getpid());

        /*子进程暂停 3s*/
        sleep(3);

        printf("I am about to quit the process!\n\n");

        /*子进程正常退出*/
        exit(0);                          //(3)
    }
    /*父进程*/
    else {                                //(4)

        /*调用 wait，父进程阻塞*/
        child_pid = wait(&status);        //(5)

        /*若发现子进程退出，打印出相应情况*/
        if (child_pid == pid) {
            printf("Get exit child process id: %d\n",child_pid);
            printf("Get child exit status: %d\n\n",status);
        } else {
            printf("Some error occured.\n\n");
        }

        exit(0);
    }
}
```













### 进程组、会话、终端

#### 进程组

作用:对相同类型的进程进行管理

#### 进程组的诞生

- 在shell里面直接执行一个应用程序，对于大部分进程来说，自己就是进程组的首进程。进程组只有一个进程
- 如果进程调用了fork函数，那么父子进程同属一个进程组，父进程为首进程
- 在shell中通过管道执行连接起来的应用程序，两个程序同属一个进程组，第一个程序为进程组的首进程

进程组id：pgid，由首进程pid决定

#### 会话

作用：管理进程组

#### 会话的诞生

- 调用setsid函数，新建一个会话，应用程序作为会话的第一个进程，称为会话首进程
- 用户在终端正确登录之后，启动shell时linux系统会创建一个新的会话，shell进程作为会话首进程

会话id：会话首进程id，SID

#### 前台进程组

shell进程启动时，默认是前台进程组的首进程。

前台进程组的首进程会占用会话所关联的终端来运行，shell启动其他应用程序时，其他程序成为首进程

#### 后台进程组

后台进程中的程序是不会占用终端

在shell进程里启动程序时，加上&符号可以指定程序运行在后台进程组里面

ctrl+z

```shell
sleep 100 & //前台进程组变为后台进程组

sleep 100
ctrl + z
```

jobs:查看有哪些后台进程组

fg+job id可以把后台进程组切换为前台进程组

#### 终端

- 物理终端

  - 串口终端
  - lcd终端

- 伪终端

  - ssh远程连接产生的终端
  - 桌面系统启动的终端

- 虚拟终端

  linux内核自带的，ctrl+alt+f0~f6可以打开7个虚拟终端











### 守护进程

会话用来管理前后台进程组

会话一般关联着一个终端

当终端被关闭了之后，会话中的所有进程都会被关掉



#### 守护进程

守护进程不受终端影响，就算终端微退出，也可以继续在后台运行



#### 如何来写一个守护进程

1.创健一个子进程，父进程直接退出

​	方法：通过fork(函数

2.创建一个新的会话，摆脱终端的影响

​	方法：通过setsid0函数

3.改变守护进程的当前工作目录，改为"/"

​	方法：通过chrdir()函数

4.重设文件权限掩码

​	新建文件的权限受文件权限掩码影响

​	uamsk:022,000010010,只写

​	新建文件默认执行权限：666,110110110

​	真正的文件执行权限：666&-umask 

​	方法：通过umask()函数

5.关闭不需要的文件描述符

​	0,1,2:标准输入、输出、出错

​	方法:通过close0函数

```c
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <string.h>
#include <fcntl.h>

int main()
{
        pid_t pid;
        int fd,len,i,num;
        char *buf="the dameon is running\n";
        len =strlen(buf)+1;


        pid=fork();
        if(pid>0)
        {
                exit(0);
        }

        //创建新会话，摆脱终端影响
        setsid();

        //改变当前工作目录
        chdir("/");

        //重设文件权限掩码
        umask(0);

        for(int i=0;i<3;i++)
        {
                close(i);
        }

        while(1)
        {
                fd=open("/var/log/dameon.log",O_CREAT|O_WRONLY|O_APPEND,0666);
                write(fd,buf,len);
                close(fd);
                sleep(5);
        }
        return 0;
}
```





### ps命令详解



两个常用命令：

aux

axjf

- a:显示一个终端所有的进程
- u:显示进程的归属用户及内存使用情况
- x:显示没有关联控制终端的进程
- j:显示进程归属的进程组id、会话id、父进程id
- f:以ascii形式显示出进程的层次关系

#### ps aux

- user：进程是哪个用户产生的
- pid:进程的身份证号码
- %cpu:表示进程占用了cpu计算能力的百分比
- %mem:表示进程占用了系统内存的百分比
- vsz:进程使用的虚拟内存大小
- rss:进程使用的物理内存大小
- tty:表示进程关联的终端
- stat:表示进程当前状态
- start:表示进程的启动时间
- time:记录进程的运行时间
- command:表示进程执行的具体程序

#### ps axjf

- ppid:表示进程的父进程id
- pid:进程的身份证号码
- pgid:进程所在进程组的id
- sid：进程所在会话的id
- tty:表示进程关联的终端
- tpgid:值为-1，表示进程为守护进程
- stat:表示进程当前状态
- uid:启动进程的用户id
- time:记录进程的运行时间
- command:表示进程的层次关系

stat：常见状态

常见的状态有以下几种:

-D:不可被唤醒的睡眠状态,通常用于1/O情况。

-R:该进程正在运行。

-S:该进程处于睡眠状态,可被唤醒。

-T:停止状态,可能是在后台暂停或进程处于除错状态。

-X:死掉的进程

-Z:/僵尸状态。

-N:低优先级。

-S:进程是会话首进程。

-1:多线程(小写L)+位于后台。



#### 使用场景

关注进程本身:ps aux

关注进程间的关系:ps axjf













### 僵尸进程和托孤进程

进程的正常退出步骤:

- 子进程调用exit()函数退出
- 父进程调用wait()函数为子进程处理其他事情

 

#### 僵尸进程

子进程退出后，父进程没有调用wait()函数处理身后事，子进程变成僵尸进程

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>

int main()
{
        pid_t pid;
        pid=fork();

        if(pid<0)
        {
                perror("fail to fokr!\r\n");
                return -1;
        }
        else if(pid==0)
        {
                printf("child next now.\n");
                exit(0);
        }else
        {
                while(1);
        }
        return 0;
}
         
```





#### 托孤进程

父进程比子进程先退出，子进程变为孤儿进程，Linux系统会把子进程托孤给1号进程(init进程)



















## 3.2 管道



### 1.什么是进程间通信(ipc)



#### 进程间通信

- 数据传输
- 资源共享
- 事件通知
- 进程控制

#### Linux系统下的ipc

- 早期unix系统ipc
  - 管道
  - 信号
  - fifo
- system-v ipc（贝尔实验室）
  - system-v 消息队列
  - system-v 信号量
  - system-v 共享内存
- socket ipc（BSD）
- posix ipc(IEEE)
  - posix 消息队列
  - posix 信号量
  - posix 共享内存







### 2.无名管道



#### pipe函数

头文件：

```
#include <unistd.h>
```

函数原型:

```
int pipe(int pipefd[2]);
```

pipefd[0]为读文件描述符；pipefd[1]为写文件描述符

返回值:

成功:0

失败:-1

#### 特点

- 特殊文件(没有名字)，无法使用open，但是可以使用close。
- 只能通过子进程继承文件描述符的形式来使用
- write和read操作可能会阻塞进程
- 所有文件描述符被关闭之后，无名管道被销毁

#### 使用步骤

- 父进程pipe无名管道
- fork子进程
- close无用端口
- write/read读写端口
- close读写端口

```c
//part7

#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_DATA_LEN 256

int main()
{
	pid_t pid;
	int pipe_fd[2];
	int status;
	char buf[MAX_DATA_LEN];
	const char data[]="pipe test program";
	int real_read,real_write;

	memset((void*)buf,0,sizeof(buf));

	if(pipe(pipe_fd)<0)
	{
		printf("pipe create error\n");
		exit(0);
	}

	pid=fork();

	if(pid==0)
	{
		close(pipe_fd[1]);
		if(real_read==read(pipe_fd[0],buf,MAX_DATA_LEN)>0)
		{
			printf("%d bytes read from the pipe is %s\n",real_read,buf);	
		}

		close(pipe_fd[0]);
		exit(0);
	}else if(pid>0)
	{
		close(pipe_fd[0]);
		if(real_write=write(pipe_fd[1],data,strlen(data))!= -1)
		{
			printf("parent write %d bytes : %s\n",real_write,data);
		}	
		close(pipe_fd[1]);
		wait(&status);
		exit(0);
	}

}
```













### 3.有名管道

####  mkfifo函数

头文件：

```
#include <sys/types.h>
#include <sys/stat.h>
```

函数原型:

```
int mkfifo(const char *filename,mode_t mode)
```

输入：文件路径，打开权限

返回值:

成功:0

失败:-1

#### 特点

- 有文件名，可以使用open函数打开
- 任意进程间数据传输
- write和read操作可能会阻塞进程
- write具有"原子性"

#### 使用步骤

- 第一个进程mkfifo有名管道
- open有名管道，write/read数据
- close有名管道
- 第二个进程open有名管道，read/write数据
- close有名管道







#### 写进程

```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>

#define MAX_DATA_LEN 256

#define MY_FIFO "/tmp/myfifo"

int main(int argc,char* argv[])
{
	pid_t pid;
	int fd;
	int nwrite;
    //第一个参数为输入fifo的数据
	char *buf=argv[1];

	if(argc<2)
	{
		printf("未输入参数!\n");
		exit(1);
	}
	
    //判断有名管道是否存在
	if(access(MY_FIFO,F_OK)==-1)
	{
		if((mkfifo(MY_FIFO,0666)<0) && (errno!=EEXIST))
		{
			printf("cannot create fifo file\n");
			exit(1);
		}
	}

	fd=open(MY_FIFO,O_WRONLY);

	if(fd<0)
	{
		printf("open fifo file error\n");
		exit(1);
	}

	if((nwrite=write(fd,buf,sizeof(buf))>0))
	{
		printf("write '%s' to FIFO\n",buf);
	}

	close(fd);
	exit(0);

}
```









#### 读进程

```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>
#include <string.h>

#define MY_FIFO "/tmp/myfifo"

#define MAX_BUFFER_SIZE PIPE_BUF

int main(int argc,char *argv[])
{
	char buff[MAX_BUFFER_SIZE];
	int fd;
	int nread;

	//判断有名管道是否存在
	if(access(MY_FIFO,F_OK)==-1)
	{
		if((mkfifo(MY_FIFO,0666)<0)&& (errno != EEXIST))
		{
			printf("cannot create fifo file\n");
			exit(1);
		}
	}
	
	fd=open(MY_FIFO,O_RDONLY);

	if(fd==-1)
	{
		printf("open fifo file error!\n");
		exit(1);
	}

	while (1)
	{
		memset(buff,0,sizeof(buff));
		if((nread=read(fd,buff,MAX_BUFFER_SIZE))>0)
		{
			printf("Read '%s' from FIFO\n",buff);
		}
		
	}
	close(fd);
	exit(0);

}
```













## 3.3 信号



### 1.信号简介



#### 信号的基本概念

软件模拟中断，进程接受信号后做出相应响应

#### 怎么产生信号?

- 硬件
  - 执行非法指令
  - 访问非法内存
  - 驱动程序
  - ...

- 软件

  - 控制台:
    - ctrl+c:中断信号
    - ctrl+|:退出信号
    - ctrl+z:停止信号

  - kill命令
  - 程序调用kill()函数

#### 信号的处理方式：

- 忽略:进程当信号从来没有发生过
- 捕获:进程会调用相应的处理函数，进行相应的处理
- 默认:使用系统默认处理方式来处理信号









### 2.常用信号分析

![image-20220617104829478](https://s2.loli.net/2022/06/17/nGtf74SaFhIMACw.png)

| 信号名  | 信号编号 | 产生原因     | 默认处理方式        |
| ------- | -------- | ------------ | ------------------- |
| SIGHUP  | 1        | 关闭终端     | 终止                |
| SIGINT  | 2        | ctrl+c       | 终止                |
| SIGQUIT | 3        | ctrl+\       | 终止+转储           |
| SIGABRT | 6        | abort()      | 终止+转储           |
| SIGPE   | 8        | 算术错误     | 终止                |
| SIGKILL | 9        | kill -9 pid  | 终止，不可捕获/忽略 |
| SIGUSR1 | 10       | 自定义       | 忽略                |
| SIGSEGV | 11       | 段错误       | 终止+转储           |
| SIGUSR2 | 12       | 自定义       | 忽略                |
| SIGALRM | 14       | alarm()      | 终止                |
| SIGTERM | 15       | kill pid     | 终止                |
| SIGCHLD | 17       | (子)状态变化 | 忽略                |
| SIGTOP  | 19       | ctrl+z       | 暂停，不可捕获/忽略 |

pkill命令

![image-20220617105048164](https://s2.loli.net/2022/06/17/CXE6P4OQw9Ro2ID.png)







### 3.signal_kill_raise函数



#### signal函数（**信号的回调函数**）

头文件:

```
include<signal.h>
```

函数原型:

```
typedef void (*sighandler_t)(int);

sighandler_t signal(int signum,sighandler_t handler);
```

参数:

- signum： 要设置的信号
- handler：
  - SIG_IGN：忽略
  - SIG_DFL：默认
  - void (*sighandler_t)(int)：自定义

返回值：

成功：上一次设置的handler

失败：SIG_ERR



```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>


void signal_handler(int sig)
{
	printf("\nthis signal number is %d\n",sig);

	if(sig==SIGINT)
	{
		printf("I have get SIGINT!\n\n");
		printf("The signal has been restored to the default processing mode!\n\n");
		signal(SIGINT,SIG_DFL);
	}

}


int main(int argc,char *argv[])
{
	printf("\nthis is an alarm test function\n\n");

	//设置信号处理的回调函数
	signal(SIGINT,signal_handler);

	while (1)
	{
		printf("waiting for the SIGINT signal, please enter ctrl +c \n");
		sleep(1);
	}
	
	exit(0);
}
```









#### kill函数

头文件:

```
#include <sys/types.h>
#include <signal.h>
```

原型函数:

```
int kill(pid_t pid,int sig);
```

参数:

- pid：进程id
- sig：要发送的信号

返回值:

成功：0

失败：-1

shell终端发送信号

```shell
kill -signal pid
eg: kill -2 7653
```



#### raise函数（发送信号）



头文件:

```
#include <signal>
```

原型函数:

```
int raise(int sig);
```

参数:

sig：发送信号

返回值：

成功：0

失败：非0







```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>


int main(int argc,char* argv[])
{
	pid_t pid;
	int ret;
	pid=fork();
	if(pid==0)
	{
		printf("Child is waiting for SIGSTOP signal!\n");
		
		//子进程暂停
		raise(SIGSTOP);
	}
	else
	{
		sleep(3);
		if((ret=kill(pid,SIGKILL))==0)
		{
			printf("parent kill %d\n",pid);
		}
		wait(NULL);
		printf("parent exit!\n");
		exit(0);
	}

}
```











### 4.信号集处理函数



#### 屏蔽信号集

屏蔽某些信号

- 手动
- 自动

#### 未处理信号集

信号如果被屏蔽，则记录在未处理信号集中

- 非实时信号(1~31)，不排队,只留一个
- 实时信号(34~64)，排队，保留全部

```c
#include <unistd.h>
#include <sys/types.h>
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>

void func(int signal)
{
	printf("hello\n");
	sleep(5);
	printf("world\n");
}



int main(int argc,char* argv[])
{
	signal(SIGINT,func);
	while(1);
	return 0;
}
```

![image-20220619162830520](https://s2.loli.net/2022/06/19/Ra52yKlkpFTosXM.png)







#### 信号集相关API

- int sigemptyset(sigset_t *set);
  - 将信号集合初始化为0
- int sigfillset(sigset_t *set);
  - 将信号集合初始化为1
- int sigaddset(sigset_t *set,int signum);
  - 将信号集合某一位设置为1
- int sigdelset(sigset_t *set,int signum);
  - 将信号集合某一位设置为0

- int sigprocmask(int how,const sigset_t *set,sigset_t *oldset);

  - 使用设置好的信号集去修改信号屏蔽集

  - 参数how:
    - SIG_BLOCK:屏蔽某个信号(屏蔽集 | set)
    - SIG_UNBLOCK:打开某个信号(屏蔽集 & (~set))
    - SIG_SETMASK:屏蔽集 = set

  - 参数oldset：保存就的屏蔽集的值，NULL表示不保存

```c
#include <unistd.h>
#include <sys/types.h>
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>

void func(int signal)
{
	//初始化设置信号集
	sigset_t set;
	//将信号集初始化为0
	sigemptyset(&set);
	//将sigint设为1,即不屏蔽
	sigaddset(&set,SIGINT);
	//将设置好的信号集保存
	sigprocmask(SIG_UNBLOCK,&set,NULL);
	
	printf("hello\n");
	sleep(5);
	printf("world\n");
	
}



int main(int argc,char* argv[])
{
	signal(SIGINT,func);
	while(1);
	return 0;
}
```



![image-20220619163353836](https://s2.loli.net/2022/06/19/hKqle38B9VCiv7M.png)









## 3.4 system-V 







### 1. system-V 消息队列



#### system-V ipc特点

- 独立于进程
- 没有文件名和文件描述符
- IPC对象具有key和ID

#### 消息队列用法

- 定义一个唯一key(ftok)
- 构造消息对象(msgget)
- 发送特定类型消息(msgsnd)
- 接受特定类型消息(msgrcv)
- 删除消息队列(msgctl)

#### ftok函数

功能：获取一个key

函数原型：

```
key_t ftok(const char *path,int proj_id)
```

参数：

- path：一个合法路径
- proj_id：一个整数

返回值：

成功：合法键值

失败：-1



#### msgget函数（获取消息队列ID）

功能：获取消息队列ID

函数原型：

```
int msgget(key_t key,int msgflg)
```

参数：

- key：消息队列的键值
- msgflg：
  - IPC_CREAT：如果消息队列不存在，则创建
  - mode：访问权限

返回值：

成功：该消息队列的ID

失败：-1



#### msgsnd函数（发送消息）

功能：发送消息到消息队列

函数原型：

```
int msgsnd(int msqid,const void *msgp,size_t msgsz,int msgflg);
```

参数：

- msqid：消息队列ID

- msgp：消息缓存区

  - struct msgbuf

    {

    ​	long mtype;	 //消息标识

    ​	char mtext[1]; //消息内容

    }

- msgsz：消息正文的字节数

- msgflg：

  - IPC_NOWAIT：非阻塞发送
  - 0：阻塞发送

返回值：

成功：0

失败：-1



#### msgrcv函数（接收消息）

功能：从消息队列读取消息

函数原型：

```
ssize_t msgrcv(int msqid,void *msgp,size_t msgsz,long msgtyp,int msgflg)
```

参数：

- msqid：消息队列ID

- msgp：消息缓存区

- msgsz：消息正文的字节数

- msgtyp：要接受消息的标识

- msgflg：

  - IPC_NOWAIT：非阻塞读取
  - MSG_NOERROR：截断消息

  - 0：阻塞读取

返回值：

成功：0

失败：-1



#### msgctl函数（设置消息队列相关属性）

功能：设置或获取消息队列的相关属性

函数原型：

```
int msgctl(int msgqid,int cmd,struct maqid_ds *buf)
```

- msgqid：消息队列的ID
- cmd
  - IPC_STAT：获取消息队列的属性信息
  - IPC_SET：设置消息队列的属性
  - IPC_RMID：删除消息队列

- buf：相关结构体缓冲区











#### 程序：接收message

```c
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/types.h>
#include <sys/msg.h>
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define BUFFER_SIZE 512

struct message
{
	long mytype;
	char mtext[BUFFER_SIZE];
};


int main(int argc,char* argv[])
{
	int qid;
	key_t key;
	struct message msg;

	//获取一个key
	key=ftok("/tmp",11);
	if(key==-1)
	{
		printf("ftok fail!\n");
		exit(1);
	}

	//获取消息队列ID
	qid=msgget(key,IPC_CREAT|0666);
	if(qid==-1)
	{
		printf("get msgget fail!\n");
	}
	while(1)
	{
		memset(msg.mtext,0,BUFFER_SIZE);
		if(msgrcv(qid,&msg,BUFFER_SIZE,0,0)<0)
		{
			printf("msgrcv");
			exit(1);
		}
		printf("The message fom process%ld :%s",msg.mytype,msg.mtext);

		if(strncmp(msg.mtext,"quit",4)==0)
		{
			break;
		}

	}
	if(msgctl(qid,IPC_RMID,NULL)<0)
	{
		printf("msgctrl");
		exit(1);
	}
	return 0;
	

}
```







#### 程序：发送message

```c
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/types.h>
#include <sys/msg.h>
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define BUFFER_SIZE 512

struct message
{
	long mytype;
	char mtext[BUFFER_SIZE];
};


int main(int argc,char* argv[])
{
	int qid;
	key_t key;
	struct message msg;

	//获取一个key
	key=ftok("/tmp",11);
	if(key==-1)
	{
		printf("ftok fail!\n");
		exit(1);
	}

	//获取消息队列ID
	qid=msgget(key,IPC_CREAT|0666);
	if(qid==-1)
	{
		printf("get msgget fail!\n");
	}
	while(1)
	{
		printf("pleas enter your message:\n");

		if(fgets(msg.mtext,BUFFER_SIZE,stdin)==NULL)
		{
			puts("no message");
			exit(1);
		}
		msg.mytype=getpid();

		if(msgsnd(qid,&msg,strlen(msg.mtext),0)<0)
		{
			printf("message posted");
			exit(1);
		}

		if(strncmp(msg.mtext,"quit",4)==0)
		{
			break;
		}

	}
	return 0;
	

}



```















### 2. system-V 信号量

#### 本质

计数器

#### 作用

保护共享资源

- 互斥
- 同步

#### 信号量用法

- 定义一个唯一key（ftok）
- 构造一个信号量(semget)
- 初始化信号量(semctl SETVA)
- 对信号量进行P/V操作(semop)
- 删除信号量(semctl RMID)



#### semget函数（获取信号量ID）

功能：获取信号量的ID

函数原型：

```
int semget(key_t key,int nsems,int semflg)
```

参数：

- key：信号量键值
- nsems：信号量数量
- semflg：
  - IPC_CREATE：信号量不存在则创建
  - mode：信号量的权限

返回值：

成功：信号量ID

失败：-1



#### semctl函数(设置信号量相关属性)

功能：获取或设置信号量的相关属性

函数原型：

```
int semctl(int semid,int semnum,int cmd,union semun arg)
```

参数：

- semid：信号量ID

- semnum：信号量编号

- cmd：

  - IPC_STAT：获取信号量的属性信息
  - IPC_SET：设置信号量的属性
  - IPC_RMID：删除信号量
  - IPC_SETVAL：设置信号量的值

- arg：

  union semun
  {
  int val;
  struct semid_ds *buf;
  }

返回值：

成功：由cmd类型决定

失败：-1



#### semop函数（进行PV操作）

函数原型：

```
int semop(int semid,struct sembuf *sops,size_t nsops)
```

参数：

- semid：信号量ID

- sops：信号量操作结构体数组

  struct sembuf

  {

  ​	short sem_num;	//信号量编号

  ​	short sem_op；//信号量P/V操作

  ​	short sem_flg;	//信号量行为，SEM_UNDO

  }

  - nsops：信号量数量

返回值：

成功：0

失败：-1



#### 程序

```c
/*sem.h*/

#include <unistd.h>
#include <sys/ipc.h>
#include <sys/types.h>
#include <sys/sem.h>
#include <sys/stat.h>
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <fcntl.h>

#define BUFFER_SIZE 512

union semun
{
	int val;
	struct semid_ds *buf;
};


//初始信号量
void init_sem(int semid,int val)
{
	union semun sem_union;
	sem_union.val=val;
	int state = semctl(semid,0,SETVAL,sem_union);
	if(state==-1)
	{
		printf("set fail!\n");
		exit(1);
	}
}
//删除信号量
void delete_sem(int semid)
{
	union semun sem_union;
	int state = semctl(semid,0,IPC_RMID,sem_union);
	if(state==-1)
	{
		printf("delete fail!\n");
		exit(1);
	}
}
//P操作
void sem_p(int semid)
{
	struct sembuf sem_buf;
	sem_buf.sem_num=0;
	sem_buf.sem_op=-1;
	sem_buf.sem_flg=SEM_UNDO; 
	int state = semop(semid,&sem_buf,1);
	if(state==-1)
	{
		printf("P fail!\n");
		exit(1);
	}
}
//V操作
void sem_v(int semid)
{
	struct sembuf sem_buf;
	sem_buf.sem_num=0;
	sem_buf.sem_op=1;
	sem_buf.sem_flg=SEM_UNDO; 
	int state = semop(semid,&sem_buf,1);
	if(state==-1)
	{
		printf("V fail!\n");
		exit(1);
	}
}
```

```c
#include "sem.h"

int main()
{
    key_t key = 6666;
    int pid;
    int qid;

    //获取信号量ID
    qid = semget(key, 1, IPC_CREAT | 0666);
    if(qid==-1)
    {
        printf("semget error!\n");
        exit(1);
    }

    //初始化信号量
    init_sem(qid,0);

    pid=fork();

    if(pid==0)
    {
        printf("child process is running!\n");
        sleep(3);
        printf("child process is running!\n");
        sem_v(qid);//释放资源
        
    }
    else{
        sem_p(qid);//阻塞
        printf("parent process is running!\n");
        sem_v(qid);
        delete_sem(qid);
    }
    return 0;
}
```

















### 3. system-V 共享内存 

#### 作用

高效率传输大量数据

#### 共享内存用法

- 定义一个唯一key（ftok）
- 构造一个共享内存对象(shmget)
- 共享内存映射(shmat)
- 解除共享内存映射(shmdt)
- 删除共享内存(shmctl RMID)



#### shmget函数（获取共享内存对象的ID）

功能：获取共享内存对象的ID

函数原型：

```
int shmget(key_t key,int size,int shmflg)
```

参数：

- key：共享对象键值
- nsems：共享内存大小
- shmflg：
  - IPC_CREATE：共享内存不存在则创建
  - mode：共享内存的权限

返回值：

成功：共享内存ID

失败：-1



#### shmat函数（映射共享内存）

功能：映射共享内存

函数原型：

```
int shmat(int shmid,const void *shmaddr,int shmflg)
```

参数：

- shmid：共享内存ID
- shmaddr：映射地址，NULL为自动分配
- shmflg：
  - SHM_RDONLY：只读方式映射
  - 0：可读可写

返回值：

成功：共享内存首地址

失败：-1



#### shmdt函数（解除共享内存映射）

功能：解除共享内存映射

函数原型：

```
int shmdt(const void *shmaddr)
```

参数：

shmaddr：映射地址

返回值：

成功：0

失败：-1



#### shmctl函数（获取或设置共享内存的相关属性）

功能：获取或设置共享内存的相关属性

函数原型：

```
int shmctl(int shmid,int cmd,struct shmid_ds *buf)
```

参数：

- shmid：共享内存ID

- cmd：
  - IPC_STAT：获取共享内存的属性信息
  - IPC_SET：设置共享内存的属性
  - IPC_RMID：删除共享内存

- buf：属性缓冲区


返回值：

成功：由cmd类型决定

失败：-1







#### 程序

```c
#include "sem.h"
#include <sys/shm.h>
int main()
{
    key_t key = 6666;
    key_t key1 = 7777;
    char* addr;//映射共享内存的地址
    int pid;
    int qid;
    int shm_id;

    //获取信号量ID
    qid = semget(key, 1, IPC_CREAT | 0666);
    if (qid == -1)
    {
        printf("semget error!\n");
        exit(1);
    }
    //构建共享内存对象
    shm_id=shmget(key1,1024,IPC_CREAT|0666);

    //初始化信号量
    init_sem(qid, 0);

    pid = fork();

    if (pid == 0)
    {
        printf("child process is running!\n");
        sleep(3);
        //映射共享内存
        addr=shmat(shm_id,NULL,0);
        char buffer[11]="helloworld";
        //向共享内存中写入
        memcpy(addr,buffer,11);
        printf("child process is running!\n");
        sem_v(qid);
    }
    else
    {
        sem_p(qid);
        printf("parent process is running!\n");
        //映射共享内存
        addr=shmat(shm_id,NULL,0);
        //读出共享内存
        printf("You wrote:%s\n",addr);
        //解除共享内存映射
        shmdt(addr);
        //删除共享内存
        shmctl(shm_id,IPC_RMID,NULL);


        sem_v(qid);
        delete_sem(qid);

        
    }

    return 0;
}
```











## 3.5 线程



### 1.线程和进程

进程是资源管理的最小单位，线程是程序执行的最小单位



### 2.创建线程

POSIX标准的线程库pthread

```c
#include <pthread.h>
```



pthread_create()函数是用于创建一个线程

```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                    void *(*start_routine) (void *), void *arg);
```

- thread：指向线程标识符的指针。
- attr：设置线程属性，具体内容在下一小节讲解。
- start_routine：start_routine是一个函数指针，指向要运行的线程入口，即线程运行时要执行的函数代码。
- arg：运行线程时传入的参数。
- 返回值：若线程创建成功，则返回0。若线程创建失败，则返回对应的错误代码。



### 3.线程属性

```c++
typedef struct
{
    int                   etachstate;      //线程的分离状态
    int                   schedpolicy;     //线程调度策略
    structsched_param     schedparam;      //线程的调度参数
    int                   inheritsched;    //线程的继承性
    int                   scope;           //线程的作用域
    size_t                guardsize;       //线程栈末尾的警戒缓冲区大小
    int                   stackaddr_set;   //线程的栈设置
    void*                 stackaddr;       //线程栈的位置
    size_t                stacksize;       //线程栈的大小
}pthread_attr_t;
```

| API                            | 描述                                 |
| ------------------------------ | ------------------------------------ |
| pthread_attr_init()            | 初始化一个线程对象的属性             |
| pthread_attr_destroy()         | 销毁一个线程属性对象                 |
| pthread_attr_getaffinity_np()  | 获取线程间的CPU亲缘性                |
| pthread_attr_setaffinity_np()  | 设置线程的CPU亲缘性                  |
| pthread_attr_getdetachstate()  | 获取线程分离状态属性                 |
| pthread_attr_setdetachstate()  | 修改线程分离状态属性                 |
| pthread_attr_getguardsize()    | 获取线程的栈保护区大小               |
| pthread_attr_setguardsize()    | 设置线程的栈保护区大小               |
| pthread_attr_getscope()        | 获取线程的作用域                     |
| pthread_attr_setscope()        | 设置线程的作用域                     |
| pthread_attr_getstack()        | 获取线程的堆栈信息（栈地址和栈大小） |
| pthread_attr_setstack()        | 设置线程堆栈区                       |
| pthread_attr_getstacksize()    | 获取线程堆栈大小                     |
| pthread_attr_setstacksize()    | 设置线程堆栈大小                     |
| pthread_attr_getschedpolicy()  | 获取线程的调度策略                   |
| pthread_attr_setschedpolicy()  | 设置线程的调度策略                   |
| pthread_attr_setschedparam()   | 获取线程的调度优先级                 |
| pthread_attr_getschedparam()   | 设置线程的调度优先级                 |
| pthread_attr_getinheritsched() | 获取线程是否继承调度属性             |
| pthread_attr_getinheritsched() | 设置线程是否继承调度属性             |



**初始化线程对象**

使用pthread_attr_init()函数可以初始化线程对象的属性

```c
int pthread_attr_init(pthread_attr_t *attr);
```

- attr：指向一个线程属性的指针
- 返回值：若函数调用成功返回0，否则返回对应的错误代码。



**销毁对象**

```c
int pthread_attr_destroy(pthread_attr_t *attr);
```

- attr：指向一个线程属性的指针
- 返回值：若函数调用成功返回0，否则返回对应的错误代码。



**线程的分离状态**

线程是可结合的，或者是可分离的

可结合的线程是能够被其他线程收回其资源和杀死的，在被线程回收之前，资源是不可释放的。

可分离的线程是不能被其他现场回收和杀死的，其资源在终止时自动释放。

进程中的线程可以**调用pthread_join()函数来等待某个线程的终止，获得该线程的终止状态，并收回所占的资源**。 如果对线程的返回状态不感兴趣，可以将rval_ptr设置为NULL。

```c
int pthread_join(pthread_t tid, void **rval_ptr)；
```



**调用pthread_detach()函数将此线程设置为分离状态**，设置为分离状态的线程在线程结束时， 操作系统会自动收回它所占的资源。设置为分离状态的线程，不能再调用pthread_join()等待其结束。

```c
int pthread_detach(pthread_t tid)；
```



想要获取某个线程的分离状态，那么可以通过**pthread_attr_getdetachstate()函数获取**：

```c
int pthread_attr_getdetachstate(const pthread_attr_t *attr, int *detachstate);
```

若函数调用成功返回0，否则返回对应的错误代码。

参数说明：

- attr：指向一个线程属性的指针。
- detachstate：如果值为PTHREAD_CREATE_DETACHED，则表示线程是分离状态， 如果值为PTHREAD_CREATE_JOINABLE则表示线程是结合状态。







**线程栈**

线程栈是非常重要的资源，它可以存放函数形参、局部变量、线程切换现场寄存器等数据。

默认的线程栈大小是1M，线程栈的大小可调

```c
int pthread_attr_setstacksize(pthread_attr_t *attr, size_t stacksize);
int pthread_attr_getstacksize(const pthread_attr_t *attr, size_t *stacksize);
```





### 4.线程退出

在线程创建后，系统就开始运行相关的线程函数，在该函数运行完之后，该线程也就退出了， 这是线程的一种隐式退出的方法

退出线程的方法是使用**pthread_exit()函数**，让线程显式退出

**exit()函数的作用是使调用进程终止**，而一个进程往往包含多个线程，因此，在使用exit()之后， 该进程中的所有线程都会被退出

```c
void pthread_exit(void *retval);
```

参数说明：

- retval：如果retval不为空，则会将线程的退出值保存到retval中，如果不关心线程的退出值，形参为NULL即可。



线程种类似于wait()的函数，**线程等待某个线程的退出，并获得他的退出值**。

```c
int pthread_join(pthread_t tid, void **rval_ptr)；
```

如果某个线程想要等待另一个线程退出，并且获取它的退出值，那么就可以使用pthread_join()函数完成， 以阻塞的方式等待thread指定的线程结束，当函数返回时，被等待线程的资源将被收回，如果进程已经结束， 那么该函数会立即返回。并且thread指定的线程必须是可结合状态的，该函数执行成功返回0，否则返回对应的错误代码。

参数说明：

- thread: 线程标识符，即线程ID，标识唯一线程。
- retval: 用户定义的指针，用来存储被等待线程的返回值。



需要注意的是一个可结合状态的线程所占用的内存仅当有线程对其执行立pthread_join()后才会释放，因此为了避免内存泄漏， **所有线程的终止时，要么已被设为DETACHED，要么使用pthread_join()来回收资源。**



#### 程序

```c++
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void *test_thread(void *arg)
{
    int num=(unsigned long long)arg;
    printf("This is test thread,arg is %d\n",num);
    sleep(5);
    pthread_exit(NULL);
}

int main()
{
    pthread_t thread;
    void *thread_return;
    int res;
    int arg = 520;
    
    printf("create thread\n");
    //创建线程
    res = pthread_create(&thread, NULL, test_thread, (void *)(unsigned long long)(arg));
    if (res != 0)
    {
        printf("create thread fail\n");
        exit(res);
    }

    printf("create treads success\n");
    printf("waiting for threads to finish...\n");

    res = pthread_join(thread, &thread_return);
    if (res != 0)
    {
        printf("thread exit fail!\n");
        exit(res);
    }

    printf("pthread exit ok!\n");

    return 0;
}
```







### 5.线程取消

子线程在运行的过程中， 在主进程或其它线程中可以调用`pthread_cance`l取消它(缺省模式)

```c
int pthread_cancel(pthread_t thread)
```

发送终止信号给thread线程，如果成功则返回0，否则为非0值。发送成功并不意味着thread会终止。

子线程被取消后， 在主进程中调用`pthread_jion`， 得到线程的返回状态是`PTHREAD CANCELED`， 即-1。

子线程可以调用`pthread_set cancel state`(PTHREAD_CANCEL_ENABLE 和PTHREAD_CANCEL_DISABLE) 设置对`pthread_cancel`请求的响应方式。

子线程可以调用`pthread_set canceltype`设置线程的取消方式

(PTHREAD CANCEL'ASYNCH ORONO US和PTHREAD CANCEL DEFERRED)子线程中调用`pthread_testcancel`设置取消点。

```c
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void *mainfunc(void *arg)
{
    int num = (int)(long)arg;
    printf("This is test thread,arg is %d\n", num);
    sleep(5);
    printf("This is test thread,arg is %d\n", num);
    pthread_exit(NULL);
}

int main()
{
    pthread_t thread;
    int res;
    int arg = 520;      
    res = pthread_create(&thread, NULL, mainfunc, (void *)(long)arg);
    if (res != 0)
    {
        printf("线程创建失败！\n");
    }
    printf("thread create success!\n");
    int cancelres=pthread_cancel(thread);
    if(cancelres==0)
        printf("thread cancel success!\n");
    pthread_join(thread,NULL);
    return 0;
}
```











### 6.线程清理

子线程退出时可能需要执行善后的工作，如释放资源和锁、回滚事务等。

善后的代码不合适写在线程主函数中，一般放在清理函数中。

```c
void pthread cleanup push(void (*routine)(void *),void *arg):
void pthread cleanup pop(int execute);
```

清理函数必须成对的书写在同一语句块中。

当线程被取消时，所有注册的清理函数以被推送到堆栈的顺序相反的顺序执行。

**当线程通过调用`pthread_exit`终止时，所有清理处理程序都将按照前一点所述执行。（如果线程通过`return`终止，则不调用清理函数。）**

当线程使用非零的`execute`参数调用`pthread cleanup_pop`时，将弹出并执行最上面的清理函数。





```c
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void cleanfunc(void *arg)
{
    int num = (int)(long)arg;
    printf("clean pop sleep This is test thread,arg is %d\n", num);
}

void *mainfunc(void *arg)
{
    //注册线程清理函数
    pthread_cleanup_push(cleanfunc,arg);
    int num = (int)(long)arg;
    printf("This is test thread,arg is %d\n", num);
    sleep(5);
    printf("sleep This is test thread,arg is %d\n", num);
    //弹出线程清理函数
    pthread_cleanup_pop(1);
    pthread_exit(NULL);
}

int main()
{
    pthread_t thread;
    int res;
    int arg = 520;   
    res = pthread_create(&thread,NULL, mainfunc, (void *)(long)arg);
    if (res != 0)
    {
        printf("线程创建失败！\n");
    }
    printf("thread create success!\n");
    //清理线程
    int cancelres=pthread_cancel(thread);
    if(cancelres==0)
        printf("thread cancel success!\n");
    //善后线程
    pthread_join(thread,NULL);
    return 0;
}
```







### 7.向线程发送信号

从外部向进程的发送信号时,信号到达进程,**不会中断子线程**。

在多线程程序中,捕获信号的代码放在哪都一样,一般放在进程的主函数中。

**在多线程程序中,在某个一个线程中调用signal或者sigaction等函数会改变所有线程中的信号处理函数。**

不存在对多个线程设置不同信号处理函数的说法

主进程向子线程发送信号用pthread_kill函数。

```c++
int pthread_kill(pthread_t thread, int sig);
```

在多线程代码中,可以使用sigwait, sigwaitinfo, sigtimedwait.

```c
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <signal.h>

void signalhandler(int sig)
{
    for (int i = 0; i < 30; i++)
    {
        printf("signal test is %d\n",i);
        sleep(1);
    }
    
}

void *mainfunc(void *arg)
{
    int num = (int)(long)arg;
    printf("This is test thread,arg is %d\n", num);
    for (int i = 0; i < 30; i++)
    {
        printf("pthread is %d\n",i);
        sleep(1);
    }
    
    pthread_exit(NULL);
}

int main()
{
    pthread_t thread;
    signal(2,signalhandler);
    int res;
    int arg = 520;      
    res = pthread_create(&thread, NULL, mainfunc, (void *)(long)arg);
    if (res != 0)
    {
        printf("线程创建失败！\n");
    }
    printf("thread create success!\n");
    sleep(2);
    pthread_kill(thread, 2);
    
    pthread_join(thread,NULL);
    printf("thread run success!\n");
    return 0;
}

```













### 8.线程同步与互斥

线程的锁的种类有互斥锁、读写锁、条件变量、自旋锁、信号灯。



#### 互斥锁



**声明互斥锁描述符**

```c++
pthread_mutex_t mutex;
```



**初始化锁**

```c++
int pthread_mutex_init(pthread_mutex_t *mutex,const pthread_mutex_attr_t *mutexattr);
```

```c++
eg:
pthread_mutex_init(&mutex,0);
```



**阻塞加锁**

```c++
int pthread_mutex_lock(pthread_mutex *mutex);
```

如果是锁是空闲状态，本线程将获得这个锁；如果锁已经被占据，本线程将排队等待，直到成功的获取锁。



**非阻塞加锁**

```c++
int pthread_mutex_trylock( pthread_mutex_t *mutex);
```

该函数语义与 pthread_mutex_lock() 类似，不同的是在锁已经被占据时立即返回 EBUSY，不是挂起等待



**解锁**

```c++
int pthread_mutex_unlock(pthread_mutex *mutex);
```

线程把自己持有的锁释放。



**销毁锁（此时锁必需unlock状态，否则返回EBUSY）**

```c++
int pthread_mutex_destroy(pthread_mutex *mutex);
```

销毁锁之前，锁必需是空闲状态（unlock）。



#### 程序

```c++
#include "CTcp/CTcpClient.h"//socket封装
#include <iostream>
#include <pthread.h>
#include <string>
using namespace std;

CTcpClient client;//申明一个客户端类
pthread_mutex_t mutex;//申明一个互斥锁

//多线程使用同一个对象，可能会产生冲突，防止此冲突，添加互斥锁
void *pth_main(void *arg)
{
    int pthread1 = (long)arg;
    char buffer[1024];
    for (int i = 0; i < 3; i++)
    {
        pthread_mutex_lock(&mutex);		//加锁

        memset(buffer, 0, sizeof(buffer));
        string str = string("这是第") + to_string(i) + string("超级女生");
        strcpy(buffer, str.c_str());
        if (client.Send(buffer, sizeof(buffer)) <= 0)
            break;
        cout << "线程" << pthread1 << "发送:" << str << endl;

        memset(buffer, 0, sizeof(buffer));
        if (client.Recv(buffer, sizeof(buffer)) <= 0)
            break;
        cout << "线程" << pthread1 << "接受:" << buffer << endl;

        pthread_mutex_unlock(&mutex);	//释放锁
        usleep(100);
 
    }
}

int main(int argc, char *argv[])
{
    if (argc < 3)
    {
        cout << "Example:./client 127.0.0.1 5005" << endl;
        return -1;
    }
    

    pthread_mutex_init(&mutex, 0);	//创建锁
	
    //客户端向服务端发起连接
    if (client.ConnectToServer(argv[1], atoi(argv[2])) == false)
    {
        cout << "Connect Server failed!" << endl;
        return -1;
    }
	
    //声明并创建两个线程，两个线程调用同一个函数
    pthread_t thread1, thread2;
    pthread_create(&thread1, NULL, pth_main, (void *)1);
    pthread_create(&thread2, NULL, pth_main, (void *)2);
	
    //等待线程退出
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);
	
    //销毁锁
    pthread_mutex_destroy(&mutex);
}

```



### 9. 查看线程

在shell中执行：

查看当前运行的进程：psaux|grep book 

查看当前运行的轻量级进程：ps-aL|grepbook 

查看主线程和新线程的关系：pstree-p主线程id

1）在top命令中，如果加上-H参数，top中的每一行显示的不是进程，而是一个线程。

```
top -H
```

2）在ps命令中加-xH参数也可以显示线程，加grep可以过滤内容。

```
ps -xH
ps -xH|grep book261
```





## 3.6 GDB工具



### 1. gdb的安装

```shell
apt install gdb
```



### 2.调试前的准备

​	用gcc编译源程序的时候，编译后的可执行文件不会包含源程序代码，如果您打算编译后的程序可以被调试，编译的时候要加-g的参数，例如：

```shell
gcc -g -o book113 book113.c -lpthread
sudo g++ pthread_signal.c -lpthread -g -o thread_signal1
```

```shell
gdb book113
```



### 3.基本调试命令

| **命令**           | **命令缩写** | **命令说明**                                                 |
| ------------------ | :----------- | ------------------------------------------------------------ |
| set args           |              | 设置主程序的参数。例如：./book119 /oracle/c/book1.c /tmp/book1.c设置参数的方法是：gdb book119(gdb) set args /oracle/c/book1.c /tmp/book1.c |
| break              | b            | 设置断点，b 20 表示在第20行设置断点，可以设置多个断点。      |
| run                | r            | 开始运行程序, 程序运行到断点的位置会停下来，如果没有遇到断点，程序一直运行下去。 |
| next               | n            | 执行当前行语句，如果该语句为函数调用，不会进入函数内部执行。 |
| step               | s            | 执行当前行语句，如果该语句为函数调用，则进入函数执行其中的第一条语句。注意了，如果函数是库函数或第三方提供的函数，用s也是进不去的，因为没有源代码，如果是您自定义的函数，只要有源码就可以进去。 |
| print              | p            | 显示变量值，例如：p name表示显示变量name的值。               |
| continue           | c            | 继续程序的运行，直到遇到下一个断点。                         |
| set var name=value |              | 设置变量的值，假设程序有两个变量：int ii; char name[21];set var ii=10 把ii的值设置为10；set var name="西施" 把name的值设置为"西施"，注意，不是strcpy。 |
| quit               | q            | 退出gdb环境。                                                |

info b 查看所有断点



### 4.调试CORE文件

程序挂掉时， 系统缺省不会生成core文件。

1) ulimit-a查看系统参数；

2. ulimit-c unlimited 把core文件的大小设为无限制

   ```shell
   ulimit-c unlimited
   ```

3. 运行程序， 生成core文件；

4. gdb 程序名 core文件名。

   ```shell
   gdb thread_signal core.19356
   ```

   ![image-20220707115222072](https://s2.loli.net/2022/07/07/69Ko875VpFPlMqv.png)

5. 使用gdb ./文件 生成的core文件直接查看段错误的位置

6. 在gdb内使用bt命令可以查看函数的调用栈

![image-20220707115243495](https://s2.loli.net/2022/07/07/SAw3pzJaBZWhC89.png)







### 5.调试正在运行的文件

可以使正在运行的文件暂停

```
gdp 程序名 -p pid
```

在gdb内使用bt命令可以查看函数的调用栈





### 6.调试多进程服务程序

调试父进程：set follow-fork-mode parent (缺省)

调试子进程：set follow-fork-mode child 设置调试模式：

set detach-on-fork[on l off] ， 缺省是on，表示调试当前进程的时候，其它的进程继续运行， 如果用off，调试当前进程的时候，身其它的进程被gdb挂起。

查看调试的进程：info inferiors 

切换当前调试的进程：inferior进程id









### 7.调试多线程服务程序

在gdb中执行：

默认情况下，除了调试的当前线程，其余线程人会正常运行

查看线程：info threads 

![image-20220707141730948](https://s2.loli.net/2022/07/07/KQrljNvVD4wz8nC.png)

切换线程：thread 线程id 

只运行当前线程：set scheduler-locking on 

运行全部的线程：set scheduler-locking off 

指定某线程执行某gdb命令：thread apply 线程id cmd 

全部的线程执行某gdb命令：thread apply all cmd











# 4. 网络编程





## 4.1 网路相关知识简介

TCP/IP是一个协议族，包含众多的协议。

​	**HTTP协议**是Hyper Text Transfer Protocol（超文本传输协议）的缩写，HTTP的应用最为广泛。HTTP协议工作于架构之上，，浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。Web服务器根据接收到的请求后，向客户端发送响应信息。

​	**FTP（File Transfer Protocol）是文件传输协议的简称。**FTP是工作在应用层的网络协议。FTP使得主机间可以共享文件，用于在两台设备之间传输文件（双向传输）。它也是一个客户端-服务端框架系统。用户可以通过一个支持FTP协议的客户端程序，连接到在远程主机上的FTP服务端程序，通过客户端程序向服务端程序发出命令，服务端程序执行用户所发出的命令，并将执行的结果返回到客户机。FTP除了基本的文件上传/下载功能外，还有目录操作、权限设置、身份验证机制，许多网盘的文件传输功能都是基于FTP实现的。

​	HTTP是单向的，设备只能主动向服务器发出数据，无法被动的接收来自网络的数据，这不适用于实时控制的场合；HTTP是有许多帧头和规则的重量级协议，实现在设备中需要耗费大量的系统资源。基于上述的形势，**MQTT和COAP等轻量级、异步的通信协议**便得到了物联网设备开发商的宠爱

​	

### 网络协议的分层模型

![tcpip001](https://s2.loli.net/2022/06/28/CbLPutoal5TwZFr.png)

**物理层(PHY)**规定了传输信号所需要的物理电平、介质特征；

**链路层（MAC）**规定了数据帧能被网卡接收的条件，最常见的方式是利用网卡的MAC地址，发送方会在欲发送的数据帧的首部加上接收方网卡的MAC地址信息，接收方只有监听到属于自己的MAC地址信息后，才会去接收并处理该数据。

每台网络设备都应该有自己的网络地址，网络层规定了主机的网络地址该如何定义，以及如何在网络地址和MAC地址之间进行映射，**即ARP协议**；网络层实现了数据包在主机之间的传递，而一台主机内部可能运行着多个网络程序。

**传输层**可以区分数据包是属于哪一个应用程序的，可以说传输层实现了数据包端到端的传递。另外，数据包在传输过程中可能会出现丢包、乱序和重复的现象，网络层并没有提供应对这些错误的机制，而传输层可以解决这些问题，如TCP协议；

应用层以下的工作完成了数据的传递工作，**应用层**则决定了你如何应用和处理这些数据





### 协议层报文间的封装与拆分

![tcpip002](https://s2.loli.net/2022/06/28/n8ZqhiENt1vPWl5.png)

当用户发送数据时，将数据向下交给传输层，这是处于应用层的操作，而**应用层**也有相关的协议，**对用户的数据进行封装**，比如MQTT、HTTP等协议，最后应用层通过调用传输层的接口来将数据递交到传输层中。**传输层会在数据前面加上传输层首部**（此处以TCP协议为例，图中的传输层首部为TCP首部，也可以是UDP首部），然后向下交给网络层。同样地，**网络层会在数据前面加上网络层首部**（IP首部），然后将数据向下交给链路层，**链路层会对数据进行最后一次封装，即在数据前面加上链路层首部**（此处使用以太网接口为例），然后将数据交给网卡。最后，**网卡将数据转换成物理链路上的电平信号**，数据就这样被发送到了网络中。数据的发送过程，可以概括为TCP/IP的各层协议对数据进行封装的过程





### 1. IP协议

​	**IP协议（Internet Protocol）**，又称之为网际协议，IP协议处于IP层工作，它是整个TCP/IP协议栈的核心协议，上层协议都要依赖IP协议提供的服务，IP协议负责将数据报从源主机发送到目标主机，通过IP地址作为唯一识别码

#### IP地址简述

​	**每个接入网络中的主机都分配一个IP地址（Internet Protocol Address），是一个32位的整数地址**，只有合法的IP地址才能接入互联网中并且与其他主机进行通信，IP地址是软件地址，不是硬件地址，硬件MAC地址是存储在网卡中的，应用于本地网络中寻找目标主机。例如， IP地址为192.168.0.122



#### IP地址编址

![tcpip003](https://doc.embedfire.com/linux/imx6/base/zh/latest/_images/tcpip003.png)

| 类别 | 第一字节(二进制) | 第一字节取值范围 | 网络号个数 | 主机号个数 | 适用范围 |
| ---- | ---------------- | ---------------- | ---------- | ---------- | -------- |
| A类  | 0XXX XXXX        | 0~127            | 125        | 16777214   | 大型网络 |
| B类  | 10XX XXXX        | 128~191          | 16368      | 65534      | 中型网络 |
| C类  | 110X XXXX        | 192~223          | 2097152    | 254        | 小型网络 |
| D类  | 1110 XXXX        | 224~239          | —          | —          | 多播     |
| E类  | 1111 XXXX        | 240~255          | —          | —          | 保留     |

##### 特殊IP地址

**1.受限广播地址**

受限广播地址是网络号与主机号都为1的地址

255.255.255.255这个地址指本网段内的所有主机，这样的数据包仅会出现在本地网络中（局域网）

**2.直接广播地址**

直接广播地址是主机号全为1而得到的地址

比如一个IP地址是192.168.0.181，这是C类地址，所以它的主机号只有一个字节，那么对主机号全取1得到一个广播地址192.168.0.255，向这个地址发送数据就能让同一网络下的所有主机接收到。

A、B、C三类地址的广播地址结构如下：

- A类地址的广播地址为：XXX.255.255.255（XXX为A类地址的第一个字节取值范围）。
- B类地址的广播地址为：XXX. XXX.255.255（XXX为B类地址的前两个字节取值范围）。
- C类地址的广播地址为：XXX. XXX. XXX.255（XXX为C类地址的前三个字节取值范围）。

**3.多播地址**

多播地址用在一对多的通信中，即一个发送者，多个接收者，不论接受者员数量的多少，发送者只发送一次数据包。多播地址属于分类编址中的D类地址， D类地址只能用作目的地址，而不能作为主机中的源地址。

**4.环回地址**

127网段的所有地址都称为环回地址，主要用来测试网络协议是否工作正常的作用。比如在电脑中使用ping 命令去ping 127.1.1.1就可以测试本地TCP/IP协议是否正常。







### 2. UDP协议

**UDP 是User Datagram Protocol**的简称， 中文名是用户数据报协议，是一种无连接、不可靠的协议，它只是简单地实现从一端主机到另一端主机的数据传输功能，这些数据通过IP层发送，在网络中传输

UDP协议的特点：

1. 无连接、不可靠。
2. 尽可能提供交付数据服务，出现差错直接丢弃，无反馈。
3. 面向报文，发送方的UDP拿到上层数据直接添加个UDP首部，然后进行校验后就递交给IP层，而接收的一方在接收到UDP报文后简单进行校验，然后直接去除数据递交给上层应用。
4. 支持一对一，一对多，多对一，多对多的交互通信。
5. 速度快，UDP没有TCP的握手、确认、窗口、重传、拥塞控制等机制，UDP是一个无状态的传输协议，所以它在传递数据时非常快，即使在网络拥塞的时候UDP也不会降低发送的数据。





### 3. TCP协议

TCP与UDP一样，都是传输层的协议，但是提供的服务却大不相同，UDP为上层应用提供的是一种不可靠的，无连接的服务，而**TCP则提供一种面向连接、可靠的字节流传输服务，TCP让两个主机建立连接的关系，应用数据以数据流的形式进行传输**

- **UDP运载的数据是以报文的形式**，各个报文在网络中互不相干传输，UDP每收到一个报文就递交给上层应用，因此如果对于大量数据来说，应用层的重装是非常麻烦的，因为UDP报文在网络中到达目标主机的顺序是不一样的；
- 而**TCP采用数据流的形式传输**，先后发出的数据在网络中虽然也是互不相干的传输，但是这些数据本身携带的信息却是紧密联系的，**TCP协议会给每个传输的字节进行编号**，当然啦，两个主机方向上的数据编号是彼此独立的，在传输的过程中，发送方把数据的起始编号与长度放在TCP报文中，**在接收方将所有数据按照编号组装起来，然后返回一个确认，当所有数据接收完成后才将数据递交到应用层中。**



#### TCP特性

**1.连接机制**

TCP是一个面向连接的协议，一个TCP连接必须有双方IP地址与端口号

**2.确认与重传**

一个完整的TCP传输必须有数据的交互，**接收方在接收到数据后必须进行正面的确认**，向发送方报告接收结果。**发送方发送数据后会启动一个定时器，若指定时间内没收到确认结果，会认为发送失败，进行重传报文。**

TCP提供可靠的运输层，但是依赖度IP层是不可靠的，也是通过这种方法来确认是否已经将数据传给接收方，若超时则重传。

**3.缓冲机制**

TCP会将数据存储在一个缓冲空间中，**等到数据量足够大的时候在进行发送数据**，这样子能提供传输的效率并且减少网络中的通信量。**数据发送出去的时候并不会立即删除数据**，还是让数据保存在缓冲区中，因为发送出去的数据不一定能被接收方正确接收，它需要等待到接收方的确认再将数据删除。同样的，在**接收方也需要有同样的缓冲机制**，**因为在网络中传输的数据报到达的时间是不一样的，而且TCP协议还需要把这些数据报组装成完整的数据，然后再递交到应用层中。**

**4.全双工通信**

**TCP协议是一个全双工的协议。TCP协议的确认是通过捎带的方式来实现**，即接收方把确认信息放到反向传来的是数据报文中，不必单独为确认信息申请一个报文，捎带机制减少了网络中的通信流量

**5.流量控制**

**TCP提供了流量控制服务（flow-control service）以消除发送方使接收方缓冲区溢出的可能性**。流量控制是一个速度匹配服务，即发送方的发送速率与接收方应用程序的读取速率相匹配，TCP通过让发送方维护一个称为**接收窗口（receive window）**的变量来提供流量控制。它允许接收方根据自身的处理能力来确定能接收数据的多少，因此会告诉发送方可以发送多少数据过来，即窗口的大小

**6.差错控制**

TCP协议也会采用校验和的方式来检验数据的有效性，主机在接收数据的时候，会将重复的报文丢弃，将乱序的报文重组，发现某段报文丢失了会请求发送方进行重发，因此在TCP往上层协议递交的数据是顺序的、无差错的完整数据。

**7.拥塞控制**

什么是拥塞？当数据从一个大的管道（如一个快速局域网）向一个较小的管道（如一个较慢的广域网）发送时便会发生拥塞

发送方必须实现一直自适应的机制，在网络中拥塞的情况下调整自身的发送速度，这种形式对发送方的控制被称为**拥塞控制**。



### 4. 端口号的概念

TCP连接是两个不同主机的应用连接，而传输层与上层协议是通过端口号进行识别的，端口号的取值范围是0~65535，一个主机内可能只有一个IP地址，但是可能有多个端口号，每个端口号表示不同的应用线程。

通过“**IP地址+端口号**”来区分主机不同的线程。

| 端口号 | 协议   | 说明                                                         |
| ------ | ------ | ------------------------------------------------------------ |
| 20/21  | FTP    | 文件传输协议，使得主机间可以共享文件。                       |
| 23     | Telnet | 终端远程登录，它为用户提供了在本地计算机上完成远程主机工作的能力。 |
| 25     | SMTP   | 简单邮件传输协议，它帮助每台计算机在发送或中转信件时找到下一个目的地。 |
| 69     | TFTP   | 普通文件传输协议。                                           |
| 80     | HTTP   | 超文本传输协议，通过使用网页浏览器、网络爬虫或者其它的工具，客户端发起一个HTTP请求到服务器上指定端口（默认端口为80），应答的服务器上存储着一些资源，比如HTML文件和图像，那么就会返回这些数据到客户端。 |
| 110    | POP3   | 邮局协议版本3，本协议主要用于支持使用客户端远程管理在服务器上的电子邮件。 |







### 5. TCP报文段

![tcpip004](https://s2.loli.net/2022/06/28/aQDvUdemFg7HsKw.png)

一般来说TCP首部只有20个字节

**源端口号**：16位发送端端口号，用于区分不同服务的端口，端口号的范围从0到65535。

**目的端口号**：16位接收端端口号。

**序号字段**：用来标识从TCP发送端向TCP接收端发送的数据字节流，**它的值表示在这个报文段中的第一个数据字节所处位置**，根据接收到的数据区域长度，就能计算出报文最后一个数据所处的序号，因为TCP协议会对发送或者接收的数据进行编号（按字节的形式），那么使用序号对每个字节进行计数，就能很轻易管理这些数据。序号是32 bit的无符号整数。

**确认序号**：包含接收端所期望收到的下一个序号，因此，确认序号应当是上次已成功收到数据的最后一个字节序号加 1。当然，只有ACK标志为 1时确认序号字段才有效。TCP为应用层提供全双工服务，这意味数据能在两个方向上独立地进行传输，因此确认序号通常会与反向数据（即接收端传输给发送端的数据）封装在同一个报文中（即捎带），所以连接的每一端都必须保持每个方向上的传输数据序号准确性

**首部长度**：它指出了TCP报文段首部长度，以字节为单位，最大能记录15*4=60字节的首部长度

**标志字段**：

-  \- URG：首部中的紧急指针字段标志，如果是1表示紧急指针字段有效。
-  \- ACK：首部中的确认序号字段标志，如果是1表示确认序号字段有效。
- \- PSH：该字段置一表示接收方应该尽快将这个报文段交给应用层。
- \- RST：重新建立TCP连接。 
- -SYN：用同步序号发起连接。 
-  -FIN：中止连接。

**窗口大小**：为字节数，起始于确认序号字段指明的值，这个值是接收端正期望接收的数据序号。窗口最大为 65535字节

**检验和**：覆盖了整个的 TCP报文段：TCP首部和TCP数据区域，由发送端计算和填写，并由接收端进行验证。

**紧急指针**：是一个正的偏移量，和序号字段中的值相加表示紧急数据最后一个字节的序号。本TCP报文段的紧急数据在报文段数据区域中，从序号字段开始，偏移紧急指针的值结束。



### 6. TCP建立连接



#### 三次握手：建立连接

![tcpip005](https://s2.loli.net/2022/06/28/8TKYbrthgfSw2O9.png)

**第一步**：客户端的TCP首先向服务器端的TCP发送一个特殊的TCP报文段（**握手请求报文**，**SYN报文段**）。该报文段不包含应用层数据。**SYN标志位置1，序号段设置为A，ACK标志位置0，确认序号段无效。**

**第二步**：服务器端收到握手请求报文，从SYN报文段提取信息，为TCP链接分配TCP缓存和变量，并发送**握手应答报文**。**SYN标志位置1，ACK标志位置1，序号段设置为B，确认序号段设置为A+1.**

**第三步**：客户端收到握手应答报文后，客户端分配TCP链接缓存和变量，返回应答报文。**SYN标志位置0，ACK标志位置1，序号段设置A+1,确认序号段设置为B+1**





#### 四次挥手：终止连接

TCP的特性造成的，因为 TCP连接是全双工连接的服务，因此每个方向上的连接必须单独关闭。

当一端完成它的数据发送任务后就能发送一个 **FIN报文段**（可以称之为终止连接请求，其实就是FIN标志位被设置为1）来终止这个方向上的连接。

![tcpip006](https://s2.loli.net/2022/06/28/u9fvUNF1bahZHJk.png)

**第一步**：客户端发送一个FIN报文段来关闭连接。**FIN标志位为1，假设序号段为C，ACK标志位为1，但是确认序号段无效**

**第二步**：服务器端收到FIN报文段，发回一个ACK报文段（**终止连接应答**），**FIN置0，ACK标志为1，确认序号C+1**，此时断开客户端->服务器的方向连接。

**第三步**：服务器就会发送一个FIN报文段。**FIN标志位为1，假设序号为D，ACK标志位为1，但是确认序号段无效**

**第四步**：客户端返回一个ACK报文段。**FIN置0，ACK标志置1，确认序号为D+1**，此时断开服务器->客户端的方向连接。









### 7. TCP状态

TCP协议的状态如下：

- **LISTENING状态**：提供某种服务，侦听远方TCP端口的连接请求，当提供的服务没有被连接时，处于LISTENING状态，端口是开放的，等待被连接。
- **SYN_SENT (客户端状态)**：客户端调用connect()函数，将会发送一个SYN请求建立一个连接，在发送连接请求后等待匹配的连接请求，此时状态为SYN_SENT.
- **SYN_RECEIVED (服务端状态)**：在收到和发送一个连接请求后，等待对方对连接请求的确认，当服务器收到客户端发送的同步信号时，将标志位ACK和SYN置1发送给客户端，此时服务器端处于SYN_RCVD状态，如果连接成功了就变为ESTABLISHED，正常情况下SYN_RCVD状态非常短暂。
- **ESTABLISHED状态**：这个状态是处于稳定连接状态，建立连接的TCP协议两端的主机都是处于这个状态，它们相互知道彼此的窗口大小、序列号、最大报文段等信息。
- **FIN_WAIT_1与FIN_WAIT_2状态**：处于这个状态一般都是单向请求终止连接，然后主机等待对方的回应，而如果对方产生应答，则主机状态转移为FIN_WAIT_2，此时{主机->对方}方向上的TCP连接就断开，但是{对方->主机}方向上的连接还是存在的。此处有一个注意的地方：如果主机处于FIN_WAIT_2状态，说明主机已经发出了FIN报文段，并且对方也已对它进行确认，除非主机是在实行半关闭状态，否则将等待对方主机的应用层处理关闭连接，因为对方已经意识到它已收到FIN报文段，它需要主机发一个 FIN 来关闭{对方->主机}方向上的连接。只有当另一端的进程完成这个关闭，主机这端才会从FIN_WAIT_2状态进入TIME_WAIT状态。否则这意味着主机这端可能永远保持这个FIN_WAIT_2状态，另一端的主机也将处于 CLOSE_WAIT状态，并一直保持这个状态直到应用层决定进行关闭。
- **CLOSE-WAIT状态**：等待从本地用户发来的连接中断请求 ，被动关闭端TCP接到FIN后，就发出ACK以回应FIN请求(它的接收也作为文件结束符传递给上层应用程序),并进入CLOSE_WAIT.
- **TIME_WAIT状态**：TIME_WAIT状态也称为 2MSL等待状态。每个具体TCP连接的实现必须选择一个TCP报文段最大生存时间MSL（Maximum Segment Lifetime），如IP数据报中的TTL字段，表示报文在网络中生存的时间，它是任何报文段被丢弃前在网络内的最长时间，这个时间是有限的，为什么需要等待呢？我们知道IP数据报是不可靠的，而TCP报文段是封装在IP数据报中，TCP协议必须保证发出的ACK报文段是正确被对方接收， 因此处于该状态的主机必须在这个状态停留最长时间为2倍的MSL，以防最后这个ACK丢失，因为TCP协议必须保证数据能准确送达目的地。

![tcpip007](https://s2.loli.net/2022/06/29/VtcdZjO5QY2vnxB.png)

- 虚线：表示服务器的状态转移。
- 实线：表示客户端的状态转移。
- 图中所有“关闭”、“打开”都是应用程序主动处理。
- 图中所有的“超时”都是内核超时处理。



**三次握手过程：**

- 图中(7)：服务器的应用程序主动使服务器进入监听状态，等待客户端的连接请求。
- 图中(1)：首先客户端的应用程序会主动发起连接，发送SYN报文段给服务器，在发送之后就进入SYN_SENT状态等待服务器的SYN ACK报文段进行确认，如果在指定超时时间内服务器不进行应答确认，那么客户端将关闭连接。
- 图中(8)：处于监听状态的服务器收到客户端的连接请求（SYN报文段），那么服务器就返回一个SNY ACK报文段应答客户端的响应，并且服务器进入SYN_RCVD状态。
- 图中(1)：如果客户端收到了服务器的SYN ACK报文段，那么就进入ESTABLISHED稳定连接状态，并向服务器发送一个ACK报文段。
- 图中(9)：同时，服务器收到来自客户端的ACK报文段，表示连接成功，进入ESTABLISHED稳定连接状态，这正是我们建立连接的三次握手过程。



**四次挥手过程：**

- 图中(3)：一般来说，都是客户端主动发送一个FIN报文段来终止连接，此时客户端从ESTABLISHED稳定连接状态转移为FIN_WAIT_1状态，并且等待来自服务器的应答确认。
- 图中(10)：服务器收到FIN报文段，知道客户端请求终止连接，那么将返回一个ACK报文段到客户端确认终止连接，并且服务器状态由稳定状态转移为CLOSE_WAIT等待终止连接状态。
- 图中(4)：客户端收到确认报文段后，进入FIN_WAIT_2状态，等待来自服务器的主动请求终止连接，此时 `{客户端->服务器}` 方向上的连接已经断开。
- 图中(11)：一般来说，当客户端终止了连接之后，服务器也会终止 `{客户端->服务器}` 方向上的连接，因此服务器的原因程序会主动关闭该方向上的连接，发送一个FIN报文段给客户端。
- 图中(5)：处于FIN_WAIT_2的客户端收到FIN报文段后，发送一个ACK报文段给服务器。
- 图中(12)：服务器收到ACK报文段，就直接关闭，此时 `{客户端->服务器}` 方向上的连接已经终止，进入CLOSED状态。
- 图中(6)：客户端还会等待2MSL，以防ACK报文段没被服务器收到，这就是四次挥手的全部过程。 注意：对于图中(13)(14)(15)的这些状态都是一些比较特殊的状态，我们暂时就不讲解了，总的来说都是一样的。













## 4.2 Socket通信





### 4.2.1 Socket 简介

Socket使用一个套接字描述符来记录网络 的连接，类似于文件描述符。

应用程序以该描述符作为传递参数，通过调用Socket API接口的函数来完成某种操作

```c++
#include <sys/types.h>
#include <sys/socket.h>
```

![image.png](https://s2.loli.net/2022/06/29/HLUa19XjKsSMCVQ.png)



### 4.2.2 socket() 创建一个socket描述符

```c++
int socket(int domain, int type, int protocol);
```

socket()函数用于创建一个socket描述符（socket descriptor）

返回值：成功则返回一个socket，失败返回-1

1. domain：**参数domain表示该套接字使用的协议族**

   对于TCP/IP协议，选择**AFT_INET**足以

   - AF_UNIX, AF_LOCAL： 本地通信
   - **AF_INET ： IPv4**
   - AF_INET6 ： IPv6
   - AF_IPX ： IPX - Novell 协议
   - AF_NETLINK ： 内核用户界面设备
   - AF_X25 ： ITU-T X.25 / ISO-8208 协议
   - AF_AX25 ： 业余无线电 AX.25 协议
   - AF_ATMPVC ： 访问原始ATM PVC
   - AF_APPLETALK ： AppleTalk
   - AF_PACKET ： 底层数据包接口
   - AF_ALG ： 内核加密API的AF_ALG接口

2. type：**参数type指定了套接字使用的服务类型**

   - **SOCK_STREAM**：提供可靠的（即能保证数据正确传送到对方）面向连接的Socket服务，多用于资料（如文件）传输，**如TCP协议。**
   - SOCK_DGRAM：是提供无保障的面向消息的Socket 服务，主要用于在网络上发广播信息，如UDP协议，提供无连接不可靠的数据报交付服务。
   - SOCK_SEQPACKET：为固定最大长度的数据报提供有序的，可靠的，基于双向连接的数据传输路径。
   - SOCK_RAW：表示原始套接字，它允许应用程序访问网络层的原始数据包，这个套接字用得比较少，暂时不用理会它。
   - SOCK_RDM：提供不保证排序的可靠数据报层。

3. protocol：参数protocol指定了套接字使用的协议

   在IPv4中，只有TCP协议提供SOCK_STREAM这种可靠的服务，只有UDP协议提供SOCK_DGRAM服务，对于这两种协议，protocol的值均为0，因为当protocol为0时，会自动选择type类型对应的默认协议。

```c++
//IPV4的TCP协议
int sockfd;
sockfd = socket(AF_INET,SOCK_STREAM,0));
```



### 4.2.3 bind() 将IP地址，端口号与套接字进行绑定

```c++
int bind(int sockfd, struct sockaddr *my_addr, socklen_t addrlen);
```

bind()函数用于将一个 IP 地址或端口号与一个套接字进行绑定。

作为服务器端，这一步绑定的操作是必要的，而作为客户端，则不是必要的，因为内核会帮我们自动选择合适的IP地址与端口号。

- sockfd：sockfd是由socket()函数返回的套接字描述符。
- my_addr：my_addr是一个指向套接字地址结构的指针。
- addrlen：addrlen指定了以addr所指向的地址结构体的字节长度。

```c++
struct sockaddr {
    sa_family_t     sa_family;
    char            sa_data[14];
}
struct sockaddr_in {
    short int sin_family;               /* 协议族 */
    unsigned short int sin_port;        /* 端口号 */
    struct in_addr sin_addr;            /* IP地址 */
    unsigned char sin_zero[8];          /* sin_zero是为了让sockaddr与sockaddr_in两个数据结构保持大小相同而保留的空字节 */
};
```

sockaddr_in和sockaddr是并列的结构（占用的空间是一样的），指向sockaddr_in的结构体的指针也可以指向sockadd的结构体，并代替它，而且sockaddr_in结构对用户将更加友好，在使用的时候进行类型转换就可以了。

```c
struct sockaddr_in server;
memset(&server,0,sizeof(server));
server.sin_family=AF_INET;// 协议族，在socket编程中只能是AF_INET。
server.sin_addr.s_addr=htonl(INADDR_ANY);// 任意ip地址。
server.sin_port=htons(atoi(argv[1]));	//指定通信端口或者htons(6666)

//将一个IP地址，端口号与套接字进行绑定
bind(sockfd,(struct sockaddr*)&server,sizeof(server));
```



​	

### 4.2.4 connect() 客户端发起握手

```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

**这个connect()函数用于客户端中，将sockfd与远端IP地址、端口号进行绑定，在TCP客户端中调用这个函数将发生握手过程**（会发送一个TCP连接请求），并最终建立一个TCP连接，而对于UDP协议来说，调用这个函数只是在sockfd中记录远端IP地址与端口号，而不发送任何数据，参数信息与bind()函数是一样的。

connect()函数是套接字连接操作，对于TCP协议来说，**connect()函数操作成功之后代表对应的套接字已与远端主机建立了连接，可以发送与接收数据。**

函数调用成功则返回0，失败返回-1

对于UDP协议来说，没有连接的概念，在这里我就将其描述为记录远端主机的IP地址与端口好，UDP协议经过connect()函数调用成功之后，在通过sendto()函数发送数据报时不需要指定目的地址、端口，因为此时已经记录到了远端主机的IP地址与端口号。**UDP协议还可以给同一个套接字进行多次connect()操作，而TCP协议不可以，TCP只能指定一次connect操作。**







### 4.2.5 listen() 服务器端进入监听状态

```c
int listen(int s, int backlog);
```

**listen()函数只能在TCP服务器进程中使用，让服务器进程进入监听状态，等待客户端的连接请求，**listen()函数在一般在bind()函数之后调用，在accept()函数之前调用

- sockfd：sockfd是由socket()函数返回的套接字描述符。
- backlog参数用来描述sockfd的等待连接队列能够达到的最大值。





### 4.2.6 accept() 服务器端用于处理连接请求

```c
int accept(int s, struct sockaddr *addr, socklen_t *addrlen);
```

**accept()函数就是用于处理连接请求的**，它会从未完成连接队列中取出第一个连接请求，建一个和参数 s 属性相同的连接套接字，并为这个套接字分配一个文件描述符，然后以这个描述符返回。

**参数s为原由socket()创建的套接字**，s仍处于监听状态。

**参数addr用来返回已连接的客户端的IP地址与端口号**

**参数addrlen用于返回addr所指向的地址结构体的字节长度**，如果我们对客户端的IP地址与端口号不感兴趣，可以把arrd和addrlen均置为空指针。

​	





### 4.2.7 read() recv()

```c++
    ssize_t read(int fd, void *buf, size_t count);

    ssize_t recv(int sockfd, void *buf, size_t len, int flags);

    ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags,
                    struct sockaddr *src_addr, socklen_t *addrlen);

ps：ssize_t 它表示的是 signed size_t 类型。
```

客户端与服务器端建立TCP连接后，可以使用sockfd套接字收发数据。

接受网络中的数据函数有read()、recv()、recvfrom()

**read() 从描述符 fd中读取 count 字节的数据并放入从 buf 开始的缓冲区中，read()函数调用成功返回读取到的字节数**

参数： 

- fd：在socket编程中是指定套接字描述符。
- buf：指定存放数据的地址。 
- count：是指定读取的字节数，将读取到的数据保存在缓冲区buf中。

**recv()函数会先检查套接字 s 的接收缓冲区，如果 s 接收缓冲区中没有数据或者协议正在接收数据，那么recv就一直等待，直到协议把数据接收完毕。当协议把数据接收完毕，recv()函数就把 s 的接收缓冲中的数据拷贝到 buf 中**

参数 flags 一般设置为0即可









### 4.2.8 write()

```c++
ssize_t write(int fd, const void *buf, size_t count);
```

**write()函数一般用于处于稳定的TCP连接中传输数据，当然也能用于UDP协议中**

它向套接字描述符 fd 中写入 count 字节的数据，数据起始地址由 buf 指定，函数调用成功返回写的字节数，失败返回-1，并设置errno变量。

1. write()函数的返回值大于0，表示写了部分数据或者是全部的数据，这样我们可以使用一个while循环不断的写入数据，但是循环过程中的 buf 参数和 count 参数是我们自己来更新的，也就是说，网络编程中写函数是不负责将全部数据写完之后再返回的，说不定中途就返回了！
2. 返回值小于0，此时出错了

write函数封装

```c
/* Write "n" bytes to a descriptor. */
ssize_t writen(int fd, const void *vptr, size_t n)
{
    size_t      nleft;      //剩余要写的字节数
    ssize_t     nwritten;   //已经写的字节数
    const char  *ptr;       //write的缓冲区

    ptr = vptr;             //把传参进来的write要写的缓冲区备份一份
    nleft = n;              //还剩余需要写的字节数初始化为总共需要写的字节数

    //检查传参进来的需要写的字节数的有效性
    while (nleft > 0) {
        if ( (nwritten = write(fd, ptr, nleft)) <= 0) { //把ptr写入fd
            if (nwritten < 0 && errno == EINTR) //当write返回值小于0且因为是被信号打断
                nwritten = 0;       /* and call write() again */
            else
                return(-1);         /* error 其他小于0的情况为错误*/
        }

        nleft -= nwritten;          //还剩余需要写的字节数=现在还剩余需要写的字节数-这次已经写的字节数
        ptr += nwritten;          //下次开始写的缓冲区位置=缓冲区现在的位置右移已经写了的字节数大小
    }
    return(n); //返回已经写了的字节数
}

ps：当然啦，如果是比较简单的数据（比如单行数据）倒是不需要那么麻烦，直接调用write()也是完全没有问题的，只是看情况写代码就行了，上面代码的封装只是保证程序的健壮性。

注意，这个函数在写入数据完成后并不是立即发送的，至于什么时候发送则由TCP/IP协议栈决定。
```







### 4.2.9 send()

```c
int send(int s, const void *msg, size_t len, int flags);
```

无论是客户端还是服务器应用程序都可以用send()函数来向TCP连接的另一端发送数据。

- s：指定发送端套接字描述符。
- msg：指定要发送数据的缓冲区。
- len：指明实际要发送的数据的字节数。
- flags：一般设置为0即可



### 4.2.10 sendto()

```c++
int sendto(int s, const void *msg, size_t len, int flags, const struct sockaddr *to, socklen_t tolen);
```

sendto()函数与send函数非常像，但是它会通过 struct sockaddr 指向的 to 结构体指定要发送给哪个远端主机，在to参数中需要指定远端主机的IP地址、端口号等，而tolen参数则是指定to 结构体的字节长度。







### 4.2.11 服务器端和客户端绑定IP地址和端口的细节

```c++
unsigned long inet_addr(const char* cp);	//字符串和in_addr结构体的转换
char* inet_ntoa(struct in_addr in);

m_servaddr.sin_addr.s_addr = inet_addr("192.168.149.129");  // 指定ip地址
```

```c++
uint32_t htonl(uint32_t _hostlong) //htonl就是把本机字节顺序转化为网络字节顺序
/*
    h---host 本地主机
    to  就是to 了
    n  ---net 网络的意思
    l 是 unsigned long
    long型的0x40写完整为:0x 00 00 00 40，共四个字节，调用htonl后四个字节颠倒顺序，为0x 40 00 00 00。
*/
    
m_servaddr.sin_addr.s_addr = htonl(INADDR_ANY);  // 本主机的任意ip地址
```

```c++
uint16_t htons(uint16_t _hostshort);//将一个无符号短整型数值转换为网络字节序
//计算机的端口数量是65536个，也就是2^16，两个字节
//简单地说,htons()就是将一个数的高低位互换   (如:12 34 --> 34 12)  
     
m_servaddr.sin_port = htons(5000);  // 通信端口
```



```c++
struct hostent* h;
  if ( (h = gethostbyname(argv[1])) == 0 )   // 客户端指定服务端的ip地址。
  { 
      printf("gethostbyname failed.\n"); close(sockfd); 
      return -1; 
  }
```

```c++
servaddr.sin_port = htons(atoi(argv[2]));//客户端程序指定服务端的通信端口
```





### 4.2.12 gethostbyname函数 把ip地址或域名转换为hostent 结构体表达的地址。

```c++
struct hostent *gethostbyname(const char *name);
```

参数name，域名或者主机名，例如"192.168.1.3"、"www.freecplus.net"等。

返回值：如果成功，返回一个hostent结构指针，失败返回NULL。

gethostbyname只用于客户端。

gethostbyname只是把字符串的ip地址转换为结构体的ip地址，只要地址格式没错，一般不会返回错误。失败时不会设置errno的值。









### 服务器端程序

```c++
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <iostream>
using namespace std;

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        cout << "Using:./server ip port\nExample:./server 5005\n\n";
        return -1;
    }

    //第一步：创建服务端socket
    int listenfd;
    listenfd = socket(AF_INET, SOCK_STREAM, 0);
    if (listenfd == -1)
    {
        cerr << "socket";
        return -1;
    }

    //第二步：服务器端将通信的地址和端口绑定到socket
    struct sockaddr_in serveraddr;                  // 服务端地址信息的数据结构。
    serveraddr.sin_family = AF_INET;                // 协议族，在socket编程中只能是AF_INET。
    serveraddr.sin_addr.s_addr = htonl(INADDR_ANY); // 任意ip地址。
    serveraddr.sin_port = htons(atoi(argv[1]));     //指定通信端口
    if (bind(listenfd, (struct sockaddr *)&serveraddr, sizeof(serveraddr)) != 0)
    {
        cerr << "bind";
        return -1;
    }

    //第三步：将socket设置为监听模式
    if (listen(listenfd, 5) != 0)
    {
        cerr << "listen";
        close(listenfd);
        return -1;
    }

    //第四步：接受客户端的链接
    int sockfd;
    struct sockaddr_in clientaddr;            //客户端的地址信息
    int addrlen = sizeof(struct sockaddr_in); //客户端地址的大小
    sockfd=accept(listenfd,(struct sockaddr*)&clientaddr,(socklen_t *)&addrlen);
    cout <<"客户端"<<inet_ntoa(clientaddr.sin_addr)<<"已链接"<<endl;

    //第五步：与客户端通信，接收客户端发过来的报文后。
    char buffer[1024];
    while (1)
    {
        int iret;
        // 接收客户端的回应报文。
        memset(buffer, 0, sizeof(buffer));
        iret = recv(sockfd, buffer, sizeof(buffer), 0);
        if (iret <= 0)
        {
            cout << "iret=" << iret << endl;
            break;
        }
        cout << "recv:" << buffer << endl;

        // 向客户端发送报文
        cout << "input: " << flush;
        cin.getline(buffer, 1024);
        if (strcmp(buffer, "quit") == 0)
            break;
        // 向服务端发送请求报文。
        iret = send(sockfd, buffer, strlen(buffer), 0);
        if (iret <= 0)
        {
            cerr << "send" << endl;
            break;
        }

        
    }

    // 第六步：关闭socket，释放资源。
    close(sockfd);
    close(listenfd);
    return 0;
}
```



### 客户端程序

```c++
/*
 * 程序名：client.cpp，此程序用于演示socket的客户端
 * 作者：C语言技术网(www.freecplus.net) 日期：20190525
 */
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <iostream>
using namespace std;

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        printf("Using:./client ip port\nExample:./client 127.0.0.1 5005\n\n");
        return -1;
    }

    // 第1步：创建客户端的socket。
    int sockfd;
    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    {
        perror("socket");
        return -1;
    }

    // 第2步：向服务器发起连接请求。
    struct hostent *h;
    if ((h = gethostbyname(argv[1])) == 0) // 指定服务端的ip地址。
    {
        printf("gethostbyname failed.\n");
        close(sockfd);
        return -1;
    }
    struct sockaddr_in servaddr;
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(atoi(argv[2])); // 指定服务端的通信端口。
    memcpy(&servaddr.sin_addr, h->h_addr, h->h_length);
    if (connect(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) != 0) // 向服务端发起连接清求。
    {
        perror("connect");
        close(sockfd);
        return -1;
    }

    char buffer[1024];

    // 第3步：与服务端通信，发送一个报文后等待回复，然后再发下一个报文。
    // for (int ii = 0; ii < 3; ii++)
    // {
    //     int iret;
    //     memset(buffer, 0, sizeof(buffer));
    //     sprintf(buffer, "这是第%d个超级女生，编号%03d。", ii + 1, ii + 1);
    //     if ((iret = send(sockfd, buffer, strlen(buffer), 0)) <= 0) // 向服务端发送请求报文。
    //     {
    //         perror("send");
    //         break;
    //     }
    //     printf("发送：%s\n", buffer);

    //     memset(buffer, 0, sizeof(buffer));
    //     if ((iret = recv(sockfd, buffer, sizeof(buffer), 0)) <= 0) // 接收服务端的回应报文。
    //     {
    //         printf("iret=%d\n", iret);
    //         break;
    //     }
    //     printf("接收：%s\n", buffer);
    // }

    while (1)
    {
        int iret;
        cout << "input: " << flush;
        cin.getline(buffer, 1024);
        if (strcmp(buffer, "quit")==0)
            break;
        // 向服务端发送请求报文。
        iret = send(sockfd, buffer, strlen(buffer), 0);
        if (iret <= 0)
        {
            cerr << "send" << endl;
            break;
        }

        // 接收服务端的回应报文。
        memset(buffer, 0, sizeof(buffer));
        iret = recv(sockfd, buffer, sizeof(buffer), 0);
        if (iret <= 0)
        {
            cout << "iret=" << iret << endl;
            break;
        }
        cout << "recv:" << buffer << endl;
    }

    // 第4步：关闭socket，释放资源。
    close(sockfd);
}
```



### 4.2.13 客户端封装类

```c++
//CTcpClient.h

#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>

class CTcpClient
{
private:
    
public:
    int m_sockfd;
    CTcpClient();
    ~CTcpClient();

    /*
     *   向服务器端发起链接
     *   serverip 服务端ip
     *   port 通信端口
     */
    bool ConnectToServer(const char *serverip, const int port);

    // 向对端发送报文。buf为报文，buflen为报文长度(strlen)
    int Send(const void *buf, const int buflen);

    // 接收对端的报文。buf为报文，buflen为报文长度(sizeof)
    int Recv(void *buf, const int buflen);
};
```

```c++
//CTcpClient.cpp

#include "CTcpClient.h"

CTcpClient::CTcpClient()
{
    m_sockfd = 0;
}

CTcpClient::~CTcpClient()
{
    if (m_sockfd != 0)
        close(m_sockfd);
}

/*
 *   向服务器端发起链接
 *   serverip 服务端ip
 *   port 通信端口
 */
bool CTcpClient::ConnectToServer(const char *serverip, const int port)
{
    m_sockfd = socket(AF_INET, SOCK_STREAM, 0); // 创建客户端的socket

    struct hostent *h; // ip地址信息的数据结构
    if ((h = gethostbyname(serverip)) == 0)
    {
        close(m_sockfd);
        m_sockfd = 0;
        return false;
    }

    struct sockaddr_in servaddr;
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(port);
    memcpy(&servaddr.sin_addr, h->h_addr, h->h_length);

    // 向服务器发起连接请求
    if (connect(m_sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) != 0)
    {
        printf("connect fail\n");
        close(m_sockfd);
        m_sockfd = 0;
        return false;
    }
    return true;
}

// 向对端发送报文。buf为报文，buflen为报文长度
int CTcpClient::Send(const void *buf, const int buflen)
{
    return send(m_sockfd, buf, buflen, 0);
}

// 接收对端的报文。buf为报文，buflen为报文长度
int CTcpClient::Recv(void *buf, const int buflen)
{
    return recv(m_sockfd, buf, buflen, 0);
}
```



### 4.2.14 服务器端封装类

```c++
//CTcpServer.h
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
 
class CTcpServer
{
private:
  int m_clientlen;    //结构提structadd_in的大小
  struct sockaddr_in m_clientaddr;  //客户端的地址信息
  struct sockaddr_in servaddr; // 服务端地址信息的数据结构
public:
  int m_listenfd;   // 服务端用于监听的socket
  int m_clientfd;   // 客户端连上来的socket
 
  CTcpServer();
   ~CTcpServer();

  // 初始化服务端
  bool InitServer(int port);  
  
  // 等待客户端的连接
  bool Accept();  
 
  // 向对端发送报文
  int  Send(const void *buf,const int buflen);

  // 接收对端的报文
  int  Recv(void *buf,const int buflen);

  // 输出客户端连接的IP地址
  char* GetIP();
};
```

```c++
//CTcpServer.cpp
#include "CTcpServer.h"
CTcpServer::CTcpServer()
{
  m_clientlen = sizeof(struct sockaddr_in);
}

CTcpServer::~CTcpServer()
{
  if (m_listenfd != 0)
    close(m_listenfd); // 析构函数关闭socket
  if (m_clientfd != 0)
    close(m_clientfd); // 析构函数关闭socket
}

// 初始化服务端的socket，port为通信端口
bool CTcpServer::InitServer(int port)
{
  m_listenfd = socket(AF_INET, SOCK_STREAM, 0); // 创建服务端的socket

  // 把服务端用于通信的地址和端口绑定到socket上
  memset(&servaddr, 0, sizeof(servaddr));
  servaddr.sin_family = AF_INET;                // 协议族，在socket编程中只能是AF_INET
  servaddr.sin_addr.s_addr = htonl(INADDR_ANY); // 本主机的任意ip地址
  servaddr.sin_port = htons(port);              // 绑定通信端口
  if (bind(m_listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) != 0)
  {
    close(m_listenfd);
    m_listenfd = 0;
    return false;
  }

  // 把socket设置为监听模式
  if (listen(m_listenfd, 5) != 0)
  {
    close(m_listenfd);
    m_listenfd = 0;
    return false;
  }
  return true;
}

// 等待客户端的连接
bool CTcpServer::Accept()
{
  if ((m_clientfd = accept(m_listenfd, (struct sockaddr *)&m_clientaddr, (socklen_t *)&m_clientlen)) <= 0)
    return false;
  return true;
}

// 向对端发送报文
int CTcpServer::Send(const void *buf, const int buflen)
{
  return send(m_clientfd, buf, buflen, 0);
}

// 接收对端的报文
int CTcpServer::Recv(void *buf, const int buflen)
{
  return recv(m_clientfd, buf, buflen, 0);
}

// 输出客户端连接的IP地址
char* CTcpServer::GetIP()
{
  return (inet_ntoa(m_clientaddr.sin_addr));
}

```











## 4.3 同步，异步，阻塞和，阻塞和多路复用I/O





### 同步I/O

在一个线程中，CPU执行代码的速度极快，然而，一旦遇到I/O操作，如读写文件、发送网络数据时，就需要等待 I/O 操作完成，才能继续进行下一步操作，这种情况称为**同步 I/O。**

可以使用 **多线程或者多进程** 来并发执行代码，当某个线程/进程被挂起后，不会影响其他线程或进程。





### 异步I/O

当程序需要对I/O进行操作时，它只发出I/O操作的指令，并不等待I/O操作的结果，然后就去执行其他代码了。一段时间后，当I/O返回结果时，再通知CPU进行处理。这样子用户空间中的程序不需要等待内核空间中的 I/O 完成实际操作，就可执行其他任务，提高CPU的利用率。

**简单来说就是，用户不需要等待内核完成实际对io的读写操作就直接返回了。**





### 阻塞I/O

![socket_io001](https://s2.loli.net/2022/06/30/O75WpYkyJmwiVg2.png)

阻塞I/O的特点就是在IO执行的两个阶段（用户空间与内核空间）都被阻塞



### 非阻塞I/O

![socket_io002](https://s2.loli.net/2022/06/30/QC3s5PZ8oFivtjN.png)

当用户进程调用 `read()/recvfrom()` 等系统调用函数时，如果内核空间中的数据还没有准备好，那么它并不会阻塞用户进程，而是立刻返回一个error。当内核空间的数据准备好了，它就会将数据从内核空间中拷贝到用户空间，此时用户进程也就得到了数据。

非阻塞I/O的特点是用户进程需要不断的 **主动询问** 内核空间的数据准备好了没有。



### 多路复用I/O 

多路复用就是单个进程可以同时处理多个网络连接的I/O

**select，poll，epoll**等函数不断轮询他们负责的所有socket，当某个socket有数据到达了，就通知用户进程。

一般来说I/O复用多用于以下情况：

1. 当客户处理多个描述符时。
2. 服务器在高并发处理网络连接的时候。
3. 服务器既要处理监听套接口，又要处理已连接套接口，一般也要用到I/O复用。
4. 如果一个服务器即要处理TCP，又要处理UDP，一般要使用I/O复用。
5. 如果一个服务器要处理多个服务或多个协议，一般要使用I/O复用

与多进程和多线程技术相比， **I/O多路复用技术的最大优势是系统开销小，系统不必创建进程/线程，也不必维护这些进程/线程** ，从而大大减小了系统的开销。但select，poll，epoll本质上都是同步I/O，因为他们都需要 **在读写事件就绪后自己负责进行读写** ，也就是说这个读写过程是 **阻塞** 的，而 **异步I/O则无需自己负责进行读写，异步I/O的实现会负责把数据从内核拷贝到用户空间** 。



#### select

select机制会监听它所负责的所有socket，当其中一个socket或者多个socket可读或者可写的时候，它就会返回，而如果所有的socket都是不可读或者不可写的时候，这个进程就会被阻塞，直到超时或者socket可读写，当select函数返回后，可以通过遍历fdset，来找到就绪的描述符。

![socket_io003](https://s2.loli.net/2022/07/05/apmcyObnXk9N2lS.png)

1. 用户进程调用 `select()` 函数，如果当前没有可读写的socket，则用户进程进入阻塞状态。
2. 对于内核空间来说，它会从用户空间拷贝 `fd_set` 到内核空间，然后在内核中遍历一遍所有的socket描述符，如果没有满足条件的socket描述符，内核将进行休眠，当设备驱动发生自身资源可读写后，会唤醒其等待队列上睡眠的内核进程，即在socket可读写时唤醒，或者在超时后唤醒。
3. 返回 `select()` 函数的调用结果给用户进程，返回就绪 `socket` 描述符的数目，超时返回0，出错返回-1。
4. 注意，在 `select()` 函数返回后还是需要轮询去找到就绪的 `socket` 描述符的，此时用户进程才可以去操作 `socket` 。

![socket_io004](https://doc.embedfire.com/linux/imx6/base/zh/latest/_images/socket_io004.png)

**缺点：**

1. 单个进程最大可以监视1024个文件描述符
2. 需要存放的fd的数据结构，内核态与用户态传递该结构时复制开销大
3. 寻找活跃描述符时，需要遍历所有fd，带来大量的时间消耗

**函数原型**

```c++
int select(int maxfdp1,fd_set *readset,fd_set *writeset,fd_set *exceptset,const struct timeval *timeout)
```

- maxfdp1指定感兴趣的socket描述符个数，它的值是套接字最大socket描述符加1，socket描述符0、1、2 … maxfdp1-1均将被设置为感兴趣（即会查看他们是否可读、可写）。
- readset：指定这个socket描述符是可读的时候才返回。
- writeset：指定这个socket描述符是可写的时候才返回。
- exceptset：指定这个socket描述符是异常条件时候才返回。
- timeout：指定了超时的时间，当超时了也会返回。

如果对某一个的条件不感兴趣，就可以把它设为空指针。

返回值：就绪 `socket` 描述符的数目，超时返回0，出错返回-1。





#### poll

`poll` 的实现和 `select` 非常相似，只是描述fd集合的方式不同， `poll` 使用 `pollfd` 结构而不是 `select` 的 `fd_set` 结构，poll不限制socket描述符的个数，因为它是使用链表维护这些socket描述符的，其他的都差不多和 `select()` 函数一样， `poll()` 函数返回后，需要轮询 `pollfd` 来获取就绪的描述符，根据描述符的状态进行处理，但是poll没有最大文件描述符数量的限制。 `poll` 和 `select` 同样存在一个缺点就是，包含大量文件描述符的数组被整体复制于用户态和内核的地址空间之间，而不论这些文件描述符是否就绪，它的开销随着文件描述符数量的增加而线性增大。

```c++
int poll (struct pollfd *fds, unsigned int nfds, int timeout);
```





#### epoll

epoll使用一个`epfd文件描述符`管理多个socket描述符。**将用户空间的socket描述符的事件存放到内核的一个事件表中** ，这样在用户空间和内核空间的copy只需一次。当epoll记录的socket产生就绪的时候，epoll会通过callback的方式来激活这个fd，这样子在epoll_wait便可以收到通知，告知应用层哪个socket就绪了，这种通知的方式是可以直接得到那个socket就绪的，因此相比于 `select` 和 `poll` ，它不需要遍历socket列表，时间复杂度是O(1)，不会因为记录的socket增多而导致开销变大。



##### epoll的操作模式

- LT模式：即水平出发模式，当epoll_wait检测到socket描述符处于就绪时就通知应用程序，应用程序可以不立即处理它。下次调用epoll_wait时，还会再次产生通知。
- ET模式：即边缘触发模式，当epoll_wait检测到socket描述符处于就绪时就通知应用程序，应用程序 **必须** 立即处理它。如果不处理，下次调用epoll_wait时，不会再次产生通知。

**ET模式在很大程度上减少了epoll事件被重复触发的次数，因此效率要比LT模式高** 。epoll工作在ET模式的时候，必须使用非阻塞套接口，以避免由于一个文件句柄的阻塞读/阻塞写操作把处理多个文件描述符的任务饿死。



##### epoll函数

**epoll_create函数**

```c++
int epoll_create(int size);
```

创建一个epoll的 **epfd**，size参数用来告诉内核这个监听的数目一共有多大



**epoll_ctl函数**

用于控制某个epoll文件描述符上的事件，可以注册事件，修改事件，以及删除事件。

```c++
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
```

- epdf：由epoll_create()函数返回的epoll文件描述符（句柄）。
- op：op是操作的选项，目前有以下三个选项：
- EPOLL_CTL_ADD：注册要监听的目标socket描述符fd到epoll句柄中。
- EPOLL_CTL_MOD：修改epoll句柄已经注册的fd的监听事件。
- EPOLL_CTL_DEL：从epoll句柄删除已经注册的socket描述符。
- fd：指定监听的socket描述符。
- event：event结构如下：

```c++
typedef union epoll_data {
    void        *ptr;
    int          fd;
    uint32_t     u32;
    uint64_t     u64;
} epoll_data_t;

struct epoll_event {
    uint32_t     events;      /* Epoll events */
    epoll_data_t data;        /* User data variable */
};
```

- events可以是以下几个宏的集合：
  - EPOLLIN：表示对应的文件描述符可以读（包括对端SOCKET正常关闭）。
  - EPOLLOUT：表示对应的文件描述符可以写。
  - EPOLLPRI：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）。
  - EPOLLERR：表示对应的文件描述符发生错误。
  - EPOLLHUP：表示对应的文件描述符被挂断。
  - EPOLLET： 将EPOLL设为边缘触发(Edge Triggered)模式，这是相对于水平触发(Level Triggered)来说的。
  - EPOLLONESHOT：只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里。
  - 

**epoll_wait()函数**

epoll_wait()函数的作用就是等待监听的事件的发生，类似于调用select()函数。

```c++
int epoll_wait(int epfd, struct epoll_event *events,
               int maxevents, int timeout);
```

- events：用来从内核得到事件的集合。

- maxevents：告知内核这个events有多大，这个maxevents的值不能大于创建epoll_create()时的指定的size。
- timeout：超时时间。
- 函数的返回值表示需要处理的事件数目，如返回0表示已超时。



##### epoll高效

- epoll_wait()函数返回的时就绪描述符数量，需要去epoll指定数组获得指定数量的socket描述符，时间复杂度为O(1)
- 采用内存映射技术，用户空间和内核空间的逻辑地址映射在同一物理地址，减少数据交换
- select/poll，进程调用一定方法后，内核才对所有监视的socket描述符进行扫描。epoll使用`epoll_ctl()`注册socket描述符，当描述符就绪时，内核采用回调机制，激活socket描述符，调用`epoll_wait()`可以得到通知，只管理就绪的描述符







## 4.4 并发网络服务器



### 4.4.1 多进程网络服务器

1. 初始化设置父进程信号处理函数（关闭父进程监听socket）
2. 父进程监听，并且关闭父进程与客户端的socket
3. 子进程负责与客户端进程进行通信，关闭监听socket，设置子进程信号处理函数（遇到信号释放子进程资源）
4. 子进程结束，调用子进程退出函数

```c++
#include "CTcp/CTcpServer.h"
#include <iostream>
#include <signal.h>
using namespace std;

CTcpServer TcpServer;

//父进程处理函数
void FathEXIT(int sig)
{
    if (sig > 0)
    {
        signal(sig, SIG_IGN);
        signal(SIGINT, SIG_IGN);
        signal(SIGTERM, SIG_IGN);
        printf("catching the signal(%d).\n", sig);
    }
    close(TcpServer.m_listenfd);
    kill(0, 15);
    printf("父进程退出。\n");

    // 编写善后代码（释放资源、提交或回滚事务）
    exit(0);
}

//子进程处理函数
void ChldEXIT(int sig)
{
    if (sig > 0)
    {
        signal(sig, SIG_IGN);
        signal(SIGINT, SIG_IGN);
        signal(SIGTERM, SIG_IGN);
    }

    printf("子进程退出。\n");
    // 编写善后代码（释放资源、提交或回滚事务）
    exit(0);
}

int main(int argc, char *argv[])
{
    if (argc < 2)
    {
        cout << "example: server 5050(port_name)" << endl;
        return -1;
    }
	//父进程信号函数
    signal(SIGINT, FathEXIT);
    signal(SIGTERM, FathEXIT);
    
    //初始化Server的通信端口
    if (TcpServer.InitServer(atoi(argv[1])) == false)
    {
        cout << "tcpServer.initserver failed" << endl;
        return -1;
    }
    while (1)
    {
        //等待客户端连接
        if (TcpServer.Accept() == false)
        {
            cout << "tcpServer.Accept failed" << endl;
            return -1;
        }
        
        //建立进程
        if(fork()>0)
        {
            close(TcpServer.m_clientfd);//父进程关闭与客户端建立的socket
            continue;
        }
        
        //子进程信号函数
        signal(SIGINT, ChldEXIT);
        signal(SIGTERM, ChldEXIT);
        
        close(TcpServer.m_listenfd);//子进程关闭listen的socket
        cout << "客户端" << TcpServer.GetIP() << "已连接" << endl;
        
        char buffer[1024];
        //子进程与客户端进行通信
        while(true)
        {
            memset(buffer, 0, sizeof(buffer));
            if (recv(TcpServer.m_clientfd, buffer,  sizeof(buffer), 0) <= 0)
                break;
            cout << TcpServer.m_clientfd << "接收:" << buffer << endl;
            //cout << getpid() << "接收:" << buffer << endl;

            strcpy(buffer, "ok");
            if (send(TcpServer.m_clientfd, buffer,  sizeof(buffer), 0) <= 0)
                break;
            cout << TcpServer.m_clientfd  << "发送:" << buffer << endl;
            //cout << getpid() << "发送:" << buffer << endl;
		}
        cout << "客户端已断开"<<endl;
        ChldEXIT(0);
    }
    return 0;
}
```











### 4.4.2 多线程网络服务器

1. 主进程负责监听
2. 线程负责和客户端进程进行通信
3. 线程结束调用线程清理函数，关闭与客户端建立的socket
4. SIGINT信号处理函数，关闭服务端的监听socket，并取消所有线程

```c++
#include "CTcp/CTcpServer.h"
#include <iostream>
#include <pthread.h>
#include <signal.h>
#include <vector>
using namespace std;

CTcpServer Server;
vector<pthread_t> vpthread;

//信号处理函数
void signalhandler(int arg)
{
    cout << "信号处理函数开始" << endl;
    close(Server.m_listenfd);
    for (int i = 0; i < vpthread.size(); i++)
    {
        cout << vpthread[i] << "取消！" << endl;
        pthread_cancel(vpthread[i]);
    }
    cout << "信号处理函数结束" << endl;
}

//线程清理函数
void cleanfunc(void *arg)
{
    cout << "线程清理函数开始" << endl;
    close((int)(long)(arg));
    for (int i = 0; i < vpthread.size(); i++)
    {
        if (vpthread[i] == pthread_self())
            vpthread.erase(vpthread.begin() + i);
    }
    cout << "线程清理函数结束" << endl;
}

//线程调用函数
void *pthmain(void *arg)
{
    pthread_cleanup_push(cleanfunc, arg);
    pthread_detach(pthread_self());
    // thread_setcanceltype()
    int clientfd = (int)(long)arg;
    char buffer[1024];

    while (1)
    {
        memset(buffer, 0, sizeof(buffer));
        if (recv(clientfd, buffer, sizeof(buffer), 0) <= 0)
            break;
        cout << clientfd << "接收:" << buffer << endl;

        strcpy(buffer, "ok");
        if (send(clientfd, buffer, sizeof(buffer),0 ) <= 0)
            break;
        cout << clientfd << "发送:" << buffer << endl;
    }
    cout << "客户端已断开" << endl;
    pthread_cleanup_pop(1);
}

int main(int argc, char *argv[])
{
    signal(2, signalhandler);

    //初始化Server的通信端口
    if (Server.InitServer(atoi(argv[1])) == false)
    {
        cout << "tcpServer.initserver failed" << endl;
        return -1;
    }
    while (1)
    {
        //等待客户端连接
        if (Server.Accept() == false)
        {
            cout << "tcpServer.Accept failed" << endl;
            return -1;
        }
        cout << "客户端" << Server.GetIP() << "已连接" << endl;

        pthread_t thread;
        if (pthread_create(&thread, NULL, pthmain, (void *)(long)Server.m_clientfd) != 0)
        {
            cout << "pthread_create failed!" << endl;
            return -1;
        }
        vpthread.push_back(thread);
    }
    return 0;
}
```











