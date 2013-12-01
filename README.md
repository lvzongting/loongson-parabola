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


    #pacman -r target/ -Sy base --config pacman.conf --arch mips64el


    PMON> load (usb0,0)/boot/vmlinuz-linux-libre
    PMON> g root=/dev/sdb1 rw 

无法执行安装脚本，需要到本地去执行安装脚本，否则启动的时候巨慢无比。

可以考虑通过qmeu chroot进去之后，来执行安装脚本。

qemu-static-user 方法尝试过了，用的是debian的包1.6.0，那里的mips64el chroot进去之后exe format err。我猜想是debian的mips64el是mipsII 而parabola的是mispIII所以才造成这样的问题。

另外我发现用debootstrap 来做这个事情非常顺手，因为有个非常牛的工具叫做qemu-debootstrap 专门做这个事情。

两个方向，从源码编译一下qemu-static-user再试一下，第二个方向通过clfs看看boot之前至少要做什么，比如mknod  /dev 做完之后，等启动进入系统之后在进行二阶段。

第一方向有希望，我找到loongson的qemu的包了，而且我也会编译静态的qemu了。等我尝试一下。如果成功我就发AUR。

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

==========================

debian foreign install
=================

第一步，将存储介质接到另外一个运行linux的主机上，比如x86的笔记本或是x86_64位的台式机等等。

    比如，将小本的8G的电子盘通过ce to sata 接口接到主机上。
    比如，直接将U盘接到主机上。

第二步，将这个存贮介质分区格式化。

    fdisk /dev/sdb  或是 gparted 
    然后格式化。
    mkfs.ext3 /dev/sdb1
    mkfs.ext4 /dev/sdb2

第三步，将格式化好的分区挂载好。

    mount /dev/sdb2 /mnt

第四步，通过qemu-debootstrap 进行foreign install

    sudo apt-get install qemu-user-static
    sudo apt-get install debootstrap
    sudo qemu-debootstrap --arch mipsel sid /mnt http://debian.ustc.edu.cn/debian


    特别的：如果想安装一些特别的包可以用如下命令
    sudo qemu-debootstrap --arch mipsel --include ssh,net-tools sid /mnt http://debian.ustc.edu.cn/debian
    

第五步，挂载boot分区,并将启动文件写好。

    mv /mnt/boot{,.bak}
    mkdir -p /mnt/boot
    mount /dev/sdb1 /mnt/boot
    cp -rf /mnt/boot.bak /mnt/boot
    sync

第六步，卸下分区。

    umount /mnt/boot
    umount /mnt

第七步，将存储介质放回8089D小本。

    最后一步，重启8089D。然后你就进入debian sid了。


