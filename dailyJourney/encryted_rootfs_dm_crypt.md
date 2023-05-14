刚接触磁盘加密方面的知识，知之甚少。由于工作中还是有这方面的涉及，就有了一边查找资料外加向资深同事求助之后形成了此篇学习心得。  

估计很多细节知识点会在后面继续补充，这里先把我认为的一些关键点写出来。  

这里的磁盘加密使用的dm-crypt模组，应该是Linux内核的，这是后面需要继续查阅的。  

说下整个思路， 使用一个sd card。 把它分成三个分区，也就是mmcblk1p1, mmcblk1p2 和 mmcblk1p3.   

mmcblk1p1会包含镜像的bootloader, kernel, 和 dts。要注意的是，这里会重新编译kernel，重新编译的kernel会包含一些安全模组，initramfs,  
和自动化导入key、挂载device mapper等的脚本。   
mmcblk1p2作为预编译的rootfs， 我们可以在启动这个rootfs系统后使用camm-keygen生成key, keyblob. 只要需要手动复制到mmcblk1p1分区供导入到解密磁盘使用。  
mmcblk1p3就是我们所要加密的磁盘，也就是device mapper所映射的加密磁盘。我们的接下来使用yocto编译的只读的、支持ext4的rootfs被加载到这个磁盘分区。  






