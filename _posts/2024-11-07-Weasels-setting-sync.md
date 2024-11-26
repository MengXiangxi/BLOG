---
title: "Windows上小狼毫输入法的同步设定"
categories:
- Coding
tags:
- solution
- Chinese
- cheat sheet
date: 2024-11-07
layout: post
---

小狼毫输入法是[中州韵](https://rime.im/)输入法引擎的官方Windows实现，由程序员[佛振](https://github.com/lotem)开发。这是一个中用的开源输入法，是我中文输入的首选。

目前，网络上有一些用户资料同步的实践经验。我的方法与之相异，主要特点是利用任务计划程序（Task Scheduler）自动执行同步操作，及用PowerShell脚本运行`\sync`命令以利用通配符特性避免版本更改带来的路径变化。

我的同步方式是自建NAS。利用可以进行云同步的其他服务也都一样。

- 安装小狼毫。

- 在磁盘上新建一个用于同步的文件夹，如`D:/Sync/Miscellaneous/rime_sync`。Rime将在这个文件夹下自动新建若干子目录，每个子目录对应一台设备。需要保证这一文件夹保持同步。

- 打开“【小狼毫】用户文件夹”，编辑其中的`installation.yaml`文件。添加两行：

```txt
installation_id: "##Computer_ID##"
sync_dir: "D:/Sync/Miscellaneous/rime_sync"
```

其中，`##Computer_ID##`为这台机器的名称，需要自定义。

- 新建一个PowerShell脚本，内容如下：

```powershell
C:\"Program F"*\Rime\weasel-*.*.*\WeaselDeployer.exe /sync
```

其中通配符是为了适配不同的版本，包括有的默认安装路径会在`Program Files`而非`Program Files (x86)`的64-bit系统。将其命名为`sync_rime.ps1`，可保存在`D:\Library\Scripts\`之类的目录下。可以在不同Windows计算机上对这一脚本进行同步，因为它具有一定普适性。

- 在任务计划程序中新建一个任务。

```txt
名称：Sync RIME
安全选项：不管用户是否登录都要运行
触发器1：每日（在每天的4:00）
触发器2：工作站锁定时（当锁定任何用户的工作站时）
操作：启动程序
程序或脚本：powershell.exe
添加参数：-WindowStyle Hidden -File D:\Library\Scripts\sync_rime.ps1
```

其中，参数`-WindowStyle Hidden`保证运行脚本时不会将命令窗口调到前台，但部分机器上会失效，目前原因不明。

注意，Windows客户端默认进制运行PowerShell脚本，可以参考[这里](../../../../coding/2021/09/04/Life-hack.html#PowerShell)解除限制。
