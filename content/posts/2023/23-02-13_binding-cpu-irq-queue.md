---
title: "理解 CPU、中断、队列、进程的绑定关系"
subtitle: ""
date: 2023-02-13T12:27:10+08:00
lastmod: 2023-02-13T12:27:10+08:00
draft: false
author: "Xinyu Yang"
authorLink: ""

keywords: ["irq", "队列", "NIC"]
description: ""
images: []

tags: ["arch"]
categories: ["record"]

hiddenFromHomePage: false
hiddenFromSearch: false
featuredImage: ""
featuredImagePreview: ""

toc:
  enable: false
math:
  enable: false
comment:
  enable: true
lightgallery: false

license: ""
---

最近在学习网络性能的测试，其中涉及到 CPU、中断、队列以及进程的映射关系。因为所涉及的元素比较多，所以对于他们的映射关系感到迷迷糊糊，产生了一大堆问题，比如：中断和队列是一对一吗？CPU 和队列是一对一吗？怎么查看和修改他们的映射关系？

为了解决以上问题，笔者查阅了很多文档资料。为了加深理解并且防止遗忘，决定将自己的理解记录下来。

<!--more-->

首先回答第一个问题：每一个队列有一个与之对应的中断号，然后可以使用 `Message Signaled Interrupts (MSI-X)` 来将中断路由给指定的 CPU。队列与中断之间的映射可以从 `/proc/interrupts` 获取，下面是一个 `8` 核 CPU 的例子：

```
						CPU0       CPU1       CPU2       CPU3       CPU4       CPU5       CPU6       CPU7       
   0:          6          0          0          0          0          0          0          0   IO-APIC    2-edge      timer
   1:          0          4          0          0          0          0          0          0   IO-APIC    1-edge      i8042
   8:          0          0          1          0          0          0          0          0   IO-APIC    8-edge      rtc0
   9:          0          0          0          0          0          0          0          0   IO-APIC    9-fasteoi   acpi
  12:          6          0          0          0          0          0          0          0   IO-APIC   12-edge      i8042
  16:          0          0          0          0          0          0          0          0   IO-APIC   16-fasteoi   i801_smbus
  17:          0          0          0          0          0          0          0        151   IO-APIC   17-fasteoi   snd_hda_intel:card1
 125:          0          0          0          0          0          0          0          0   PCI-MSI 327680-edge      xhci_hcd
 126:          0          0          0          0      11596          0    5250359          0   PCI-MSI 376832-edge      ahci[0000:00:17.0]
 127:        765          0          0          0          0          0          0   70831230   PCI-MSI 3145728-edge      enp6s0
 128:          0          0          0          0          0       4285          0          0   PCI-MSI 360448-edge      mei_me
 129:          0          0          0          0          0          0        699          0   PCI-MSI 514048-edge      snd_hda_intel:card0
 NMI:       2784       2802       2778       2755       2605       2570       2615       2658   Non-maskable interrupts
 LOC:  530160796  528154099  531420711  523368187  526087908  508786297  499113075  494567411   Local timer interrupts
 SPU:          0          0          0          0          0          0          0          0   Spurious interrupts
```

可以看出网卡 `enp6s0` 的队列所对应的中断号为 `127`。除此之外，还可以看出，该中断几乎都是由 `CPU7` 进行处理的。那么该中断跟 `CPU7` 有什么关系呢？

为了回答该问题，我们可以查看该中断对应的绑定信息，该信息可以用 `cat /proc/irq/127/smp_affinity` 命令获取 (可参考 [SMP IRQ affinity — The Linux Kernel documentation](https://www.kernel.org/doc/html/latest/core-api/irq/irq-affinity.html))。结果为：

```bash
$ cat /proc/irq/127/smp_affinity
80
```

`80` 是一个十六进制的 `bitmap`，转换成二进制就是 `0b 1000 0000` ，从右往左数，第 `8` 个位置为 `1`，所表示的意思就是：该中断（`127`）被绑定到第 `8` 个 CPU （`CPU7`）。

如果要更改该绑定关系，直接将相应的数字写到该文件中即可。比如我们想将其绑定到 `CPU0-3`：

```bash
$ sudo su
# echo 0f > /proc/irq/127/smp_affinity

or

$ sudo sh -c 'echo 0f > /proc/irq/127/smp_affinity'
```

这里需要注意的是，无法直接使用 `sudo`，需要先切换为 `root` 用户，才能用此操作，或者将整个命令当成脚本（参考 [linux - How do I use sudo to redirect output to a location I don't have permission to write to? - Stack Overflow](https://stackoverflow.com/questions/82256/how-do-i-use-sudo-to-redirect-output-to-a-location-i-dont-have-permission-to-wr)）。

虽然理论上我们已经将网卡对应的中断绑定到 `CPU0-3`，但是笔者发现，在实际使用中（`Ubuntu 20.04`），中断默认被序列号最小的 CPU 处理，也不知道是什么原因。

通过以上操作，我们也回答了第二、三个问题，CPU 和队列不是一对一的关系，一个 CPU 可以对应多个队列，同时一个队列也可以配置多个 CPU。关于更改如何修改绑定，上文已经详述。

## 参考文献

1. [Scaling in the Linux Networking Stack — The Linux Kernel documentation](https://www.kernel.org/doc/html/latest/networking/scaling.html)
