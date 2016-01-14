title: 读书笔记-Linux Bible 9th Edition之玩转文本文件
date: 2015-11-30 15:04:46
toc: true
tags:
 - linux
 - 读书笔记
---
>根据书的第5章整理一下关于操作文本文件的常用命令。在Linux系统中许多信息都是在文本文件中管理的,所以熟练掌握对文本文件的更改，查找是很重要的。这一点，在出bug查找服务器日志时真是深有体会啊！😼😼

相关博客:
 [Linux Bible 9th Edition之使用shell](http://yemengying.com/2015/11/23/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0-Linux-Bible-9th-Edition/)
 [Linux Bible 9th Edition之文件系统](http://yemengying.com/2015/11/26/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0-Linux-Bible-9th-Edition%E4%B9%8B%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F/)
 
![李光珠](/images/liguangzhu.gif)



<!-- more -->

### 使用vim和vi编辑文件

>刚接触vi编辑器可能会觉得有点难，不过当你熟悉了之后可以只用键盘就能快速高效的编辑文件，无需使用鼠标或功能键。(如果觉得vi不适合你，可以选择其它的文本编辑器，比如:nano,gedit,jed,kate,kedit,mcedit,nedit...等等)

从最常见的打开文件的开始了解vi    
打开文件:

```
 $ vi test
```
如果这是一个空的文件，你会看到类似下图的东东。最上面闪烁的东东代表了光标的当前位置，最下面的一行显示了关于文件的一些信息,中间的"~"符号代表没有内容。
![使用vi打开一个空文件](/images/vi1.jpg)
当你看到这个界面可能会感觉不知所措，因为没有任何菜单，提示和图标来告诉你该做什么。更恐怖的是，你不能直接输入，否则会听见"嘟嘟"的声音。

#### 添加文件内容

不要怕,首先,需要了解两种主要的模式:命令模式(command)和编辑模式(input)。vi编辑器以命令模式启动，在添加或改变文本内容之前需输入命令来告诉vi你想要做什么(大小写敏感)。输入下面的命令就可以进入编辑模式，当编辑结束后，按*Esc*键就可以回到命令模式。

|命令|作用|
|:---|:---|
|a|可以在光标右侧开始插入文本
|A|可以在当前行的最后开始插入文本
|i|可以在光标左侧开始插入文本
|I|可以在当前行的最前面开始插入文本
|o|在当前行下面 插入新的一行 
|O|在当前行上面 插入新的一行

>![提示](/images/tips.jpg) 当进入编辑模式时，屏幕下方会出现-- INSERT --；编辑结束后，按*Esc*键就可以回到命令模式。不过如果输入了":"符号，需要按两下*Esc*键

#### 在文本中移动

可以使用方向键可以在文本中移动光标，但还有一些小技巧可以让我们更方便的在文本中移动

|命令|作用|
|:---|:---|
|w|光标移到下一个单词的开头(单词以spaces,tabs,标点界定)
|W|光标移到下一个单词的开头(单词以spaces,tabs界定)
|b|光标移到前一个单词的开头(单词以spaces,tabs,标点界定)
|B|光标移到前一个单词的开头(单词以spaces,tabs界定)
|0(zero)|光标移到当前行的最前面
|$|光标移到当前行的最后
|H|光标移到屏幕的左上角
|M|移到中间行第一个字符
|L|光标移到屏幕的左下角

#### 删除，复制，更改文本

了解了如何添加文本和移动光标是远远不够的，还需要知道如何删除，复制和更改文本。命令x,d,y,c等可以帮助我们删除和修改文本，这些命令也可以和移动光标的命令(上一个表格中提到的)或者数字配合使用来告诉编辑器确切的操作是什么。

|命令|作用|
|:---|:---|
|x|删除光标所在位置的字符
|X|删除光标所在位置的前一个字符
|d?|删除一些文本
|c?|更改一些文本
|y?|复制一些文本

?代表这些命令要和移动光标的命令配合着使用，下面是一些例子

|命令|作用|
|:---|:---|
|dw|删除当前光标位置的后一个单词
|db|删除当前光标位置的前一个单词
|dd|删除当前一整行
|c$|更改(实际上是擦除)从当前位置到当前行最后的内容，并进入编辑模式
|c0|更改(实际上是擦除)从当前位置到当前行最前面的内容，并进入编辑模式
|yy|将当前行复制到buffer中

上面这些命令也可以和数字配合使用,下面是栗子🌰

|命令|作用|
|:---|:---|
|3dd|删除当前行往下的三行
|3dw|删除接下来的三个单词
|5cl|删除接下来的5个字符，并进入编辑模式

#### 粘贴
可以使用命令p和P，将复制到buffer中的内容粘贴到文本中。p是将缓存区的内容粘贴到当前光标所在位置的下方，P是将缓存区的内容粘贴到当前光标所在位置的上方

#### 在文件中跳跃

|命令|作用|
|:---|:---|
|ctrl+f|向下一页|
|ctrl+b|向上一页|
|ctrl+d|向下一页半|
|ctrl+u|向上一页半|
|G|跳到最后一行
|1G|跳到第一行|
|35G|跳到第35行|

#### 查找文本

查找文本时,"/"和"?"分别对应向前和向后查找,也可以使用一些通配符，比如/The.*foot,
?[pP]rint,查找之后可以按n和N来重复查找和按相反方向查找

|命令|作用|
|:---|:---|
|/hello|向前查找单词"hello"
|?goodbye|向后查找单词"goodbye"


#### 使用ex模式
当输入冒号,并且光标在最下方时就进入了ex模式，下面是一些在ex模式下查找，修改文本的栗子🌰

|命令|作用|
|:---|:---|
|:g/local|查找local 并打印
|:s/local/r|将local第一次出现的位置替换为r



#### 退出vi

以下的命令用来保存和退出文件

|命令|作用|
|:---|:---|
|ZZ|保存修改 并退出vi
|:w|保存修改 但不退出vi
|:wq|与ZZ命令一样
|:q|退出文件，该命令只有在没有未保存的修改下才起效
|:q!|退出文件 不保存对文件的修改

### 查找文件

> 为了帮助用户更有效的查找他们的文件，linux系统提供了locate,find,grep三个命令，依次来看看他们的作用。

#### 使用localte命令查找文件
>大多数linux系统中，updatedb命令会每天执行一次，将系统中文件的名字存到数据库中。通过locate命令，我们可以查找存在在数据库中的文件的位置。相较于find命令，locate命令效率更高，因为它搜索数据库而不是整个文件系统。不过locate命令也有它的缺点，它并不能找到所有存放在系统的文件，因为并不是所有的文件都会存储于数据库中，/etc/updatedb.conf文件决定了哪些文件将存在于数据库中。另外，普通用户无法通过数据库查找那些他们在文件系统中没有权利查看的文件，比如，普通用户无法在/root目录下执行ls命令，那么他们也无法通过locate查找这个目录下的文件。如果用locate命令查找一个字符串，那么这个字符串可能出现在返回文件的路径的任意位置。举个栗子，查找passwd，结果可能为/etc/passwd,/usr/bin/passwd和其它路径包含passwd的文件。还需要注意的是，如果创建一个文件之后，希望立刻通过locate查找它，最好执行命令updatedb更新下数据库。

```
  $ locate .bashrc
  /etc/skel/.bashrc 
  /home/cnegus/.bashrc
  
  # locate ./bashrc (身份不同 查找结果不同)
  /etc/skel/.bashrc 
  /home/bill/.bashrc 
  /home/joe/.bashrc 
  /root/.bashrc
  
  $ locate -i muttrc（-i 忽略大小写）  /etc/Muttrc  /etc/Muttrc.local   /usr/share/doc/mutt-1.5.20/sample.muttrc
  $ locate services (查找的字符串可能出现在文件路径中)  /etc/services  /usr/share/services/bmp.kmgio   /usr/share/services/data.kmgio
```

#### 使用find命令查找文件
>由于有许多不同的属性，find命令是查找文件的利器。当执行find命令时，它会搜索整个文件系统，这会造成find命令比locate命令耗时长，但同时也能让用户查找到系统中最新的文件。find命令最大的优点在于所有你能想到的文件的属性，都可以通过它查找，例如名字，拥有者，权限，大小，修改时间等等，也可以进行组合查找。

```
  $ find (列出当前目录下所有的文件和目录)
  $ find /etc (列出/etc目录下所有的文件和目录，权限不足时会报错)
  $ find -ls (列出文件的拥有者，权限，大小等信息)
  $ find /etc -name passwd（根据名字查找）
  $ find /etc -iname '*passwd*' （可以使用通配符）
  $ find /bigdata -size +10G （查找大小大于10G的文件）
  $ find /smalldata -size -5M (查找大小小于5M的文件)
  $ find /home -user chris -ls (输出/home目录下拥有者是chris的文件的详细信息)
  $ find /home -user chris -or -user joe -ls (输出/home目录下拥有者是chris或joe的文件的详细信息)
  $ find /home -not -user root -ls (输出/home目录下拥有者不是root的文件的详细信息)
  $ find /bin -perm 755 -ls（通过权限查找）
  $ find . -perm -002 -type f -ls （通过类型查找）
```

find命令还有一个很棒的特性，可以使用-excute和-ok选项可以在查找到的任何文件上执行命令。excute选项会直接在每个找到的文件上执行命令，不会询问是否执行。而ok选项会在每个文件执行命令前询问是否执行。

```
$ find /etc -iname iptables -exec echo "I found {}" \; 
I found /etc/bash_completion.d/iptablesI found /etc/sysconfig/iptables
# find /var/allusers/ -user joe -ok mv {} /tmp/joe/ \;
mv ... /var/allusers/dict.dat > ? ymv ... /var/allusers/five > ? y
```

>![提示](/images/tips.jpg) 想了解更多关于find命令的信息，可以执行命令 man find

#### 使用grep命令在文件中查找
>如果希望查找包含特定内容的文件，可以使用grep命令。通过grep命令，可以搜索单个文件，也可以递归搜索整个目录。默认情况下，grep命令是大小写敏感的

```
 $ grep desktop /etc/services 
 desktop-dna 2763/tcp # Desktop DNA 
 desktop-dna 2763/udp # Desktop DNA $ grep -i desktop /etc/services sco-dtmgr 617/tcp # SCO Desktop Administration Server
 sco-dtmgr 617/udp # SCO Desktop Administration Server
 airsync 2175/tcp  # Microsoft Desktop AirSync Protocol 
```

第一个例子，是在/etc/services文件中查找字符串desktop.第二个例子通过选项-i,在查找时大小写不敏感
-v 是查找不包含指定内容的行，-r是在目录中递归查找，-l是列出文件名 而不是包含内容的具体行

```
 $ grep -v desktop /etc/services
 $ grep -rli peerdns /usr/share/doc/ 
 /usr/share/doc/dnsmasq-2.66/setup.html 
 /usr/share/doc/initscripts-9.49.17/sysconfig.txt
```

以我大谢耳朵结尾吧，主要看气质，哈哈~~
![sheldon](/images/sheldon.jpg)







