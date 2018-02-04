---
title: 虚拟机VHD瘦身计划
date: 2018-01-22 10:05:54
tags: [技术,虚拟机]
---
因工作需要在Mac上跑了一个VirtualBox虚拟win7，使用对win系统友好的vhd格式作为虚拟硬盘。经过一段时间使用发现vhd占用空间远大于虚拟磁盘使用量，想办法减减肥才行。
<!-- more-->

步骤整理如下：

1. 在 Virtual PC 的安装目录 Virtual Machine Additions 目录里找到 `Virtual Disk Precompactor.iso` 将其中的 `precompact.exe` 解压出来备用。
2. 使用 Windows 7 的磁盘管理挂载（附加） VHD ，然后使用 `precompact.exe` 将不用的空间填充。例如，你的 VHD 挂载为 G、H、I 这三个分区，使用如下参数：`precompact.exe /SetDisks:ghi` 如果不加参数，将连同物理磁盘一同处理，浪费时间。
3. 这样将只对 VHD 中的分区进行处理，完成后回到磁盘管理分离 VHD文件。
4. 最后，打开命令行输入以下命令选择并压缩压缩 VHD。

```
diskpart
select vdisk file="c:\test.vhd"
compact vdisk
```