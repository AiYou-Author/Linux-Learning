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
