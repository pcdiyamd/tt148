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

4. 部署完成後，此時網站還無法瀏覽，Cloudflare會給你URL，複制貼上到astro.config.mjs的site對應網址，終端機切到blog目錄：

```bash
git add .
git commit -m "修改site"
git push
```

5. cloudflare pages會顯示更新中，完成後點擊blog網址會出現clone好的網站
6. 修改網站名稱、個人連結、網站語言設定等，在`src/config.ts`檔案中

## 安裝套件(選用)

#### 1. 超連結用 rehype-external-links

- 檢視 Blog 發現外部連結都是直接開啟，不是新開視窗，google 查詢後發現有解。
- 依 Camel's blog教學[^2]方法 1 所示，可以藉由開源寫好的直接引用，小修改一下即可😀

```bash
pnpm install rehype-external-links
```

```js
// import externalLink plugin
import rehypeExternalLinks from 'rehype-external-links';
// astro.config.ts
markdown: {
  // 加上你自己寫的 plugin 設定
  rehypePlugins: [
    [
      rehypeExternalLinks,
      {
        target: "_blank",
      },
    ],
  ],
}
```

astro.config.ts(在astro blog根目錄下)，加入`import externalLink from "./src/externalLink";`，並在`rehypePlugins:`內容中加入 markdown 樣式，這裡要小心操作😂

#### 2. Footnotes 顯示為中文：註腳

在寫文章的內容引用到footnote功能，會呈現在文章最底下，astro預設顯示為英文footnote，我想將改為顯示中文`註腳`

- 安裝必要的 Remark 套件（如果尚未安裝）：

```bash
pnpm install remark-rehype rehype-stringify
```

- 修改 Astro 設定 astro.config.mjs：

```js
import { defineConfig } from 'astro/config';
import remarkRehype from 'remark-rehype';

export default defineConfig({
  markdown: {
  // 設定 footnoteLabel 為中文
    remarkPlugins: [
      [remarkRehype, { footnoteLabel: '註腳' }] 
    ],
  },
});
```

這樣 `footnoteLabel` 應該就會變成 `"註腳"`

> [!TIP]
> 增加套件注意事項：
> 每個套件增加在 astro.config.mjs 中，markdown: {}裡的內容，注意應對

常用的功能指令：

| `pnpm install`並`pnpm add sharp` | 安裝依賴                             |
| ------------------------------- | -------------------------------- |
| pnpm dev                        | 在啟動本地開發伺服器<http://localhost:4321/> |
| pnpm build                      | 構建網站至./dist/                     |
| pnpm preview                    | 本地預覽已構建的網站                       |
| pnpm new-post <filename>        | 創建新文章                            |
| pnpm astro ...                  | 執行，等指令`astro add``astro check`   |
| pnpm astro --help               | 顯示 Astro CLI 説明                  |
