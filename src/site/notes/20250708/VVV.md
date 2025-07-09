---
{"dg-publish":true,"permalink":"/20250708/VVV/"}
---

![Pasted image 20250709103050.png](/img/user/Pasted%20image%2020250709103050.png)
跑起來執行成功會這樣
![Pasted image 20250709103100.png](/img/user/Pasted%20image%2020250709103100.png)

如果要修改對他執行修改就可以了
![Pasted image 20250709103122.png](/img/user/Pasted%20image%2020250709103122.png)
他跟Live server一樣 ![Pasted image 20250709103207.png](/img/user/Pasted%20image%2020250709103207.png)![Pasted image 20250709103400.png](/img/user/Pasted%20image%2020250709103400.png)
網站首頁在public底下
![Pasted image 20250709103433.png](/img/user/Pasted%20image%2020250709103433.png)

---

# Vue 網站基本架構

這是一個 Vue 專案的基本結構，將其整理並延伸解釋，方便您在 HackMD 中使用。

## 專案結構概覽

一個典型的 Vue CLI 建立的專案會包含以下核心資料夾與檔案：

```
vueproject/
├── public/
│   ├── favicon.ico
│   └── index.html
├── src/
│   ├── assets/
│   │   └── (放置圖片、CSS 等靜態資源)
│   ├── components/
│   │   └── HelloWorld.vue
│   ├── App.vue
│   └── main.js
├── .gitignore
├── babel.config.js
├── package.json
├── package-lock.json
├── README.md
└── vue.config.js
```

## `public` 資料夾

- **`favicon.ico`**: 網站的小圖示，顯示在瀏覽器頁籤上。
    
- **`index.html`**: 網站的入口 HTML 檔案。所有 Vue 應用程式的內容最終都會被掛載到這個檔案中的 `<div id="app">` 元素裡。
    
## `src` 資料夾 (專案核心原始碼)

這是您主要進行開發工作的目錄，包含了應用程式的原始碼。

- **`assets/`**: 用於存放專案中的靜態資源，例如圖片 (如同您提供的圖片所示)、通用 CSS 檔案、字體等。這些檔案在打包時會經過 Webpack (或其他打包工具) 處理。
    
- **`components/`**: 放置可重複使用的 Vue 元件。每個 `.vue` 檔案通常對應一個獨立的元件，例如 `HelloWorld.vue`。
    
- **`App.vue`**: 這是 Vue 應用程式的「根元件」。它作為所有其他元件的父級，是整個應用程式的骨架。一個 `.vue` 檔案包含了 `<template>` (HTML 結構)、`<script>` (JavaScript 邏輯) 和 `<style>` (CSS 樣式) 三個區塊。
    
- **`main.js`**: 這是 Vue 應用程式的 JavaScript 入口點。它負責初始化 Vue 實例，導入根元件 (`App.vue`) 並將其掛載到 `index.html` 中的指定元素 (通常是 `#app`)。也會在此檔案中註冊全域元件或載入其他 Vue 函式庫。
    
- **`views/` (常見於較大型專案)**: 在一些專案中，可能會看到一個 `views` 或 `pages` 資料夾，用於存放應用程式的「頁面級」元件。這些元件通常與路由 (Vue Router) 配置相關聯，每個 `view` 元件代表應用程式的一個獨立頁面。
    

## 其他重要檔案

- **`.gitignore`**: Git 版本控制的忽略檔案清單，指定哪些檔案或資料夾不應被 Git 追蹤 (例如 `node_modules/`)。
    
- **`babel.config.js`**: Babel 的設定檔，用於將 ES6+ 的 JavaScript 程式碼轉換為向後相容的 JavaScript，以便在各種瀏覽器環境中執行。
    
- **`package.json`**: 專案的設定檔，包含了專案的元資料 (名稱、版本等)、腳本命令 (如 `npm run serve`, `npm run build`) 和所有相依的套件及版本號清單。
    
- **`package-lock.json`**: 鎖定 `package.json` 中套件的確切版本，確保不同開發者安裝的套件版本一致。
    
- **`README.md`**: 專案的說明文件，通常包含專案的介紹、安裝與運行方式等。
    
- **`vue.config.js`**: Vue CLI 的設定檔，用於對 Webpack 配置進行更細緻的調整，例如配置代理、靜態資源路徑等。
    

## Vue 元件 (Vue Components)

Vue 應用程式由多個可組合、可重複使用的元件構成。每個 `.vue` 單一檔案元件 (Single File Component, SFC) 封裝了該元件的 HTML (Template)、JavaScript (Script) 和 CSS (Style)。

- **`<template>`**: 定義元件的 HTML 結構和呈現邏輯。
    
- **`<script>`**: 包含元件的 JavaScript 邏輯，例如資料 (data)、方法 (methods)、生命週期鉤子 (lifecycle hooks) 等。
    
- **`<style>`**: 定義元件的 CSS 樣式。可以使用 `scoped` 屬性將樣式限定在當前元件內，避免樣式衝突。
    

## Vue 的核心概念

- **響應式系統 (Reactivity System)**: Vue 最強大的功能之一。當資料改變時，Vue 會自動偵測並更新 DOM，無需手動操作。
    
- **組件化 (Component-based)**: 將複雜的 UI 拆分成小型、獨立且可重複使用的元件，提高開發效率和程式碼的可維護性。
    
- **聲明式渲染 (Declarative Rendering)**: 開發者只需聲明 UI 應該呈現的狀態，Vue 會負責底層的 DOM 操作。
    
- **單頁應用程式 (Single Page Application, SPA)**: Vue 常被用於建立 SPA，意味著網頁載入後，所有內容的更新和切換都在單個 HTML 頁面內完成，提供流暢的使用者體驗。





# ICON製造機 (避免版權) 
favicon
![Pasted image 20250709104111.png](/img/user/Pasted%20image%2020250709104111.png)


![Pasted image 20250709104152.png](/img/user/Pasted%20image%2020250709104152.png)s
### 這裡把他換掉

![Pasted image 20250709104752.png](/img/user/Pasted%20image%2020250709104752.png)

## 如果你要開放某些功能給人用 你要加上 export 

## 我公開是helloworld  所以你export 也要 helloword 
![Pasted image 20250709104827.png](/img/user/Pasted%20image%2020250709104827.png)

![Pasted image 20250709105023.png](/img/user/Pasted%20image%2020250709105023.png)
#註冊元件 

![Pasted image 20250709105335.png](/img/user/Pasted%20image%2020250709105335.png)

### Vue 主要就是三個區 
第一區是 template
第二區是export default (ES5/ES6) (另外一種語法的js)
第三區是 style區

蓋一個小小的網頁房子，Vue 就像是幫你蓋房子的工具箱，裡面有三個重要的部分：

1.  **Template (設計圖區):**
    *   這裡就像是房子的「設計圖」。
    *   你告訴 Vue 你的網頁看起來會是什麼樣子？裡面有什麼文字？要放圖片嗎？按鈕要放在哪裡？
    *   總之，就是決定網頁的「骨架」和「內容」。

2.  **Export Default (大腦和控制中心):**
    *   這裡就像是房子的「大腦」或「控制中心」。
    *   你可以在這裡寫程式碼，告訴網頁要做什麼事情。
    *   比如，當有人點擊按鈕時，網頁要顯示一段文字；或者網頁一打開，就要從網路上抓取一些資料來顯示。
    *   所有讓網頁「動起來」或處理事情的邏輯，都在這裡寫。

3.  **Style (裝潢區):**
    *   這裡就像是房子的「裝潢師」。
    *   你可以在這裡設定網頁的樣子，讓它看起來更漂亮。
    *   比如，文字要用什麼顏色？字體要多大？圖片旁邊要留多少空白？背景要用什麼顏色？
    *   總之，就是決定網頁的「外觀」和「風格」。

所以，簡單來說：
*   **Template** 決定網頁「長什麼樣子」和「有什麼內容」。
*   **Export Default** 決定網頁「會做什麼事」。
*   **Style** 決定網頁「看起來有多漂亮」。
----



![Pasted image 20250709105600.png](/img/user/Pasted%20image%2020250709105600.png)

# Main.js對應到app 

![Pasted image 20250709105638.png](/img/user/Pasted%20image%2020250709105638.png)
# Vue有提供import 所以
# 會變成 createApp 

![Pasted image 20250709105933.png](/img/user/Pasted%20image%2020250709105933.png)

## babeijs.io

![Pasted image 20250709113308.png](/img/user/Pasted%20image%2020250709113308.png)
	
unmounted 不要用的時候釋放掉