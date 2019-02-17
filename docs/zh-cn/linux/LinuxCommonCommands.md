#### cd    切换用户当前工作目录
```
cd (选项) (参数)
cd命令用来切换用户工作目录到一个指定的目录下，这个指定的目录你可以使用绝对路径或者是相对路径，（绝对路径代表从根目录开始，相对路径代表以当前目录为起点）
如果直接用 cd 命令而不加任何参数，则会切换到登录用户的主目录
比如我一开始是用root用户登录的，那么他已进入命令行就会进入到 /root/ 目录，使用cd 切换到其他目录后，直接cd（不加任何参数）那么他会切换回到/root/ 目录
cd 切换目录时要注意的几点
" ~ "表示为home directory（家目录或者叫用户目录）的意思
 " . "  表示目前所在的目录
 " .. " 表示目前目录位置的上一层目录。

实例
cd    进入用户主目录；
cd ~  进入用户主目录；
cd -  返回进入此目录之前所在的目录；
cd ..  返回上级目录（若当前目录为“/“，则执行完后还在“/"；".."为上级目录的意思）；
cd ../..  返回上两级目录；
cd !$  把上个命令的参数作为cd参数使用。
```
#### pwd 显示用户当前工作目录
```
pwd（选项）
pwd是用来显示用户当前的工作路径的 ，他是以绝对路径的形式呈现的
例如以下例子
[root@localhost linuxidc]# cd /root
[root@localhost ~]# pwd
/root
[root@localhost ~]# cd /home/linuxidc/
[root@localhost linuxidc]# pwd
/home/linuxidc
```
#### ls 显示目标列表（之前在讲目录结构的时候使用过的）
```
ls（选项）（参数）
参数可以为一个目录，如果什么都不加，代表当前目录
ls目录在我看来算是Linux中使用频率最高的命令了
在Windows下，打开文件资源管理器，进入一个目录，里面的文件目录，都是直接显示的
而在Linux下，进入一个目录，想查看一个目录里面的文件，或文件夹就必须用到ls命令了
ls命令的选项如下
-a：显示所有档案及目录（ls内定将档案名或目录名称为“.”的视为影藏，不会列出）；
-A：显示除影藏文件“.”和“..”以外的所有文件列表；
-C：多列显示输出结果。这是默认选项；
-l：与“-C”选项功能相反，所有输出信息用单列格式输出，不输出为多列；
-F：在每个输出项后追加文件的类型标识符，具体含义：“*”表示具有可执行权限的普通文件，“/”表示目录，“@”表示符号链接，“|”表示命令管道FIFO，“=”表示sockets套接字。当文件为普通文件时，<br>不输出任何标识符；
-b：将文件中的不可输出的字符以反斜线“”加字符编码的方式输出；
-c：与“-lt”选项连用时，按照文件状态时间排序输出目录内容，排序的依据是文件的索引节点中的ctime字段。与“-l”选项连用时，则排序的一句是文件的状态改变时间；
-d：仅显示目录名，而不显示目录下的内容列表。显示符号链接文件本身，而不显示其所指向的目录列表；
-f：此参数的效果和同时指定“aU”参数相同，并关闭“lst”参数的效果；
-i：显示文件索引节点号（inode）。一个索引节点代表一个文件；
--file-type：与“-F”选项的功能相同，但是不显示“*”；
-k：以KB（千字节）为单位显示文件大小；
-l：以长格式显示目录下的内容列表。输出的信息从左到右依次包括文件名，文件类型、权限模式、硬连接数、所有者、组、文件大小和文件的最后修改时间等；
-m：用“,”号区隔每个文件和目录的名称；
-n：以用户识别码和群组识别码替代其名称；
-r：以文件名反序排列并输出目录内容列表；
-s：显示文件和目录的大小，以区块为单位；
-t：用文件和目录的更改时间排序；
-L：如果遇到性质为符号链接的文件或目录，直接列出该链接所指向的原始文件或目录；
-R：递归处理，将指定目录下的所有文件及子目录一并处理；
--full-time：列出完整的日期与时间；
--color[=WHEN]：使用不同的颜色高亮显示不同类型的。
之前跟大家讲过每种颜色分别代表什么样的颜色，在这你也可以通过 --color 自己指定不同类型显示那种不同的颜色
下面是简单的演示
[root@localhost ~]# ls    #默认显示当前目录下的文件
1.txt  2.txt  3.txt  a  b  c
[root@localhost ~]# ls -a　　　　#显示当前目录下所有的文件,包含"."开头的隐藏文件
.  1.txt  3.txt  b              .bash_logout  .bashrc                c      .cshrc    .python_history  .viminfo
..  2.txt  a      .bash_history  .bash_profile  .bashrc-anaconda3.bak  .cache  .ipython  .tcshrc          .vimrc
[root@localhost ~]# ls -l      #显示当前目录下文件的详细信息，如权限，文件大小，修改时间
total 12
-rw-r--r--. 1 root root    0 Apr  5 10:29 1.txt
-rw-r--r--. 1 root root    0 Apr  5 10:29 2.txt
-rw-r--r--. 1 root root    0 Apr  5 10:29 3.txt
drwxr-xr-x. 2 root root 4096 Apr  5 10:29 a
drwxr-xr-x. 2 root root 4096 Apr  5 10:29 b
drwxr-xr-x. 2 root root 4096 Apr  5 10:29 c
[root@localhost ~]# ll　　　　　　　#等同与ls -l 命令，显示文件详细信息
total 12
-rw-r--r--. 1 root root    0 Apr  5 10:29 1.txt
-rw-r--r--. 1 root root    0 Apr  5 10:29 2.txt
-rw-r--r--. 1 root root    0 Apr  5 10:29 3.txt
drwxr-xr-x. 2 root root 4096 Apr  5 10:29 a
drwxr-xr-x. 2 root root 4096 Apr  5 10:29 b
drwxr-xr-x. 2 root root 4096 Apr  5 10:29 c
[root@localhost ~]# ls -al      #组合选项 相当于 ls -a -l 显示当前目录下所有文件或目录的详细信息
total 68
dr-xr-x---.  7 root root 4096 Apr  5 10:29 .
dr-xr-xr-x. 22 root root 4096 Apr  5 09:27 ..
-rw-r--r--.  1 root root    0 Apr  5 10:29 1.txt
-rw-r--r--.  1 root root    0 Apr  5 10:29 2.txt
-rw-r--r--.  1 root root    0 Apr  5 10:29 3.txt
drwxr-xr-x.  2 root root 4096 Apr  5 10:29 a
drwxr-xr-x.  2 root root 4096 Apr  5 10:29 b
-rw-------.  1 root root 4083 Apr  4 18:03 .bash_history
-rw-r--r--.  1 root root  18 May 20  2009 .bash_logout
-rw-r--r--.  1 root root  176 May 20  2009 .bash_profile
-rw-r--r--.  1 root root  247 Mar 12 05:07 .bashrc
-rw-r--r--.  1 root root  176 Mar 11 06:12 .bashrc-anaconda3.bak
drwxr-xr-x.  2 root root 4096 Apr  5 10:29 c
drwxr-xr-x.  3 root root 4096 Apr  1 05:19 .cache
-rw-r--r--.  1 root root  100 Sep 22  2004 .cshrc
drwxr-xr-x.  5 root root 4096 Mar 12 05:10 .ipython
-rw-------.  1 root root  32 Mar 31 17:16 .python_history
-rw-r--r--.  1 root root  129 Dec  3  2004 .tcshrc
-rw-------.  1 root root 4016 Apr  2 04:35 .viminfo
-rw-r--r--.  1 root root  25 Mar 12 05:10 .vimrc
其他选项可以自己动手去试，由于篇幅问题，我就不一一演示了
```
#### mv 移动文件目录命令
```
mv(选项)(参数)
mv命令可以用来移动一个文件或是一个目录，同时也可以用来作为改名的命令
他一般需要两个参数 ，source（源文件或源目录），target（目标文件或目标目录）
注意，如果目标路径中存在相同的文件，那么将会覆盖原先的文件，如果只是移动到当前目录下，而且与原先文件目录名不同，那么这只是一个改名的操作
mv的选项如下
--backup=<备份模式>：若需覆盖文件，则覆盖前先行备份；
-b：当文件存在时，覆盖前，为其创建一个备份；
-f：若目标文件或目录与现有的文件或目录重复，则直接覆盖现有的文件或目录；
-i：交互式操作，覆盖前先行询问用户，如果源文件与目标文件或目标目录中的文件同名，则询问用户是否覆盖目标文件。用户输入”y”，表示将覆盖目标文件；输入”n”，表示取消对源文件的移动。这样可<br>以避免误将文件覆盖。
--strip-trailing-slashes：删除源文件中的斜杠“/”；
-S<后缀>：为备份文件指定后缀，而不使用默认的后缀；
--target-directory=<目录>：指定源文件要移动到目标目录；
-u：当源文件比目标文件新或者目标文件不存在时，才执行移动操作。
一般我们再使用mv命令时是用不到选项的，所以只是列举选项以供参考
mv的应用
[root@localhost linuxidc]# ls
a.py  a.sh
[root@localhost linuxidc]# mv a.py b.py      #只是改名了
[root@localhost linuxidc]# ls
a.sh  b.py
[root@localhost linuxidc]# ls /root/a/
1.py  2.py  3.py  4.py  5.py
[root@localhost linuxidc]# pwd
/home/linuxidc
[root@localhost linuxidc]# mv /root/a/* .          #移动a目录下所有文件到当前目录(*代表的是通配符，.代表的是当前目录)
[root@localhost linuxidc]# ls
1.py  2.py  3.py  4.py  5.py  a.sh  b.py
```
#### mkdir 创建目录命令
```
mkdir (选项)(参数)
在Linux端可以使用mkdir来创建目录，如果你没有加其他的路径名，那么默认是在当前目录下创建目录，注意当文件夹存在时则提示不能创建
在这里说一下创建文件夹其实和windowns管理一样，要规划好如何去布局一个文件系统，在父目录下可以再创建子目录，每个目录尽量要存放相同类型的文件，这样更易于团队的管理和使用
mkdir的选项
-Z：设置安全上下文，当使用SELinux时有效；
-m<目标属性>或--mode<目标属性>建立目录的同时设置目录的权限；
-p或--parents 若所要建立目录的上层目录目前尚未建立，则会一并建立上层目录；
--version 显示版本信息。
你也可以直接通过--help来获取选项
例如我下面创建一个文件夹
[root@localhost ~]# mkdir myfolder
[root@localhost ~]# ls
myfolder
创建一个文件夹并设置权限（权限暂时还没有讲到，只是了解一下）
[root@localhost ~]# mkdir -m 711 newFolder
[root@localhost ~]# ll
total 8
drwxr-xr-x. 2 root root 4096 Apr  9 16:49 myfolder
drwx--x--x. 2 root root 4096 Apr  9 16:53 newFolder
权限是不是和之前创建的不一样了
当然你也可以直接创建多个目录（参数）
[root@localhost ~]# mkdir a b c
[root@localhost ~]# ls
a  b  c
创建包含子目录的目录（递归创建目录）
递归创建目录需要用到 -p选项
[root@localhost ~]# mkdir -p a/b/c
[root@localhost ~]# ls
a
[root@localhost ~]# cd a
[root@localhost a]# ls
b
[root@localhost a]# cd b
[root@localhost b]# ls
c
[root@localhost b]# cd c
当前路径创建了一个a，a中又创建了一个b,b中又创建了c
还可以通过列表来批量创建目录
[root@localhost ~]# mkdir -p a/{1,2,3,4}
[root@localhost ~]# ls
a
[root@localhost ~]# cd a
[root@localhost a]# ls
1  2  3  4
#### tree命令
tree(选项)(参数)
tree命令以树状图列出目录的内容
下面是tree的所有选项
-a：显示所有文件和目录；
-A：使用ASNI绘图字符显示树状图而非以ASCII字符组合；
-C：在文件和目录清单加上色彩，便于区分各种类型；
-d：先是目录名称而非内容；
-D：列出文件或目录的更改时间；
-f：在每个文件或目录之前，显示完整的相对路径名称；
-F：在执行文件，目录，Socket，符号连接，管道名称名称，各自加上"*"，"/"，"@"，"|"号；
-g：列出文件或目录的所属群组名称，没有对应的名称时，则显示群组识别码；
-i：不以阶梯状列出文件和目录名称；
-l：<范本样式> 不显示符号范本样式的文件或目录名称；
-l：如遇到性质为符号连接的目录，直接列出该连接所指向的原始目录；
-n：不在文件和目录清单加上色彩；
-N：直接列出文件和目录名称，包括控制字符；
-p：列出权限标示；
-P：<范本样式> 只显示符合范本样式的文件和目录名称；
-q：用“？”号取代控制字符，列出文件和目录名称；
-s：列出文件和目录大小；
-t：用文件和目录的更改时间排序；
-u：列出文件或目录的拥有者名称，没有对应的名称时，则显示用户识别码；
-x：将范围局限在现行的文件系统中，若指定目录下的某些子目录，其存放于另一个文件系统上，则将该目录予以排除在寻找范围外。
事实上用不上，只是参考
例如我们通过mkdir创建一个目录体系，然后通过tree来查看
[root@localhost a]# mkdir -p a/{1,2,3,4} b/{1,2,3} c/{1,2} d/1
[root@localhost a]# ls
a  b  c  d
[root@localhost a]# tree
.
├── a
│  ├── 1
│  ├── 2
│  ├── 3
│  └── 4
├── b
│  ├── 1
│  ├── 2
│  └── 3
├── c
│  ├── 1
│  └── 2
└── d
    └── 1
 
14 directories, 0 files
　　非常清晰的列出了文件的结构
注意：tree不仅会列出目录，还会列出目录中的所有文件
比如我在a下面的1文件夹创建了三个文件
[root@localhost a]# tree
.
├── a
│  ├── 1
│  │  ├── a.py
│  │  ├── b.py
│  │  └── c.py
│  ├── 2
│  ├── 3
│  └── 4
├── b
│  ├── 1
│  ├── 2
│  └── 3
├── c
│  ├── 1
│  └── 2
└── d
    └── 1
 
14 directories, 3 files
tree命令有很多用法，我这不一一解释了，你可以按照上面的选项去尝试一下
```
#### cp 复制命令
```
cp(选项)(参数)
cp命令用来将一个或多个源文件或者目录复制到指定的目的文件或目录。它可以将单个源文件复制成一个指定文件名的具体的文件或一个已经存在的目录下。cp命令还支持同时复制多个文件，当一次复制多个文件时，目标文件参数必须是一个已经存在的目录，否则将出现错误。
cp的一些选项
-a：此参数的效果和同时指定"-dpR"参数相同；
-d：当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录；
-f：强行复制文件或目录，不论目标文件或目录是否已存在；
-i：覆盖既有文件之前先询问用户；
-l：对源文件建立硬连接，而非复制文件；
-p：保留源文件或目录的属性；
-R/r：递归处理，将指定目录下的所有文件与子目录一并处理；
-s：对源文件建立符号连接，而非复制文件；
-u：使用这项参数后只会在源文件的更改时间较目标文件更新时或是名称相互对应的目标文件并不存在时，才复制文件；
-S：在备份文件时，用指定的后缀“SUFFIX”代替文件的默认后缀；
-b：覆盖已存在的文件目标前将目标文件备份；
-v：详细显示命令执行的操作。
cp的参数其实和mv有点类似，都是需要两个参数，一个是源文件路径，一个目标路径，只是mv是移动，文件数量并没有变，而cp命令是复制一个文件或目录
下面我们通过一个例子来熟悉以下这个命令
把a.py复制到a文件夹中
[root@localhost ~]# ls
a  a.py
[root@localhost ~]# cp a.py a/
[root@localhost ~]# ls
a  a.py
[root@localhost ~]# cd a
[root@localhost a]# ls
a.py
注意，同mv命令一样，肯定是会用到相对文件路径的，" . "代表当前文件路径，" .. "代表上一层文件路径，" ~ "代表家目录
比如把文件复制到当前文件夹，并改名
[root@localhost ~]# ls
a.py
[root@localhost ~]# cp a.py b.py
[root@localhost ~]# ls
a.py  b.py
或是从其他文件路径复制到当前路径
[root@localhost b]# tree /root
/root
├── a
│  └── a.py
└── b
 
2 directories, 1 file
[root@localhost b]# cp ../a/a.py .
[root@localhost b]# tree /root
/root
├── a
│  └── a.py
└── b
    └── a.py
 
2 directories, 2 files
当要复制多个文件时或是一个目录时，这时就要用到-r 递归处理
[root@localhost b]# pwd
/root/b
[root@localhost b]# tree /root
/root
├── a
│  ├── 1.py
│  ├── 2.py
│  └── 3.py
├── b
└── c
 
3 directories, 3 files
[root@localhost b]# cp -r ../a .
[root@localhost b]# tree /root
/root
├── a
│  ├── 1.py
│  ├── 2.py
│  └── 3.py
├── b
│  └── a
│      ├── 1.py
│      ├── 2.py
│      └── 3.py
└── c
　　将a整个目录都复制过来了
有时候我们在复制多个文件时，很有可能会遇到有文件重复，这时我们就可以使用 -i 选项交互式进行cp命令，也就是在覆盖既有文件之前先询问用户
[root@localhost b]# tree /root
/root
├── a
│  ├── a.py
│  └── b.py
├── b
│  └── a.py
└── c
 
3 directories, 3 files
[root@localhost b]# cp -i  ../a/a.py .
cp: overwrite `./a.py'? y      #y代表的是yes
如果文件很多时，而且重名文件也很多，那么你就可能需要不停的按y或yes，这样很麻烦
cp aaa/* /bbb
复制目录aaa下所有到/bbb目录下，这时如果/bbb目录下有和aaa同名的文件，需要按Y来确认并且会略过aaa目录下的子目录。
 
cp -r aaa/* /bbb
这次依然需要按Y来确认操作，但是没有忽略子目录。
 
cp -r -a aaa/* /bbb
依然需要按Y来确认操作，并且把aaa目录以及子目录和文件属性也传递到了/bbb。
 
\cp -r -a aaa/* /bbb
成功，没有提示按Y、传递了目录属性、没有略过目录。
```