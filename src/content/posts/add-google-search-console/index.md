---
title: 網站設定google search console
published: 2025-03-29
description: '開啟網站被google search檢索設定'
image: './cover.png'
tags: [Google Search, site]
category: 'Blog'
draft: false 
lang: ''
---
## 緣由

最近才建置好blog，但測試了一下google search，怎麼都找不到網站或文章關鍵字的資訊，上找搜尋了一下才想起要加入google search console😆

## 環境

網站：Astro Blog(本機astro儲存庫 ➡️ push to Github ➡️ Cloudflare Pages)

## 設定

- 進入[google search console](https://search.google.com/search-console/about)(這裡簡稱GSC)頁面

> [!NOTE]
> 選擇`網址前置字元`，輸入網站URL ➡️ 繼續
<div style="width: 80%; margin: 10px">
    <img src="https://tt5083.github.io/picx-images-hosting/add-google-search-console/google-search-console.webp" alt="google search console setting">
</div>

- 驗證擁有權

> [!NOTE]
>
> 這裡我選用`HTML標記`，複制`<meta anme="google...XXXXX">`程式碼

<div style="width: 80%; margin: 10px">
    <img src="https://tt5083.github.io/picx-images-hosting/add-google-search-console/google-search-console-1.webp" alt="get html meta code on google search console">
</div>

- 網站設定
  - 我的Blog是Astro，修改`/src/layouts/Layout.astro`，在`<head>`之內，貼上剛才複製好的HTML標記
  - 推送更新

```bash
git add .
git commit -m "add google search console"
git push
```

- 開始驗證

<div style="width: 80%; margin: 10px">
    <img src="https://tt5083.github.io/picx-images-hosting/add-google-search-console/google-search-console-2.webp" alt="google search console verification">
</div>

> [!TIP]
> 在google search console主頁點擊開始驗證，會跳出以下畫面，點擊前往資源
> 等個1~2天再回來看看GSC驗證結果

<div style="width: 80%; margin: 10px">
    <img src="https://tt5083.github.io/picx-images-hosting/add-google-search-console/google-search-console-3.webp" alt="wait verification">
</div>

## 參考

- [【教學】如何將 Cloudflare Page 加入 Google Search Console？－Mr. W 電腦村莊｜痞客邦](https://william8510.pixnet.net/blog/post/576793988)
