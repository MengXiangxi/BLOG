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

> **Updated 20260102** Migirating to various softwares 

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
- **Obsidian**, WebDAV sync up with `Remotely Sync` addon.
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
- [Uninstall OneDrive](https://support.microsoft.com/en-us/office/turn-off-disable-or-uninstall-onedrive-f32a17ce-3336-40fe-9c38-6efb09f944b0)
- **VSCode**, log in, and sync
- **Rime**, install, set up sync (as in the BLOG post)
- Add "AutoLock.exe" to startup (`shell:startup` from Run command)
- **Thunderbird** (Edit `profiles.ini`, and use the `-p` flag to start Thunderbird)
- **Wechat**, set the file location
- **Office**, add personal templates
- **7zip** (Not really necessary with Windows 10, or can be replaced with [`NanaZip`](https://github.com/M2Team/NanaZip))
- **AnyDesk**, set up stand-alone password, update address
- **Sunlogin**, switch to long-term password, update address ==> **AweSunRemote**
- <del>**Fiddler Classic**, exempt all Windows Apps.</del> Now integrated in `Clash Verge`.
- **CAS Preview** via Microsoft Store
- <del>Log in to **Microsoft To Do**, pin to task bar </del>
- **Sumutra PDF**
- **Tencent Meeting**
- **Cherry Studio**, retrive from WebDAV
- **TeXLive**, can be installed via mirror
- <del>**MobaXterm**</del> **Tabby**, WebDAV sync using `Settings Sync` plugin.
- **Git for Windows**
- **GitHub Desktop**
- Renew the ssh key. Set the ssh key in GitHub.
- <del>**Anaconda**</del> **MiniConda**
- **MATLAB**, add custom packages to the path variable
- **Origin**, set the User File Folder to the sync folder.
- **Zotero**, log in, set sync with WebDAV, cancel sync with Zotero Cloud.
- **Calibre**
- **R**
- **RStudio**
- Import Hyper-V VM. Set up router and shared folder.
- **MicroDicom**
- **3D Slicer**
- **ITK-SNAP**
- **Tencent Docs**
- **Adobe Acrobat**
- **Adobe Photoshop**
- **Adobe Illustrator**
- **Fiji ImageJ**
- <del>**monolixSuite**</del>
- **Feishu**
