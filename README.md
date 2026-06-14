# 💳 Moneybook Dashboard

個人財務儀表板 — 整合 Moneybook 消費記錄 + 元大證券 + 國泰證券，純前端單頁 HTML，資料存於 Google Drive。

**正式版**：https://qoqstor.github.io/moneybook-dashboard

---

## ✨ 功能

### 💰 消費分析
- 自動讀取 Drive 最新 Moneybook 明細 CSV
- 年度趨勢圖、月度熱圖、類別分析、月份詳解

### 📊 投資組合（國泰證券）
- 登入時自動從 Gmail【AI投資日報】同步最新持股
- DOM 解析 HTML，不依賴 AI API key
- 顯示持股價格、漲跌、訊號、除息事件

### 🏦 元大歷史對帳單
- 登入時自動從 Gmail 掃描未同步月份
- PDF.js 前端解密（密碼存 session）
- 各月份市值、成本、損益、持股明細全展開

### 📡 盤前即時行情
- Yahoo Finance API 顯示加權指數 + 持股即時報價

---

## 🚀 快速開始

### 前置需求
1. **Google Cloud Console** 建立 OAuth 2.0 Client ID
   - 授權 JS 來源加入你的網頁網址
   - 啟用 Google Drive API、Gmail API
2. **Google Drive** 建立資料夾，上傳 Moneybook CSV
3. **Gemini API Key**（選用）— 申請：https://aistudio.google.com，模型：`gemini-2.5-flash`

### 首次使用
1. 開啟網頁 → 點「還沒帳號？註冊」
2. 填入 Email、密碼、OAuth Client ID、Drive 資料夾 ID、Gemini API Key
3. 完成 Google 授權
4. 上傳 Moneybook 明細 CSV 到 Drive 資料夾

### 日常使用
- **每月**：上傳新的 Moneybook CSV 到 Drive
- **每天**：登入網頁，自動同步國泰日報和元大對帳單

---

## 📁 Drive 資料夾結構

```
你的資料夾/
├── Moneybook_明細_YYYYMMDD_1.csv  ← 手動上傳
├── portfolio.json                  ← 自動生成/更新
├── yuanta_history.json             ← 自動生成/更新
└── Yuanta_PDF/
    └── *.pdf                       ← 自動備份
```

---

## 🔧 技術棧

| 套件 | 用途 |
|------|------|
| Firebase Auth + Firestore | 帳號管理、AES-256 加密儲存憑證 |
| Chart.js 4.4 | 圖表 |
| PapaParse 5.4 | CSV 解析 |
| PDF.js 3.11.174 | PDF 前端解密 |
| Google Identity Services | OAuth 登入 |
| Google Drive API | 讀寫資料 |
| Gmail API | 自動抓取對帳單 |
| Yahoo Finance API | 即時股價 |
| Gemini 2.5 Flash（選用） | AI 解析 fallback |

---

## 🔒 安全性

- OAuth Client ID、Drive 資料夾 ID、Gemini API Key 均以 **AES-256-GCM** 加密後存 Firestore
- 加密 key 由用戶登入密碼派生（PBKDF2），伺服器無法讀取原始值
- PDF 密碼僅存於瀏覽器 sessionStorage，頁面關閉即清除

---

## 📝 開發

詳細開發文件請見 [HANDOFF.md](./HANDOFF.md)。

### 分支說明
- `main`：正式版（v1.4.1）
- `dev`：開發版（v1.4.1-dev），含 DEV banner
