---
title: Arch Linux 初步安裝
published: 2025-03-28
description: 'Arch Llinux 安裝記錄'
image: './cover.png'
tags: [Arch, 維護更新]
category: 'Linux'
draft: false 
lang: ''
---
## 環境

- 硬體：Lenovo T480S /1TB SSD NVM/24G ram/
- 網路：中華雙向 500m
- 2GB 以上的隨身碟

## 安裝工具

- 燒錄工具Rufus：[連結](https://rufus.ie/zh_TW/)
- Arch Linux 安裝映像檔：[下載頁面](https://archlinux.org/download/)

### 1. 燒錄映像檔

1. 下載好 Arch Linux 映像檔與 Rufus 後，開啟 Rufus 選擇 Arch Linux ISO 檔
![[Arch-Linux-setup.png]]
1. 點執行後會跳出<u>偵測到 ISOHybrid 映像檔</u>，點擊<u>預設的「ISO 映像模式寫入」</u>

### 2. 主機 BIOS 確認 UEFI 啟動

### 3. 主機 BIOS Secure Boot  關閉

---

## 安裝 Archinstall 前置動作

下載並製做好安裝碟後，在 Archinstall 之前需要照著以下 YT 影片前半段的設定，不然會出問題。
<https://www.youtube.com/watch?v=AYxaNjbC1wg&t=5s>

### 1. 測試有無網路

網路有通，測 ping 出去會有回應

```bash
ping google.com
```

> [!WARNING]
> 如果沒有網路
>
> 1. 輸入`iwctl`，查看 NetworkConfigurationEnabled 的狀態
> 2. 輸入 device list 查看 Devices 中 Address 欄位是否有網路位址、還有網卡 Name(假設為 wlan0)
> 3. 如果是無線網路卡：輸入`device wlan0 show`查看 wlan0 介面(假設 Mode 為 station)
> 4. 輸入 `station wlan0 get-networks`列出目前環境有的無線網路
> 5. 輸入`station wlan0 connect 網路名稱`，輸入網路密碼
> 6. 輸入`exit`
> 7. `ping google.com` 成功會回覆 XXX bytes from ....... time=XX ms
> 8. Ctrl+C 中斷網路測試
> 9. 完成網路卡的工作
> 10. 指令`clear`可以清除畫面

📌pacman -Sy 指令是什麼?[^1]

### 2. 更新同步包裝資料庫

```bash
pacman -Syy
```

### 3. 磁碟分割

```bash
lsblk
```

- `lsblk`查看電腦磁碟掛載情況，sd 開頭為 SSD 或 HDD;nvme 開頭為 nvme SSD
- 假設要設定 SSD 為安裝碟，會顯示 sda 及 sda1，確認大小是否正確

```bash
fdisk -l
```

- `fdisk -l` 可以查看關於磁碟更多的資訊，以確定指向選擇的安裝磁碟

```bash
gdisk /dev/sda
```

- `gdisk /dev/sda` 對 sda 指定進入 gdisk 模式：
  - 輸入`X`進入 gdisk 專家模式
  - 輸入`z`為抹除 sda 動作
  - 輸入`y`，同意抹除 sda 磁碟
  - 詢問Blank out MBR? 輸入`y`[^2]
  - 此時`lsblk`會發現原本 2 個 sda 變成 1 個 sda，無分區

#### 檢驗 Arch linux 下載與更新系統金鑰

- 指令：`pacman -Sy archlinux-keyring`
- 下載、更新確定檔案完整

```bash
pacman -Sy archlinux-keyring
```

## 開始安裝

- 指令：`pacman -Sy archinstall`
- 進入 Arch Linux 安裝腳本

```bash
pacman -Sy archinstall
```

> [!Tip] 斜線 `/` 作用
>
> - 在archinstall中，在眾多選項找名稱時，可以直接按鍵盤`/`輸入文字快速找尋

- 安裝選項重點設定：
  - Archinstall language：無法更改
  - Mirrors：`TW`
  - Locales➡️Locale language：zh-TW-UTF8
  - Disk configuration➡️Partitioning➡️`Use a best-effort default partition layout`➡️選擇 sda 磁碟(按空白鍵)➡️我選 ext4
  - Disk encryption：不加密
  - Boot loader：選`Grub`
  - Swap：`True`
  - Hostname：預設
  - Root password：輸入密碼
  - User password(新增使用人員 ID 與密碼，以後登入就用這組)：
    - Add a user：輸入 ID、密碼
    - be a superuser(sudo)：`yes(default)`
    - `confirm and exit`
  - Profile(系統類型)：
    - Type ➡️ `Desktop` ➡️ KDE Plasma
    - Graphics driver(顯示卡 GPU)：筆電有 intel 內顯與 Nvidia 獨顯，我先以內顯安裝
      - `All open-source`
    - greeter(磁碟為SSD者)：`sddm`
  - Audio：`Pipewire`
  - kernels：Linux(預設)
  - Additional packages：如果 Language 為 EN 擇不添加，中文要加繁體字型，不然亂碼
    - 輸入`noto-fonts-cjk`
  - Network configuration：`use NetworkManager`
  - Timezone：Asia/Taipei
  - 完成

---

## Q&A

#### 亂碼
>
> [!NOTE]
> 遇到亂碼
> 進入桌面後，在 archinstall 階段忘了加入 noto-fonts-cjk，以致進入系統為亂碼
依照教學下載安裝字體，再系統生成，重開機即正常顯示中文字

#### Nvidia Driver setup
>
> [!NOTE] 安裝 NVIDIA DRIVER
> 至於 Nvidia 顯示卡，Arch 的儲存庫有提供 Nvidia 顯示卡的專有驅動，不需要額外加套件庫。安裝後 nouveau 會自動被停用。

```bash
pacman -S linux-headers nvidia-dkms nvidia-settings
```

#### Discover 軟體庫問題
>
> [!NOTE]
> 處理 Discover 應用程序端問題
>
> - [連結](https://youtu.be/AYxaNjbC1wg?t=746&si=VXHH-Me6s8Gcoid6)
> - 打開 KONSOLE
> - 輸入`sudo pacman -Sy flatpak`
>
#### yay
>
> [!NOTE]
> 安裝 yay

```bash
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

1. 編輯：`sudo vim /etc/makepkg.conf`，找到`MAKEFLAGS="-j2"`這行，取消註解，再將後面改成`"-j$(nproc)"`。這樣在編譯 AUR 套件時即會使用全部 CPU。
2. 編輯：`sudo vim /etc/pacman.conf`，取消註解`Color`和`ParallelDownloads`，開啟顏色和平行下載套件。再自行新增`ILoveCandy`參數開啟 pacman 的彩蛋。
3. 因為AUR是先編譯後打包，執行時`yay`指令的時候不需要加 sudo，只有打包後要安裝套件時需要密碼。

#### 中文輸入法
>
> [!NOTE]
> 加入中文輸入法
> `sudo pacman -S fcitx5-im fcitx5-chewing fcitx5-qt fcitx5-gtk fcitx5-chinese-addons`

- 在 KDE 左下角的應用程式列表搜尋「Fcitx5 設定程式」，加入「新酷音」注音輸入法。
- 📌最後中文用雷米輸入法
  - `sudo apt install fcitx5-rime`
  - 控制台 ➡️ 鍵盤 ➡️ 虛擬鍵盤 ➡️ Fcitx5
  - 用 KDE plasma 的話應該只要 X 開頭那行就可以了，

```bash
vim /etc/environment
```

加入 `XMODIFIERS=@im=fcitx`
參考：<https://home.gamer.com.tw/artwork.php?sn=5577703>
完成

#### fastfetch

- 安裝 fastfetch：[網址](https://github.com/fastfetch-cli/fastfetch)
- Arch Linux: `pacman -S fastfetch`
- 完成

#### Docker

##### **1. 安裝必要套件**

先確保系統安裝了 `docker` 和 `docker-compose`（Docker Compose V2 是內建的 `docker compose` 指令）。

```bash
sudo pacman -Syu docker docker-compose
```

##### **2. 啟動並設定 Docker**

啟動 Docker 並讓它開機自動啟動：

```bash
sudo systemctl enable --now docker
```

將你的使用者加入 `docker` 群組（可選，這樣可以不用 `sudo` 執行 Docker）：

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Portainer：
<https://ipv6.rs/tutorial/Arch_Linux/Portainer/>

```bash
sudo docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```

安裝完自己輸入密碼登入設定

## 平時維護

> [!NOTE]
> 詢問AI：請問Archlinux是滾動式的更新，若輸入`sudo pacman -Syu`指令，有需要注意什麼，避免更新後系統損毁或無法登入?

> [!TIP]
> ChatGPT 回覆：
>
### 1. 查看官方公告

- 在執行更新前，建議先查看 [Arch Linux News](https://archlinux.org/news/) 是否有重大變更。例如：
  - 內核 (`linux`)、GLIBC、Xorg、NVIDIA 驅動等可能會導致系統無法啟動。
  - 變更可能會影響你的服務 (如 Docker、Portainer、OMV)。

```bash
sudo pacman -Syu
```

### 2. 備份重要資料

更新前建議至少備份：

- **系統設定檔**
  - `/etc/`
  - `/var/lib/`
  - `/home/`（個人資料）
  - `/boot/`（如果手動管理 kernel 或啟動引導）
- **已安裝的套件清單**

```bash
    pacman -Qqe > pkglist.txt
```

📌下完指令`pkglist.txt`檔會存放目前所在位置(輸入`pwd`可查詢目前位置)

### 3. 避免「部分升級」

如果要更新，務必完整升級 (`pacman -Syu`)，**不要**部分升級 (`pacman -Sy` 後只裝部分軟體)，避免相依性問題。

### 4. 注意 AUR 套件

如果你有安裝 AUR 套件，建議在 `pacman -Syu` 之後，使用 `paru` 或 `yay` 更新 AUR：
`paru -Syu   # 或 yay -Syu`
有時 AUR 套件的相依性會與官方套件有衝突，可能需要手動處理。

### 5. 手動更新密鑰環

如果有時不可避免地在<u>一段時間裡沒有更新系統</u>，或者在安裝系統時使用的[安裝映像](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97#%E8%8E%B7%E5%8F%96%E5%AE%89%E8%A3%85%E6%98%A0%E5%83%8F "安裝指南")版本較舊，可能會遇到密鑰環過舊而導致的簽名錯誤，提示為「文件簽名損壞」。這時你可以在更新系統之前手動同步包資料庫並升級 [archlinux-keyring](https://archlinux.org/packages/?name=archlinux-keyring) 包：

```bash
 pacman -Sy archlinux-keyring
```

此命令成功以後，應**立即**升級所有軟體包以避免[部分升級](https://wiki.archlinuxcn.org/wiki/%E9%83%A8%E5%88%86%E5%8D%87%E7%BA%A7 "部分升級")：

```bash
 pacman -Su
```

### 總結

1. **先查 Arch Linux 官方新聞，確保沒有破壞性更新**。
2. **完整備份重要設定檔與安裝套件清單**。
3. **安裝 LTS 內核作為備案**，避免內核更新導致無法開機。
4. **更新前確保 `archlinux-keyring` 是最新的**，避免 GPG 錯誤。
5. **避免在 SSH 上直接更新，或至少使用 `screen` 保持連線**。
6. **更新後檢查 `.pacnew` 設定檔，避免配置衝突**。
7. **如有問題，可使用 `arch-chroot` 進行修復**。
這樣做可以最大程度降低更新導致系統崩潰的風險。

## 參考

> [!INFO]
> 五分鐘快速安裝 Arch Linux 指南：[連結](https://www.youtube.com/watch?v=AYxaNjbC1wg&t=5s)
> **Ksk Royal 的 YouTube 影片**提供了一個 **簡潔快速** 的 Arch Linux 安裝指南，著重於使用 **archinstall 指令稿** 在任何電腦或筆記型電腦上輕鬆設定此作業系統。影片示範了在 **Windows 11 筆記型電腦上進行雙重開機** 的過程，並詳細說明了 **下載 ISO 映像檔、製作開機 USB、設定 BIOS、連接網路、磁碟分割、選擇桌面環境、安裝常用軟體** 等步驟。此外，影片還涵蓋了 **後續設定**，例如修正 Discover 應用程式的後端以及將 Windows 開機選項新增至 GRUB 選單。

> [!INFO]
> Arch Linux 從零開始安裝教學：[連結](https://lingyinaudio.com/tool-tutorial-archlinux-install/)
> 此份教學文件**詳述了從零開始手動安裝 Arch Linux 作業系統的完整流程**，目標是打造一個**極簡且個人化的系統**。內容涵蓋了**前置準備、分割磁區、安裝核心套件、設定系統、安裝桌面環境 KDE Plasma、中文輸入法，以及其他常用軟體**。此外，文章也**簡要提及了使用 `archinstall` 腳本進行快速安裝的方法**，並分享了作者**將 Arch Linux 應用於音響系統的經驗與推薦**。

> [!INFO]
> Arch Linux安裝教學，KDE Plasma桌面＋中文輸入法 · Ivon的部落格：[連結](https://ivonblog.com/posts/install-archlinux/)
> 此部落格文章提供一份詳盡的Arch Linux安裝教學，目標是建立一個具備KDE Plasma桌面環境和中文注音輸入法的正體中文系統。文章詳細說明了從製作開機隨身碟、硬碟分割、基本系統安裝，到桌面環境、驅動程式、中文輸入法和開機引導的設定步驟，並介紹了pacman套件管理員和AUR的使用。作者分享了選擇Arch Linux的原因，強調其高度自訂性、軟體版本新穎、滾動更新以及豐富的社群資源，但也提醒Arch Linux並非新手友善，適合有一定Linux經驗的使用者。

[^1]: Pacman 是一個軟體包管理器，作為 Arch Linux 發行版的一部分。它最早由 Arch Linux 的 Judd Vinet 開發。 Pacman**可以解決安裝過程中的依賴問題，自動下載並且安裝所有需要的軟體包**。 Pacman 也被移植到 Windows，作為基礎系統的一部分隨 MSYS2 分發。
[^2]: 以 MBR 方式分割制造 MBR 磁區
