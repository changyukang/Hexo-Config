title: VMware空间压缩、Snapshot处理注意点和磁盘文件不正确的解决方法
layout: post
comment: true
date: 2015-07-07 12:31:27
categories: VMware
tags: [VMware,snapshot]
keywords: vmware,snapshot
description: <div class="note info"><p>VMware disk space compression.</p></div>
---
### 问题一、空间压缩

压缩空间时需要保证磁盘空间足够，在往磁盘写`0`时也要保证宿主机磁盘空间足够，不然会写失败，在压缩时还可能会出现**无效的参数**问题。

### 问题二、快照删除

快照会占用相当大的磁盘空间，在删除快照时也要保证空间足够，不然快照删除会失败。

本次出现问题就是虚拟机中磁盘全部填充了`0`，然后空间剩余不多，再删除快照，导致快照删除失败，再进行磁盘压缩，出现参数无效的问题的。

### 问题三、加载虚拟机，磁盘文件不正确

加载哪个作为磁盘文件是记录在`.vmx`文件中的，例如：
> scsi0:0.fileName = "karchlinux.vmdk"

如果磁盘文件不正确，可以在这里修正，一般磁盘文件为`.vmdk`文件：
> karchlinux**.vmdk**
