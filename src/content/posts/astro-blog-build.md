---
title: 用Astro建置Blog
published: 2025-03-27
description: 'Creat an Astro Blog'
image: ''
tags: [Blog, Astro, Github]
category: 'Blog'
draft: false 
lang: ''
---
## 環境

- AMD Ryzen 7 3700X 8-Core Processor/32 GB
- Windows 11 家用版：24H2
- node：v20.19.0
- pnpm：10.6.5
- nvm：1.2.2
- astro：v5

## 事前準備

- 安裝Node.js
- 工具：Visual Studio code,  [Astro官方套件](https://marketplace.visualstudio.com/items?itemName=astro-build.astro-vscode).
- **Terminal** - Astro is accessed through its command-line interface (CLI).
- **PNPM** - 是一個用於管理 JavaScript 和 Node.js 專案依賴的軟體包管理工具。

## 選用主題式安裝

覺得適合的主題 [fuwari](https://github.com/saicaca/fuwari)，安裝教學(2024-09-10) 提到：

1. 使用此範本[生成新倉庫](https://github.com/saicaca/fuwari/generate)或 [Fork 此倉庫](https://github.com/saicaca/fuwari)

> [!NOTE]
> 本篇部署方式採用 Cloudflare Pages，所以 github 的倉庫名稱是否要直接用 username 不太重要。

2. 進行本地開發，Clone 新的倉庫，在blog資料夾安裝依賴`pnpm install`、`pnpm add sharp`
    - 若未安裝 [pnpm](https://pnpm.io/)，執行`npm install -g pnpm`
    - 以下的 username 可以是其它名稱

```bash
 git clone https://github.com/username/username.github.io.git
 cd username.github.io
 pnpm install
 pnpm add sharp
```

3. [Cloudflare註冊帳號](https://dash.cloudflare.com/)，左邊點擊`Workers和Pages` ➡️ 連線至 Git ➡️ 連結你的 github 帳號

- 選擇存放庫選：clone 主題(fuwari)的倉庫➡️開始設定
- Framework預設：`Astro`
- 組建命令：npm run build (預設)
- 組建輸出目錄：/dist (預設)
- ➡️開始部署

4. 等一小段時間部署完成後，此時網站還無法瀏覽，Cloudflare會給你URL，複制貼上到astro.config.mjs裡的site，終端機切到blog目錄：

```bash
git add .
git commit -m "修改site"
git push
```

5. cloudflare pages會顯示更新中，完成後點擊blog網址會出現clone好的網站
