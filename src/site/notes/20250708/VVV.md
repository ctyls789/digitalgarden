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
-----

![Pasted image 20250709134258.png](/img/user/Pasted%20image%2020250709134258.png)

![Pasted image 20250709134645.png](/img/user/Pasted%20image%2020250709134645.png)

### 1. `{{ }}` 雙大括號 (Mustache 語法)

- **概念：** 這是 Vue 最基本的文本插值方式。它允許您將資料（data）從 Vue 實例綁定到 HTML 模板中的文本內容。當資料改變時，綁定的內容也會自動更新。
    
- **延伸：** 雙大括號內不僅可以放資料屬性，也可以放 JavaScript 表達式，例如 `{{ message.toUpperCase() }}` 或 `{{ count + 1 }}`。但它們只能用於文本內容，不能用於 HTML 屬性。
    
- **HackMD 應用：** HackMD 不直接執行 Vue.js 程式碼，但如果您想在 HackMD 中展示 Vue 程式碼範例，會經常使用到這個語法。
    

### 2. `v-text` 指令

- **概念：** 用於更新元素的 `textContent`。它會完全替代元素內部的文本內容。
    
- **用法：** `<span v-text="message"></span>`
    
- **與 `{{ }}` 比較：** `{{ }}` 更常用且更具彈性，因為它允許您在元素內部混合文本和資料。`v-text` 則會覆蓋整個元素的文本內容。在大多數情況下，`{{ }}` 是更好的選擇。
    
- **HackMD 應用：** 同上，用於展示程式碼範例。
    

### 3. `v-html` 指令

- **概念：** 用於更新元素的 `innerHTML`。它會將綁定的值作為純 HTML 插入到元素中。
    
- **用法：** `<div v-html="rawHtml"></div>`
    
- **安全性注意：** **非常重要！** 使用 `v-html` 插入用戶提供的內容時，存在 **XSS (跨站點腳本攻擊)** 的風險。因為它會直接渲染 HTML，如果內容包含惡意腳本，這些腳本將被執行。永遠只在您信任的內容上使用 `v-html`。
    
- **HackMD 應用：** 用於展示程式碼範例，強調其插入 HTML 的功能和潛在的安全風險。
    

### 4. `v-model` 指令

- **概念：** 用於在表單輸入元素或組件上創建**雙向數據綁定**。這意味著：
    
    - 當資料改變時，輸入框的值會更新。
        
    - 當用戶在輸入框中輸入內容時，綁定的資料也會自動更新。
        
- **用法：** `<input v-model="searchText">`
    
- **常用於：** `<input>`, `<textarea>`, `<select>` 元素，以及自定義組件。
    
- **修飾符 (Modifiers)：** `v-model` 提供一些修飾符來方便處理輸入：
    
    - `.lazy`：在 `change` 事件觸發時更新數據，而不是 `input` 事件。
        
    - `.number`：將用戶輸入的字符串自動轉為數字。
        
    - `.trim`：自動去除輸入的首尾空白字元。
        
- **HackMD 應用：** 可以在 HackMD 中列出 `v-model` 的範例程式碼，展示其在表單中的應用。
    

### 5. `v-bind` 指令 (簡寫為 `:`)

- **概念：** 用於**單向綁定**一個或多個特性 (attribute)，或一個組件 prop 到表達式。當資料改變時，元素的特性會更新。
    
- **用法：**
    
    - 綁定特性：`<img v-bind:src="imageUrl">` 或簡寫為 `<img :src="imageUrl">`
        
    - 綁定 Class：`<div :class="{ active: isActive }"></div>`
        
    - 綁定 Style：`<div :style="{ color: textColor }"></div>`
        
- **延伸：** 可以動態地綁定任何 HTML 屬性，而不僅僅是 `src`。這使得您可以根據資料動態地改變元素的行為、外觀和狀態。
    
- **HackMD 應用：** 常用於展示如何動態綁定圖片、CSS Class 或 Style 的程式碼範例。
    

### 6. `v-pre` 與 `v-cloak` 指令

- **`v-pre`：**
    
    - **概念：** 跳過元素和它的子元素的編譯過程。這在顯示原始 Vue 模板語法時非常有用，避免 Vue 將它們當作可編譯的指令。
        
    - **用法：** `<span v-pre>{{ 這不會被編譯 }}</span>`
        
    - **用途：** 常用於展示 Vue 語法本身，或在不需要 Vue 控制的靜態內容上節省編譯開銷。
        
- **`v-cloak`：**
    
    - **概念：** 用於隱藏尚未編譯完成的 Vue 實例。在 Vue 實例掛載之前，`v-cloak` 屬性會保留在元素上；當實例掛載並完成編譯後，該屬性會自動被移除。
        
    - **用法：**
        
        HTML
        
        ```
        <div v-cloak>
          {{ message }}
        </div>
        <style>
          [v-cloak] { display: none; }
        </style>
        ```
        
    - **用途：** 解決「閃爍」問題 (FOUC - Flash Of Unstyled Content)，即在 Vue 完成渲染之前，瀏覽器可能會短暫顯示未編譯的 Mustache 語法或其他模板內容。
        
- **HackMD 應用：** 可以在範例程式碼中展示它們，特別是 `v-cloak` 搭配 `<style>` 隱藏未渲染內容的用法。
    

### 7. `v-once` 指令

- **概念：** 只渲染元素和組件一次。之後的重新渲染，元素/組件及其所有子元素/組件的內容將被視為靜態內容並跳過。
    
- **用法：** `<span v-once>{{ initialMessage }}</span>`
    
- **用途：** 性能優化。當您確定某部分內容在初次渲染後不會再改變時，使用 `v-once` 可以減少不必要的重新渲染開銷。
    
- **HackMD 應用：** 用於展示性能優化的程式碼範例。
    

### 8. `v-if` / `v-else-if` / `v-else` 指令

- **概念：** 用於條件性地渲染元素或組件。基於表達式的真假值，來決定是否將元素從 DOM 中**添加或移除**。
    
- **用法：**
    
    HTML
    
    ```
    <div v-if="type === 'A'">Type A</div>
    <div v-else-if="type === 'B'">Type B</div>
    <div v-else>Other Type</div>
    ```
    
- **與 `v-show` 比較：**
    
    - **`v-if`：** 是「真實的」條件渲染。它會在條件為假時銷毀元素及其事件監聽器和子組件；條件為真時重建。這開銷較大，適合不頻繁切換的場景。
        
    - **`v-show`：** 總是會渲染元素，只是簡單地透過 CSS 的 `display` 屬性來切換元素的顯示或隱藏。這開銷較小，適合頻繁切換的場景。
        
- **HackMD 應用：** 用於展示不同條件下渲染不同內容的邏輯程式碼。
    

### 9. `v-show` 指令

- **概念：** 用於條件性地顯示元素。它是通過切換元素的 CSS `display` 屬性來實現的。元素總是存在於 DOM 中。
    
- **用法：** `<div v-show="isVisible">This will show/hide</div>`
    
- **HackMD 應用：** 展示與 `v-if` 不同的條件渲染方式。
    

### 10. `v-for` 指令

- **概念：** 用於基於源數據多次渲染一個元素或模板塊。它通常用於遍歷數組或對象來生成列表。
    
- **用法：**
    
    - 遍歷數組：`<li v-for="item in items" :key="item.id">{{ item.name }}</li>`
        
    - 遍歷對象：`<div v-for="(value, key) in object" :key="key">{{ key }}: {{ value }}</div>`
        
    - 遍歷範圍：`<span v-for="n in 10">{{ n }}</span>`
        
- **`key` 屬性：** **非常重要！** 當使用 `v-for` 時，**必須**為每個渲染的項目提供一個唯一的 `:key` 屬性。這有助於 Vue 追蹤每個節點的身份，從而有效地重用和重新排序現有元素，提高性能和保持組件狀態。

```html
<div id="app">
  <p>{{ message }}</p>
  <input v-model="message">
  <button v-on:click="reverseMessage">反轉訊息</button>

  <div v-if="showContent">
    這是條件顯示的內容。
  </div>
  <div v-else>
    條件為假時顯示。
  </div>

  <ul>
    <li v-for="item in items" :key="item.id">
      {{ item.text }}
    </li>
  </ul>
</div>

<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!',
    showContent: true,
    items: [
      { id: 1, text: '蘋果' },
      { id: 2, text: '香蕉' },
      { id: 3, text: '橘子' }
    ]
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
```