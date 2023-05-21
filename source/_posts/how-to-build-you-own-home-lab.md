---
title: 簡單在家建構雲端伺服器
date: 2023-03-31 21:40:03
tags:
 - Proxmox
 - Home Lab
 - VM
 - Server
 - Cloud
---

# Proxmox VE

這邊文章會簡單介紹如何(Step-by-step)安裝 Proxmox，想快速入坑的同學可以往下看。
<img width="500px" src="/2023/03/31/how-to-build-you-own-home-lab/proxmox-vm-browser.png" alt="瀏覽器上的 Ubuntu " style="border-radius: 10px;">

## 介紹

Proxmox 是一個開源的伺服器虛擬話環境的 Linux 分支 [Linux distribution](https://en.wikipedia.org/wiki/Linux_distribution)，最大的優點就是提供使用者簡單安裝、Web 控制台、各種 Console(GUI/CLI)。除此之外，他還提供 LXC 和 Virtual Machine 兩種方式建構虛擬機。

## 安裝介紹

<!-- more -->

### 下載 (Download)

[Proxmox 下載頁面](https://www.proxmox.com/en/downloads)
點擊連結 -> Proxmox Virtual Environment -> ISO Image -> 選擇自己需要的 ISO Installer (ISO映像檔)
我這邊選用 Proxmox VE 7.4 ISO Installer

<span style="color:red;">Warning</span>
Proxmox 預設不提供 Desktop GUI<br>
如果想直接安裝成台式機<br>
請先對 CLI (Command Line Interface 有一定的了解)

<img width="500px" src="proxmox-download-page.png" alt="Proxmox Download Page" style="border-radius: 10px;">

<img width="500px" src="proxmox-download-page-iso.png" alt="Proxmox Download Page - ISO Image" style="border-radius: 10px;">

<img width="500px" src="proxmox-download-iso-installer.png" alt="Proxmox Download ISO Installler" style="border-radius: 10px;">

### 準備安裝工具

#### Linux
Linux 使用者可以直接對自己的隨身碟進行映像檔的寫入
```bash
dd bs=1M conv=fdatasync if=./proxmox-ve_*.iso of=/dev/target-device
```

#### Windows User

[rufus](https://rufus.ie/zh_TW/)
Rufus 是個能格式化並製作可開機 USB 快閃磁碟機（USB 隨身碟、Memory Stick 等等）的工具。

備注:
Easy2boot 是沒辦法順利安裝 Proxmox 

### 安裝 (Installation)

1. 對你想要安裝Proxmox的機器插入USB。

2. 當機器正在啟動時，請按下進入系統選單的案件。最常見的按鍵包括Esc、F2、F10、F11 或 F12。

3. 選擇你的 Proxmox Installer USB

4. 選擇安裝 Proxmox VE 選項

<img width="500px" src="proxmox-installer-menu.png" alt="Proxmox 安裝歡迎介面" style="border-radius: 10px;">

5. 同意 EULA 

<img width="500px" src="proxmox-eula.png" alt="EULA 同意書" style="border-radius: 10px;">

6. 硬碟安裝選項 選擇 EXT4 
   若了解自己需求的使用者，可以選擇其他的 FileSystem。

7. 選擇時區 (通常為 Asia/Taipei)

8. 建立密碼 (建議填入一個能記住的密碼，預設 root 使用者就是這個密碼)

9. 設定網路
   - Management Interface (根據裝置不同，顯示名稱可能不同)
     - ens0
     - enp2s0
   - Hostname (FQDN): 有網域的可能就會選擇填入網域，沒有的就隨意填入。
   - IP Address: 使用 Home Router 可能會填入 `192.168.0.150` 根據自己的 Router 設定調整
   - Netmask: 如果沒有多個網段的需求，則填入 `255.255.255.0` 即可。
   - Gateway: 根據自己的環境設定 (例如: 填入 Router IP 192.168.0.1)
   - DNS Server: 根據自己的喜好設定，例如說 Google: 8.8.8.8, CloudFlare: 1.1.1.1 等等

10. 打開 WebUI (https://IP-Address:8006)

<img width="500px" src="proxmox-web-ui.png" alt="WebUI" style="border-radius: 10px;">

11. 登入

使用 Step 8 的密碼

<img width="500px" src="proxmox-login.png" alt="WebUI" style="border-radius: 10px;">

# 展示

## Ubuntu VM

我也在上面順利安裝了一個 Ubuntu 
還可以用 Browser 打開你的 OS

<img width="500px" src="proxmox-vm-browser.png" alt="Browser VM" style="border-radius: 10px;">

下次再來介紹如何搭建 VM 或 LXC。
