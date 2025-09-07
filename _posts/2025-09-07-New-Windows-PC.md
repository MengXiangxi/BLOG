---
title: "The checklist for setting up a new Windows PC"
categories:
- Coding
tags:
- cheat sheet
date: 2025-09-07
layout: post
---

When setting up a new Windows PC, I usually follow the checklist below. It is mainly for my own reference, but you may find it useful as well.

- Install necessary updates
- Add network destinations
- Move Downloads to D:/, Remove shortcuts to Music and Pictures; Remove startup fixed icons.
- **Clash Verge**, sync via webdav
- Download **Firefox** via Edge, log in, set as default, fix to task bar
- **Bitwarden**
- **Everything**
Exclude:
```text
%LOCALAPPDATA%/QNAP
%APPDATA%/Microsoft
```
- **Logitech Logi Options +**, set up mouse
- **Joplin**, set up WebDAV sync. Install addons: Journal; Outline
- **Tailscale**, install and log in
- **Qsync**, establish links, excldue the following
```text
*.lnk
*.rdp
ShareX.url
*\.venv\*
*\node_modules\*
```
- Activate Windows
- Windows settings
	- Power: No sleep
	- Desktop icons
	- Link bluetooth earphone
	- Enable clipboard history (Winkey+V)
	- Developer Options
		- Show file extension, show full path
		- Enable remote desktop
		- Set Windows Terminal as the default terminal.
		- Use sudo
	- Add Scan Utility
	- Add Windows function: Hyper-V
	- Add Printer, setup default (double side)
- Remove Edge from the startup
- Uninstall OneDrive
- **VSCode**, log in, and sync
- **Rime**, install, set up sync (as in the BLOG post)
- Add "AutoLock.exe" to startup (`shell:startup` from Run command)
- **Thunderbird** (Edit `profiles.ini`, and use the `-p` flag to start Thunderbird)
- **Wechat**, set the file location
- **Office**, add personal templates
- **7zip**
- **AnyDesk**, set up stand-alone password, update address
- **Sunlogin**, switch to long-term password, update address
- **Fiddler Classic**, exempt all Windows Apps.
- **CAS Preview** via Microsoft Store
- Log in to **Microsoft To Do**, pin to task bar
- **Sumutra PDF**
- **Tencent Meeting**
- **Cherry Studio**, retrive from WebDAV
- **TeXLive**, can be installed via mirror
- **MobaXterm**
- **Git for Windows**
- **GitHub Desktop**
- Renew the ssh key. Set the ssh key in GitHub.
- **Anaconda**
- **MATLAB**, add custom packages to the path variable
- **Origin**, set the User File Folder to the sync folder.
- **Zotero**, log in, set sync with WebDAV, cancel sync with Zotero Cloud.
- **Calibre**
- **R**
- **RStudio**
- Import Hyper-V VM. Set up router and shared folder.
- **MicroDicom**
- **Tencent Docs**
- **Adobe Acrobat**
- **Adobe Photoshop**
- **Adobe Illustrator**
- **Fiji ImageJ**
- **monolixSuite**
- **Feishu**
