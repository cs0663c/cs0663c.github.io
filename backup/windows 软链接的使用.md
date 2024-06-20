# windows 软链接的使用

mklink /j "E:\surveillance" "d:\i34160\软件包\"
mklink /j "E:\要显示的位置" "d:\文件存在的真实位置" #前面的不能有现成的文件夹
#例如
1.复制User⽬录到D盘： robocopy “C:\Users” “D:\Users” /E /COPYALL /XJ
2.强制删除User⽬录： rmdir “C:\Users” /S /Q
3.创建C盘下的User的软件链接，链接到D盘User⽬录：mklink /J “C:\Users” “D:\Users”
#说明 mklink /?
MKLINK [[/D] | [/H] | [/J]] Link Target
        /D      创建目录符号链接。默认为文件符号链接。
        /H      创建硬链接而非符号链接。
        /J       创建目录联接。
       Link     指定新的符号链接名称。
       Target  指定新链接引用的路径 (相对或绝对)。
#挂载网络硬盘到目录
管理员 cmd
进到要显示的位置输入命令 
e:
cd E:\DS_HDD
mklink /D Syncthing \\DSM\Syncthing 或
mklink /D Syncthing \\DSM701\Syncthing

建父级目录  进入到要连接的目录<==>后面是远程目录