loongson-parabola
=================

parabola for loongson some note

Foreign Install
交叉安装法  

pacman.conf

    [libre]
    Include = mirrorlist

mirror list

    Server =http://parabolaweb.eu/$repo/os/$arch


In i686 or x86_64 

    newroot=target
    mkdir -m 0755 -p "$newroot"/var/{cache/pacman/pkg,lib/pacman,log} "$newroot"/{dev,run,etc}
    mkdir -m 1777 -p "$newroot"/tmp
    mkdir -m 0555 -p "$newroot"/{sys,proc}


    #pacman -r target/ -Sy base --config pacman.conf --arch mips64e


    PMON> load (usb0,0)/boot/vmlinuz-linux-libre
    PMON> g root=/dev/sdb1 rw 

无法执行安装脚本，需要到本地去执行安装脚本，否则启动的时候巨慢无比。

可以考虑通过qmeu chroot进去之后，来执行安装脚本。

qemu-static-user 方法尝试过了，用的是debian的包1.6.0，那里的mips64el chroot进去之后exe format err。我猜想是debian的mips64el是mipsII 而parabola的是mispIII所以才造成这样的问题。

另外我发现用debootstrap 来做这个事情非常顺手，因为有个非常牛的工具叫做qemu-debootstrap 专门做这个事情。

两个方向，从源码编译一下qemu-static-user再试一下，第二个方向通过clfs看看boot之前至少要做什么，比如mknod  /dev 做完之后，等启动进入系统之后在进行二阶段。

Lazy Install
偷懒式安装法

第一步：
    在U盘上要有一个救援系统，这里比较推荐gentoo的救援系统。
    或者
    把硬盘拆下来连到其他有系统的计算机上

第二步：
    分区并格式化硬盘，去百度网盘下载最新的base tarbar ，解压到你的硬盘。
    
     网盘地址 ： http://pan.baidu.com/s/1xgDNM 
     
    一个更新的，我自己生成的base tarbar ，在这个网盘地址，生成于2013-11-21
     
     网盘地址 ： http://pan.baidu.com/s/1cOaj5
    
第三步：
    把硬盘放回去，然后重启。


PS：Lazy Install这个方法没仔细写，就是个思路，因为百度盘上的那个tarbar有些老，我还更新。而且我想把第一个先弄好，第二个很简单估计各位都能看懂。


