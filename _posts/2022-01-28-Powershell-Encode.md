---
title: "Windows 10 下 Powershell 无法使用 conda 命令及中文乱码问题"
categories:
  - Coding
tags:
  - Chinese
  - solution
date: 2022-01-28
layout: post
---

In this post, I am going to explain how I failed to use the `conda` command on a machine with Chinese version of Windows 10, even though I have added the relevant directory to the PATH variable. The problem lies in the Chinese characters in the default path for the `Profile.ps1` file. The problem is solved by copying the faulty profile file to the correct destination, and specifying the encoding.

本文中，我们将介绍中文版 Windows 10 中 Powershell 无法使用 `conda` 命令的问题（尽管已经把相关路径加入PATH变量）。其原因是，默认的 `Profile.ps1` 文件路径中含有中文。只需要把 `Profile.ps1` 拷贝到正确位置并设定编码，问题就得以解决。

## 问题

Powershell 无法使用 `conda` 命令。具体表现：

```powershell
PS C:\Users\UserName> conda activate base

CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
If using 'conda activate' from a batch script, change your
invocation to 'CALL conda.bat activate'.

To initialize your shell, run

    $ conda init <SHELL_NAME>

Currently supported shells are:
  - bash
  - cmd.exe
  - fish
  - tcsh
  - xonsh
  - zsh
  - powershell

See 'conda init --help' for more information and options.

IMPORTANT: You may need to close and restart your shell after running 'conda init'.
```

此时运行 `conda init powershell`，效果如下：

```powershell
PS C:\Users\UserName> conda init powershell
no change     C:\ProgramData\Anaconda3\Scripts\conda.exe
no change     C:\ProgramData\Anaconda3\Scripts\conda-env.exe
no change     C:\ProgramData\Anaconda3\Scripts\conda-script.py
no change     C:\ProgramData\Anaconda3\Scripts\conda-env-script.py
no change     C:\ProgramData\Anaconda3\condabin\conda.bat
no change     C:\ProgramData\Anaconda3\Library\bin\conda.bat
no change     C:\ProgramData\Anaconda3\condabin\_conda_activate.bat
no change     C:\ProgramData\Anaconda3\condabin\rename_tmp.bat
no change     C:\ProgramData\Anaconda3\condabin\conda_auto_activate.bat
no change     C:\ProgramData\Anaconda3\condabin\conda_hook.bat
no change     C:\ProgramData\Anaconda3\Scripts\activate.bat
no change     C:\ProgramData\Anaconda3\condabin\activate.bat
no change     C:\ProgramData\Anaconda3\condabin\deactivate.bat
no change     C:\ProgramData\Anaconda3\Scripts\activate
no change     C:\ProgramData\Anaconda3\Scripts\deactivate
no change     C:\ProgramData\Anaconda3\etc\profile.d\conda.sh
no change     C:\ProgramData\Anaconda3\etc\fish\conf.d\conda.fish
no change     C:\ProgramData\Anaconda3\shell\condabin\Conda.psm1
no change     C:\ProgramData\Anaconda3\shell\condabin\conda-hook.ps1
no change     C:\ProgramData\Anaconda3\Lib\site-packages\xontrib\conda.xsh
no change     C:\ProgramData\Anaconda3\etc\profile.d\conda.csh
no change     D:\%一些乱码%\Documents\WindowsPowerShell\profile.ps1
No action taken.
```

因为 `conda init` 命令没有执行任何改动，所以此时再运行 `conda activate` 也没啥用处。

## 分析

观察上面的输出结果，我们发现 `Profile.ps1` 文件的位置不太对劲。根据[文档](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles)，profile 文件的位置有赖于系统变量中的设定。如果系统变量设定的路径中存在中文字符，估计可能会有 bug。

我为了备份文件，曾一度把`我的文档`设定到了一个含有中文字符的目录。事实上，在其他中文版操作系统中，`我的文档`也很容易包含中文字符。因此，默认的 `Profile.ps1` 文件位置就会产生乱码。我们看一下D盘根目录，果然有一个带乱码的文件夹。

```Powershell
PS C:\Users\UserName> ls D:\


    目录: D:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
...省略...
d-----         2022/1/27     14:00                %一些乱码%                                                 
...省略...
```

而目前的 `Profile.ps1` 文件就在里面。`conda init` 命令修改了这里面的文件后，Powershell 也并找不到它，于是永远都无法产生效果。

## 解决方案

首先，找到 Profile 文件的正确位置。

```powershell
PS C:\Users\UserName> $Profile
D:\%一些正常中文%\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```

那么，我们首先要把那个乱码文件夹下面的 `D:\%一些乱码%\Documents\WindowsPowerShell\profile.ps1` 文件，拷贝到正确的位置，即 `D:\%一些正常中文%\Documents\WindowsPowerShell\profile.ps1`。如果目录不存在，就创建出来。然后编辑这个文件，在最后增加一行编码的命令：`chcp 936`。加完之后的效果如下：

```powershell
#region conda initialize
# !! Contents within this block are managed by 'conda init' !!
(& "C:\ProgramData\Anaconda3\Scripts\conda.exe" "shell.powershell" "hook") | Out-String | Invoke-Expression
#endregion
chcp 936
```

然后就可以在 Powershell 里面愉快地使用 `conda` 命令了！

## 其它

在我的实践中，遇到了执行权限不足（`Execution Policy Restricted`）的情况。需要提升执行权限。具体做法请参考[文档](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies)。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```
