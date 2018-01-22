---
layout: post
title: Compact VHD file
date: 2018-01-22 10:05:54
tags: VM
comments: true
---
# 如何压缩虚拟机VHD文件至实际容量

1.在 Virtual PC 的安装目录 Virtual Machine Additions 目录里找到 `Virtual Disk Precompactor.iso` 

将其中的 `precompact.exe` 解压出来备用。

2.使用 Windows 7 的磁盘管理挂载（附加） VHD ，然后使用 `precompact.exe` 将不用的空间填充。

例如，你的 VHD 中包含 G、H、I 这三个分区，，可以使用如下参数：

```
precompact.exe /SetDisks:ghi
```

这样将只对 VHD 中的分区进行处理。如果不加参数，将连同物理磁盘一同处理，浪费时间。

3.回到磁盘管理分离 VHD。

4.最后，打开命令行，输入以下命令选择并压缩压缩 VHD。
```
diskpart
select vdisk file="c:\test.vhd"
compact vdisk
```