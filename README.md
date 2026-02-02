# 靈感記錄倉庫

> 記錄各種專案靈感、pending 的想法、待優化項目、未來計劃

**最後更新**：2026-02-02

---

## 📁 目前專案

### 1. 動漫元素週期表卡片專案

將化學元素週期表的 118 個元素，與**各大動漫 IP 的代表性角色**進行配對，生成高質感的卡片圖片，適用於 LINE Bot、教育內容或收藏。

**專案狀態**：v0.1-alpha（概念驗證階段）

**詳細文件**：
- [PENDING.md](./PENDING.md) - 待辦事項與靈感記錄
- [CHANGELOG.md](./CHANGELOG.md) - 版本變更記錄

---

### 2. Auruma Server Linux 遷移計劃

將辦公室 Auruma-Server 從 Windows Server 2016 遷移到 Linux Ubuntu，並安裝各種應用與服務（包含 Clawdbot）。

**專案狀態**：規劃階段

**詳細文件**：
- [auruma-linux-migration-pending.md](./auruma-linux-migration-pending.md) - 遷移計劃與待辦事項

**主要目標**：
- 遷移到 Ubuntu Server 24.04 LTS
- 安裝 Clawdbot（個人 AI 助手平台）
- 部署 Docker + PostgreSQL + Nginx
- 設定監控與備份系統

**預計時間**：2026-02 開始規劃，2026-03 執行遷移

---

### 3. Clawdbot 商業應用 AI 機器人

使用 Clawdbot v2026.1.24 建立小規模商業應用的 AI 機器人，評估 Claude Max vs 開源模型（Ollama + RTX 3090）兩種技術方案。

**專案狀態**：方案評估階段

**詳細文件**：
- [clawdbot-commercial-bot-pending.md](./clawdbot-commercial-bot-pending.md) - 方案評估與計劃

**核心決策**：
- **方案 A**：Claude Max（雲端，月費 NT$ 650）
- **方案 B**：Ollama + RTX 3090（本地，硬體投資約 NT$ 44,000）
- **方案 C**：混合方案（彈性切換，推薦）

**商業應用場景**：
- LINE Bot 客服系統
- 企業內部助手（Slack/Telegram）
- 自動化工作流

**預計時間**：2026-02 決定方案，2026-03 開始部署

---

### 4. 雲端相片備份系統 (Photo Backup Orchestrator)

將 Google Photos 和 iCloud Photos 的相片與影片自動備份到 Synology NAS，提供去重與歸檔功能。

**專案狀態**：✅ v1.0 基礎架構已完成

**詳細文件**：
- [photo-backup-orchestrator-pending.md](./photo-backup-orchestrator-pending.md) - 專案說明與待辦事項

**核心功能**：
- Google Photos 備份（OAuth 2.0 + Photos Library API）
- iCloud Photos 備份（icloudpd + 2FA）
- SHA-256 智慧去重
- 依拍攝日期自動歸檔
- 即時進度顯示（rich 套件）
- SQLite 檔案索引

**程式碼位置**：`C:\photo-backup-orchestrator`

**NAS 備份路徑**：`X:\photos_archive\`

---

## 🎨 專案 1 詳細資訊：動漫元素卡片

#### 🎯 核心理念

- **跨 IP 整合**：不限於單一動漫，涵蓋所有經典動漫作品
- **代表性角色**：每個元素配對該 IP 最具代表性的角色
- **教育娛樂**：結合化學知識與動漫文化
- **高質感設計**：適合手機顯示，檔案大小 < 300KB

---

## 🎨 設計規格

### 圖片規格

| 項目 | 規格 | 說明 |
|------|------|------|
| **長寬比** | 1:2 (寬:高) | 例：512×1024px |
| **檔案格式** | JPEG | 高壓縮率 |
| **檔案大小** | < 300KB | LINE Bot 限制 |
| **DPI** | 120 | 手機顯示優化 |
| **字體** | Microsoft JhengHei | 中文正體 |

### 卡片結構

```
┌─────────────────┐
│                 │
│   角色圖片      │  60% - 動漫角色立繪/場景
│   (AI 生成)     │
│                 │
├─────────────────┤
│ 元素符號 + 序號 │  20% - Gemini 生成（帶顏色背景）
├─────────────────┤
│  中文標籤區     │  20% - Matplotlib 後製
│  - 元素名稱     │
│  - 族群         │
│  - 陰電性       │
│  - 角色名稱     │
│  - 特性關聯     │
└─────────────────┘
```

---

#### 📊 目前進度

**✅ 已完成**
- 技術驗證：Gemini API 圖片生成
- 技術驗證：Matplotlib 中文標籤
- 技術驗證：1:2 比例調整
- 技術驗證：檔案大小壓縮 (< 300KB)
- 批次生成腳本（雙 API 並行）
- 速率限制控制（避免 API quota）
- 測試案例：火影忍者 118 張（122 張已生成）

**🔨 待優化**
- Gemini 圖片品質問題（原子序號遺漏、比例不穩定）
- 角色與元素配對邏輯設計
- 跨 IP 配對規則制定

---

## 📝 如何使用這個倉庫

### 新增專案靈感

1. 建立新的 Markdown 檔案（例如：`project-name-pending.md`）
2. 記錄專案概念、待辦事項、靈感
3. Commit 並 push 到 GitHub

### 更新現有專案

1. 修改對應的 Markdown 檔案
2. 在 CHANGELOG.md 記錄重大變更
3. Commit 並 push

---

## 🗂️ 檔案結構

```
ideas-archive/
├── README.md                              # 本檔案（倉庫總覽）
├── PENDING.md                             # 專案 1：動漫元素卡片 - 待辦事項
├── CHANGELOG.md                           # 專案 1：動漫元素卡片 - 版本記錄
├── auruma-linux-migration-pending.md     # 專案 2：Auruma Linux 遷移計劃
├── clawdbot-commercial-bot-pending.md    # 專案 3：Clawdbot 商業機器人方案評估
├── photo-backup-orchestrator-pending.md  # 專案 4：雲端相片備份系統
├── PUSH_TO_GITHUB.md                     # GitHub 推送指南
└── .gitignore                            # Git 忽略規則
```

未來會新增更多專案的 pending 文件。

---

## 🔨 目前待優化項目總覽

### 專案 1：動漫元素卡片

**詳見**：[PENDING.md](./PENDING.md)

主要待辦：
- Gemini 圖片品質優化（原子序號遺漏問題）
- 跨 IP 角色配對表設計（3 種方案待決定）
- 動漫 IP 清單確定（10-15 個 IP）

### 專案 2：Auruma Server Linux 遷移

**詳見**：[auruma-linux-migration-pending.md](./auruma-linux-migration-pending.md)

主要待辦：
- 完成遷移計劃細節
- 備份所有資料
- 決定郵件服務處理方式
- 決定資料庫遷移方案
- 選定安裝時間並執行遷移

### 專案 3：Clawdbot 商業 AI 機器人

**詳見**：[clawdbot-commercial-bot-pending.md](./clawdbot-commercial-bot-pending.md)

主要待辦：
- 決定技術方案（Claude Max / Ollama / 混合）
- 評估 Auruma 硬體相容性（RTX 3090）
- 採購硬體（如選擇方案 B/C）
- 安裝 Clawdbot v2026.1.24
- 設定訊息平台整合（LINE/Telegram/Slack）
- 開發商業功能與整合

### 專案 4：雲端相片備份系統

**詳見**：[photo-backup-orchestrator-pending.md](./photo-backup-orchestrator-pending.md)

主要待辦：
- 設定 Google Cloud 專案並取得 OAuth 憑證
- 完成首次 Google Photos 同步測試
- 完成首次 iCloud 認證與同步測試
- 設定 Windows Task Scheduler 定時執行

---

## 💡 專案細節：動漫元素卡片

### 配對方案思路

#### 方案 A：依元素特性選擇 IP

| 元素族群 | 特性 | 適合的動漫 IP |
|---------|------|--------------|
| **鹼金屬** | 爆炸、活潑 | 《咒術迴戰》爆發系角色 |
| **鹼土金屬** | 堅固、防禦 | 《進擊的巨人》硬化能力 |
| **過渡金屬** | 金屬、武器 | 《鋼之鍊金術師》煉金術 |
| **鹵素** | 腐蝕、毒 | 《鬼滅之刃》毒系血鬼術 |
| **惰性氣體** | 穩定、超然 | 《一拳超人》琦玉 |
| **鑭系/錒系** | 稀有、強大 | 《龍珠》賽亞人 |

#### 方案 B：IP 輪替分配

將 118 個元素**均勻分配**給不同動漫 IP：

| IP | 元素數量 | 代表元素 |
|----|---------|---------|
| **火影忍者** | 20 | 1-20 |
| **海賊王** | 20 | 21-40 |
| **鬼滅之刃** | 15 | 41-55 |
| **咒術迴戰** | 15 | 56-70 |
| **進擊的巨人** | 10 | 71-80 |
| **鋼之鍊金術師** | 10 | 81-90 |
| **龍珠** | 10 | 91-100 |
| **一拳超人** | 8 | 101-108 |
| **死神** | 10 | 109-118 |

#### 方案 C：最具代表性角色優先

每個元素選擇**最符合該元素特性**的動漫角色，不限 IP：

**範例**：
- **H (氫)**：魯夫（海賊王）- 最輕、最多、橡膠彈性
- **He (氦)**：琦玉（一拳超人）- 穩定、無敵
- **C (碳)**：愛德華（鋼鍊）- 有機生命基礎
- **Fe (鐵)**：艾倫（進擊）- 鋼鐵意志
- **Au (金)**：吉連（龍珠）- 最強、最珍貴
- **U (鈾)**：魔人普烏（龍珠）- 毀滅級力量

**詳細配對方案請見**：[PENDING.md](./PENDING.md)

---

## 🗂️ 技術細節：資料結構設計

### 元素資料格式

```json
{
  "atomic_number": 1,
  "symbol": "H",
  "name": "氫",
  "name_en": "Hydrogen",
  "group": "1A族（鹼金屬）",
  "electronegativity": "2.20",
  "color": "#FF6B6B",

  "character": {
    "name": "魯夫",
    "name_en": "Luffy",
    "anime_ip": "海賊王",
    "anime_ip_en": "One Piece",
    "description": "草帽海賊團船長，橡膠果實能力者",
    "key_feature": "最輕、最多、彈性無限",
    "prompt_description": "Luffy with straw hat, red vest, smiling, Gear 5 white hair form"
  }
}
```

**詳細專案結構請見**：[README.md § 專案結構](./PENDING.md)（規劃於 PENDING.md）

---

## 🚀 未來規劃

- 動漫元素卡片專案持續優化
- 更多專案靈感會陸續加入此倉庫
- 當某個專案成熟後，會獨立成專屬 repository

---

## 💡 為什麼需要這個倉庫？

- 💭 **記錄想法**：避免好點子遺忘
- 📋 **追蹤進度**：清楚知道每個專案的狀態
- 🔄 **多地同步**：家裡、公司都能存取
- 📚 **經驗累積**：未來可以回顧當初的靈感

---

**維護提醒**：
- 定期檢視這個倉庫
- 完成的項目移到 CHANGELOG
- 新想法隨時加入 PENDING
- 保持文件更新

---

**最後更新**：2026-02-02
