---
title: "Tricks in maintenance of coding environments"
categories:
  - Coding
tags:
  - cheat sheet
date: 2021-08-31
layout: post
excerpt: Various toolchains have been set up to facilitate daily chores. However, setting up and maintaining the environment becomes troublesome. Here, some common commands and methods are summarized.
---

Various toolchains have been set up to facilitate daily chores. However, setting up and maintaining the environment becomes troublesome. Here, some common commands and methods are summarized.

## Ubuntu

- Change the apt-get environment to aliyun. First back up the original list.

```shell
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

Then, edit `/etc/apt/sources.list`. The content of the file can be found in the [aliyun homepage](https://developer.aliyun.com/mirror/ubuntu). Finally, run

```shell
sudo apt-get update
```

- Grant user permission with `sudo chown`\
It is used to temporarily grant a super user permission.

```shell
sudo chrown -R $USER [FOLDER]
```

- Monitoring system resources, a better way: [bashtop](https://github.com/aristocratos/bashtop)

- GPU monitoring: `nvidia-smi`

- Add to user `$PATH`

```shell
sudo nano ~/.bashrc
```

Edit, and add the following line to `~/.bashrc`

```text
export PATH=$PATH:/some/directory
```

Then,

```shell
source ~/.bashrc
echo $PATH
```

Then the directory added should be there in the `$PATH` variable.

- Mount a shared drive (samba)

```shell
sudo mount -t cifs //0.0.0.0/dir_name /mnt/destin -o username=$NAME
```

## WSL

> Always use WSL2

- Check current version

```shell
wsl.exe -l -v
lsb_release -a
```

- Show remote (WSL) directory in windows explorer

```shell
explorer.exe .
```

## Recommended docker image

Thanks to Hongjia Liu from the Department of Radiotherapy.

- Basic data science environment

```shell
sudo docker run --name [DOCKER_NAME] -p 10000:8888 -e JUPYTER_ENABLE_LAB=yes -v [LOCAL_DIR:~/docker]:/home/jovyan/work jupyter/datascience-notebook:latest
```

- GPU-enabled, advanced data science environment\
An environment integrating `zsh`, `tensor-flow`, `pytorch`, and etc.

```shell
sudo docker run -d --name <CONTAINER_NAME> -p <LOCAL_PORT>:8080 --gpus all -v "<LOCAL_DIR>:/workspace" --shm-size 10240m --env AUTHENTICATE_VIA_JUPYTER="<TOKEN>" mltooling/ml-workspace-gpu:0.13.2
```

`<TOKEN>` is a paraphrase to assess the container. The CUDA version of the host server must be CUDA-11.2. The host server shall not be an LXC virtual machine.

## Jupyter Notebook

- To set breakpoint

```python
from IPython.core.debugger import set_trace

set_trace()
```

- A workaround to skip a certain cell when running the notebook

```python
%%script skip-cell
```

- To use a conda environment as a kernel

```bash
conda info --envs
conda activate myenv
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"

# To uninstall
jupyter kernelspec list
jupyter kernelspec uninstall myenv
```

## Python

- Useful pip mirrors

Recommended: Peking University mirror
```bash
pip install pip -U -i https://mirrors.pku.edu.cn/pypi/web/simple # 首先将pip版本升级至10.0.0+
pip config set global.index-url https://mirrors.pku.edu.cn/pypi/web/simple
```
Other mirrors
```bash
# 清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
# 阿里源
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
# 腾讯源
pip config set global.index-url http://mirrors.cloud.tencent.com/pypi/simple
# 豆瓣源
pip config set global.index-url http://pypi.douban.com/simple/
```

## Git

- Proxy
```bash
git config --global https.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
git config --global --unset http.proxy
git config --global --unset https.proxy
```

- SSH key

In git bash

```git
ls -al ~/.ssh
ssh-keygen -t ed25519 -C "your_email@example.com"
ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```

- Change remote URI

Github recently ask clients to use ssh everywhere. It is worthwhile to switch from HTTPS to SSH.

```git
git remote -v
# View existing remote

git remote set-url origin git://new_uri.git
# Change

git remote -v
# Validate
```

- Repair lost `.git` folder (as in [this](https://www.reddit.com/r/git/comments/1mhus5/i_lost_my_git_folder_i_have_a_copy_of_the/) post)

```git
# Create a new .git
git init
# Create objects for local files
git checkout -b temp
git add .
git commit -m 'importing local files'
# Configure your remote
git remote add origin git@xxx.com:xxx/xxx.git
# Now, have it grab the rest of the objects from the remote
git fetch --all
# And switch back to it:
git checkout remotes/origin/main
git checkout -b main
# And, if you didn't have any un-pushed changes:
# delete the local branch
git branch -D temp
```

## R

- To update R on Windows

```R
install.packages("installr") # if not previously installed
library(installr)
updateR()
```

## Windows Terminal

- Add an admin PowerShell profile

Command line:

```text
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -Command Start-Process -Verb RunAs "wt"
```

Starting directory: `Use parent process directory`

## Powershell

- Allowing local script. The following commands need to be run with the admin permission.

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
# OR
Set-ExecutionPolicy -ExecutionPolicy Bypass
```
