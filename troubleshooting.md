# 專案故障排除與修改紀錄 (Troubleshooting & Update Log)

本文件記錄了本專案在與 Firebase 整合及上線部署過程中遇到的問題與修正方法，供後續維護或 AI 協作參考。

---

## 1. 字詞修正（渡 $\rightarrow$ 度）

### 【狀況描述】
網頁中的宣傳標語「一燈引路，渡得一人，願以此心，廣結善緣」、以及介面上的「發願渡人」、「想渡的朋友姓名」等處使用了中文字「渡」。在佛教及本專案的招生推廣語境中，「度」代表度化、引度、普度眾生，更符合教義標準。

### 【修正方案】
* 全面檢索專案，將主網頁 `index.html`、原型網頁、`README.md` 等檔案中的「渡」字全部替換為「**度**」。
* 將包含舊字詞的實體檔案名稱一併重新命名：
  * `招生點燈-渡人點燈原型.html` $\rightarrow$ `招生點燈-度人點燈原型.html`
  * `版本一/招生點燈-渡人點燈原型.html` $\rightarrow$ `版本一/招生點燈-度人點燈原型.html`

---

## 2. 累計發願計數器卡在 0 盞且不遞增

### 【狀況描述】
網頁與 Firebase 連線後，首頁上方的「現在已發願度人」數量被重設為 0 盞，且網頁開啟時，天燈持續升起但數字完全沒有動態遞增。
* **原因 A**：原本網頁寫死了 `1,286` 盞作為底數，但程式在載入 Firebase 時覆寫了此變數。
* **原因 B**：原程式碼邏輯中限制了「只有在沒有連線 Firebase 時才在天燈飄升時加數」，使有串接 Firebase 的正式版網頁完全失去動態遞增的互動效果。

### 【修正方案】
1. **首頁初始值**：主網頁 HTML 靜態初始顯示改為 `0` 盞，避免一開始載入時閃爍顯示舊底數。
2. **剔除模擬加數，確保真實性**：配合使用者「希望顯示真實點燈人數，不要虛假膨脹」的要求，移除所有本機自動累加（模擬加數）的邏輯。
3. **即時連動 Firebase**：網頁人數完全以 Firebase Realtime Database 內的 `wishes` 節點子節點數量（`snapshot.numChildren()`）為依歸。每當有新願力寫入資料庫，人數即時更新。

---

## 3. Firebase 資料庫權限過期導致數據無法載入

### 【狀況描述】
資料庫內已有 20 筆真實點燈名單，但網頁開啟時依然顯示 `0` 盞，且背景完全沒有飄出任何天燈（甚至沒有預設天燈）。
* **原因**：Firebase 在建立 Realtime Database 時，若選用「測試模式」，預設只會有 **30 天**的讀寫有效期。過期後，Firebase 規則會自動將 `.read` 與 `.write` 鎖定為 `false`，導致網頁發起讀取請求時被拒絕（Permission Denied）。由於 JavaScript 未攔截此錯誤而崩潰，導致天燈與計數器全部失效。

### 【修正方案】
* 引導使用者至 Firebase Console $\rightarrow$ Realtime Database $\rightarrow$ **「規則」 (Rules)** 標籤頁下，將規則修改為允許公開讀寫，並重新發布：
  ```json
  {
    "rules": {
      ".read": true,
      ".write": true
    }
  }
  ```

---

## 4. Firebase SDK ESM 模組化載入相容性問題

### 【狀況描述】
在前一版本中，Firebase SDK 被升級為最新的 v9+ Modular ESM 模組化寫法，並使用 `<script type="module">` 載入。但在部分瀏覽器、或使用者環境中，常因網路安全限制或代理伺服器攔截，導致瀏覽器無法成功解析並載入 gstatic CDN 上的 ES 模組，使得整段 JavaScript 載入失敗（天燈無法生成，點燈按鈕也無反應）。

### 【修正方案】
* 將 Firebase 的載入方式還原為最穩健的 **v8 Compat（相容）版本**，不依賴 `type="module"`：
  * **引入腳本**：
    ```html
    <script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-database-compat.js"></script>
    ```
  * **初始化與連線語法**：還原為標準的 `firebase.initializeApp()` 與 `db.ref()` 物件鏈結調用，大幅提升跨瀏覽器及各種網路環境下的運行相容性。
