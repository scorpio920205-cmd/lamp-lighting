# 心燈引渡｜普慶禪修班渡人點燈網站

## 專案簡介
本專案將中台禪寺原版「線上祈福點燈」的形式，轉化為**普慶禪修班**的招生與推廣工具。已在精舍上課的學員，可以為尚未上課的朋友點亮心燈、許下發願，期望引導（渡）他們前來禪修班上課。

專案主要採用**直式螢幕投影或播放**進行展示，營造安詳、莊嚴且具互動感的氛圍。

---

## 核心功能與特色

### 1. 雙頁式架構
- **頁面一：心燈引渡（公開展示頁）**
  - **背景自動切換**：依據系統時間自動判定白天或夜晚（6:00 - 18:00 為白天，其餘為夜晚）。
  - **手動切換按鈕**：左上角設有白天/夜晚手動切換功能，方便現場預覽與調整。
  - **動態天燈飄升**：天燈由下方往上持續緩慢升起，文字直書於燈面（學員姓名 + 朋友姓名 + 發願心願），循環輪詢播放。
  - **發願計數跳動**：上方發願數字會伴隨新天燈生成而遞增，增強互動性。
  - **我要點燈入口**：右上角醒目的「我要點燈」紅色按鈕，可點入發願表單，亦可用於現場掃描 QR Code 進入。
- **頁面二：發願表單（點燈報名頁）**
  - 輸入學員姓名（已在精舍學員）。
  - 輸入想引渡的朋友姓名（尚未上課）。
  - 六大發願心願選擇（身體健康、家庭和樂、道業成就、學業進步、事業順利、廣結善緣），對應經典燈籠圖示與精緻小標。

### 2. 視覺設計與資源整合
- **精美實景背景**：
  - **夜晚背景**（`assets/night-bg.png`）：直接使用您提供的 `精舍夜景實景圖.png` 作為夜晚背景（融合夜景、星空與人物），月亮疊在右上角。
  - **白天背景**（`assets/day-bg.jpg`）：直接使用您提供的 `白天的精舍.jpg` 作為白天背景，讓精舍在日光下的風貌自然呈現。
- **直書天燈與配色**：天燈提供鵝黃、橘、粉、薄荷綠、天藍、淺紫、米白、珊瑚紅等柔和色系，天燈內文字使用優雅的 `Noto Serif TC` 字體直書呈現。
- **流暢動畫**：包含天燈隨機左右擺動升起（Swaying & Rising）與微小的火焰閃爍與發光濾鏡。

---

## 專案結構說明
```
.
├── index.html                 # 專案首頁 (主檔案，由版本一優化而來)
├── assets/                    # 靜態資源資料夾 (發布網頁時須與 index.html 放在同層)
│   ├── day-bg.jpg             # 合成的白天精舍背景圖 ( temple + new tree + daytime light )
│   └── night-bg.png           # 合成的夜晚精舍背景圖 ( temple + new tree + stars + moon )
├── 版本一/                     # 歷史開發版本備份
│   ├── 心燈引渡-渡人點燈原型.html
│   └── night-bg.png
├── 專案總結.docx               # 原始需求說明書
├── 樹的範本.jpg                 # 原始大樹參考圖
├── 白天的精舍.jpg               # 原始白天建築參考圖
└── 精舍夜景實景圖.png            # 原始夜景建築參考圖
```

---

## 版本演進紀錄 (Version History)

### 初始原型 (Prototype v0.1)
- 建立基本天燈飄升框架與雙頁切換邏輯。
- 心願選擇為單純文字列表。

### 版本一 (Version 1.0)
- 導入夜景背景圖 `assets/night-bg.png`，並重新調整月亮定位與發光半徑。
- **表單視覺優化**：重構「發願表單」心願選擇區。加入漸變發光燈籠圖示、副標題，將原本的簡陋方格升級為圓角點選按鈕，風格大幅提升。
- **人物定位優化**：微調左下與右下學員人物的地平線位置與大小，使其與背景更融合。

### 目前版本 (Version 2.0 - 本次更新)
- **根目錄結構重組**：將主網頁 `index.html` 移至根目錄，方便 GitHub Pages 直接讀取託管。
- **白天實景背景支援**：
  - 直接使用您最真實的原始照片：`assets/day-bg.jpg`（白天精舍）與 `assets/night-bg.png`（夜晚精舍）。
  - 優化 CSS，加入 `[data-theme="day"] .stage` 以正確載入白天實景背景。
  - 當切換至白天時，自動隱藏多餘的網頁合成雲朵與太陽（`.celestial` / `.clouds`），保留純淨實景。
- **Firebase 資料庫串接**：
  - 導入了 Firebase SDK，架設好即時資料庫 (Realtime Database) 的連接通道。
  - 支援「自動同步累計願力筆數」與「後台資料庫即時拉取發願名單播放」功能。
  - 頁面二表單送出後，資料將直通您的 Firebase 資料庫，並在前端同步飄升新天燈。
- **說明文件補全**：建立本 `README.md`，方便團隊交接與後續部署。

---

## 如何在 GitHub 上部署與開啟頁面 (GitHub Pages 啟用教學)

本專案可以直接利用 GitHub Pages 進行免費的靜態網頁託管，設定步驟如下：

### 第一步：建立 GitHub 儲存庫 (Repository)
1. 登入您的 GitHub 帳號。
2. 點選右上角的 **「+」** -> **New repository**。
3. 設定 Repository name (例如：`lamp-lighting`)，將權限設為 **Public**（公開），然後點選 **Create repository**。

### 第二步：上傳專案檔案
您可以選擇使用 Git 命令行或網頁直接上傳：

#### 方法 A：使用 Git 命令行上傳（推薦）
在專案目錄下打開終端機（或 PowerShell），依序執行以下指令：
```bash
# 初始化 Git 儲存庫
git init

# 將所有檔案加入暫存區
git add index.html assets/ README.md 版本一/ 專案總結.docx 樹的範本.jpg 白天的精舍.jpg 精舍夜景實景圖.png

# 提交變更
git commit -m "Initialize project and add assets"

# 連結遠端 GitHub 儲存庫 (請將下面的網址換成您剛剛建立的儲存庫網址)
git remote add origin https://github.com/您的帳號/您的儲存庫名稱.git

# 將分支重新命名為 main
git branch -M main

# 推送到 GitHub
git push -u origin main
```

#### 方法 B：使用 GitHub 網頁直接上傳
1. 進入您剛建立的 GitHub 倉庫頁面。
2. 點選 **"uploading an existing file"** 連結。
3. 將本機資料夾中的 `index.html`、`assets/` 資料夾（包含裡面兩張背景圖）、`README.md` 等檔案拖曳上傳至網頁中。
4. 點選 **Commit changes** 提交。

### 第三步：開啟 GitHub Pages 網頁服務
1. 在 GitHub 儲存庫網頁上方，點選 **Settings**（設定）標籤頁。
2. 在左側選單中找到 **Code and automation** 區塊，點選 **Pages**。
3. 在 **Build and deployment** 下方的 **Source** 選擇 `Deploy from a branch`。
4. 在 **Branch** 下拉選單中，將原本的 `None` 改選為 `main`，資料夾保持選擇 `/ (root)`。
5. 點選 **Save** 儲存設定。
6. 稍等 1-2 分鐘，重整該頁面，上方會出現一個綠色的提示框，顯示您的專案網址（格式通常為：`https://您的帳號.github.io/您的儲存庫名稱/`）。
7. 點選該連結即可在瀏覽器中直接開啟並運行本點燈網站！

---

## Firebase 即時資料庫 (Realtime Database) 設定指引

本網頁已內建 Firebase 串接邏輯。若要正式啟用資料儲存與即時天燈同步，請依序設定：

### 第一步：在 index.html 填入您的 Firebase 憑證
打開 `index.html`，尋找程式碼中的 `var firebaseConfig`（大約在第 233 行），將裡面的預設佔位符替換成您自己的 Firebase 憑證：
```javascript
var firebaseConfig = {
  apiKey: "您的 API 金鑰",
  authDomain: "您的專案ID.firebaseapp.com",
  databaseURL: "https://您的專案ID-default-rtdb.firebaseio.com",
  projectId: "您的專案ID",
  storageBucket: "您的專案ID.appspot.com",
  messagingSenderId: "您的傳送者ID",
  appId: "您的應用程式ID"
};
```
*備註：若維持預設佔位符不改，系統會自動切換為「預覽模擬模式」，表單提交時會進行本地飛燈模擬，讓您在沒有資料庫時也能測試效果。*

### 第二步：在 Firebase 控制台設定資料庫規則 (Database Rules)
為了讓網頁能讀寫資料，您需要在 Firebase 的 **Realtime Database** 的 **Rules**（規則）分頁中，允許公開讀寫（或依安全需求設定規則）：
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

### 第三步：資料結構
當有人送出點燈表單時，網頁會自動在資料庫的 `wishes` 路徑下以如下結構新增資料：
```json
{
  "wishes": {
    "-Nxxxxx(自動生成的ID)": {
      "a": "已在精舍學員姓名",
      "b": "朋友姓名",
      "w": "心願 (例如：身體健康)",
      "timestamp": 1698293810239
    }
  }
}
```
網頁端會自動監聽並讀取最新的 100 筆資料輪流播放，並且將 `1286` (基礎預設值) 加上資料庫中實際的願力筆數，作為大門口展示的總願力數字。

---

## 其他開發微調建議

1. **直式螢幕投影微調**：
   - 在實際投影幕或電視上播放時，可依據觀看距離調整天燈上的字體大小（font-size）以利閱讀。
