刚接触磁盘加密方面的知识，知之甚少。由于工作中还是有这方面的涉及，就有了一边查找资料外加向资深同事求助之后形成了此篇学习心得。  

这里把我自己认为的关键实现要点和一些经历的出错点写出来。  

这里的磁盘加密使用的dm-crypt模组，应该是Linux内核的，这是后面需要继续查阅的。  

说下整个思路简要，   

1. 使用一个sd card。 把它分成三个分区，也就是mmcblk1p1, mmcblk1p2 和 mmcblk1p3.   
2. 需要一个预编译的嵌入式系统（这里使用Yocto）， 当然也可以直接使用后面会提到的用于存储加密的ROOTFS系统。
3. 编译一个initramfs系统
4. 编译加载了第三点提到的initramfs的内核(Kernel)

1. 磁盘分区
应用于这里的分区方式， 我能想到的有两种，     
第一：直接把预编译的系统使用uuu工具烧写到sd card. 这样默认就有两个分区。第一分区会保存了bootloader, kernel和dtb等。第二个分区是rootfs，第三个分区在一台x86的linux系统上，使用fdisk工具在原有的2个分区后新建分区。
第二：先手动把sd card分成三个分区， 然后依次把各个镜像烧写到对应的分区。    

mmcblk1p1会包含镜像的bootloader, kernel, 和 dts。要注意的是，这里会重新编译kernel，重新编译的kernel会包含一些安全模组，initramfs,    
和自动化导入key、挂载device mapper等的脚本。   

mmcblk1p2作为预编译的rootfs， 我们可以在启动这个rootfs系统后使用camm-keygen生成key, keyblob. 只要需要手动复制到mmcblk1p1分区供导入到解密磁盘使用。  

mmcblk1p3就是我们所要加密的磁盘，也就是device mapper所映射的加密磁盘。我们的接下来使用yocto编译的只读的、支持ext4的rootfs被加载到这个磁盘分区。    

2. 预编译系统  

像在思路简要说的，我直接使用作为加密磁盘的系统作为预编译系统。  
因为是使用Yocto编译，那么在一个core-image-minimal.bb菜谱里(也可以使用其他的image菜谱，这个比较小，编译所花时间较短)需要主要添加一些关键功能，比如所支持的文件系统、安全模组等。   
   ```bash
  EXTRA_IMAGECMD_ext4 += " -b 4096 "
  #EXTRA_IMAGE_FEATURES += "read-only-rootfs"
  CORE_IMAGE_EXTRA_INSTALL += " cryptsetup cryptodev-module packagegroup-imx-security"
  IMAGE_FSTYPES += "ext4"
   ```
 系统编译后，使用uuu工具烧写到sd card，然后启动。 这之后需要去生成black key, black blob, 创建device mapper, 挂载device mapper到加密磁盘等。  
 
3. 编译initramfs   
 
还是使用Yocto编译内核内置ram文件系统。在一个layer的image目录下创建菜谱：dm-crypt-initramfs.bb。在这个菜谱关键需要编译进packages大概如下：
```bash
PACKAGE_INSTALL += " \
    ${INITRAMFS_SCRIPTS} \
    ${VIRTUAL-RUNTIME_base-utils} \
    udev \
    ${ROOTFS_BOOTSTRAP_INSTALL} \
    cryptsetup \
    cryptodev-module \
    keyctl-caam \
    keyutils \
    lvm2 \
    util-linux-blockdev \
"
export IMAGE_BASENAME = "crypttest-initramfs"
#IMAGE_NAME_SUFFIX ?= ""
IMAGE_LINGUAS = ""
IMAGE_FSTYPES = "${INITRAMFS_FSTYPES}"
inherit core-image
IMAGE_ROOTFS_SIZE = "8192"
IMAGE_ROOTFS_EXTRA_SPACE = "0"
IMAGE_FSTYPES += "tar.bz2"
```
4. 启动脚本  

因为这里black key是session key, 启动脚本是解决手动再次加载black key、生成device mapper、挂载等操作。脚本的主要bash命令也就是那些需要的再次操作命令。当然一定是要把加密rootfs放在加密分区的：
** bootparam_root="/mnt/encrypted" **

5. 编译内核  

这里编译内核，主要是需要前面的initramfs添加到内核里、启动脚本、以及以内核的方式启用一些安全驱动。

## 我的一些出错点  

key, keyblob需要手动复制到第一个磁盘分区，否则启动脚本是无法import到black key.  

关于脚本的格式，不是太清楚什么原因，总之我的这个脚本格式不对还是什么的。在系统加载到kernel的过程中，总是报各种错误，导致系统无法启动。
错误信息：
```bash
[  508.723359] device-mapper: table: 252:0: crypt: unknown target type
[  508.730351] device-mapper: ioctl: error adding target to table
device-mapper: reload ioctl on encrypted2 (252:0) failed: Invalid argument
Command failed.
```
还有就是安全驱动编译到内核的时候。在运行了编译内的defconfig后，crypt target support这项被默认成模组模式，是由于其他依赖所导致。这里花了不少时间才把其他依赖配置成内核模式。  

大致的就这些，是一个蛮历练的过程。








