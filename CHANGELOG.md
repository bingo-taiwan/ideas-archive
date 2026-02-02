# 變更記錄

所有重要的專案變更都會記錄在這個檔案中。

格式基於 [Keep a Changelog](https://keepachangelog.com/zh-TW/1.0.0/)。

---

## [未發布]

### 規劃中
- 跨 IP 角色配對表設計
- Gemini Prompt 優化
- 品質自動檢測腳本
- LINE Bot 整合

---

## [v0.2] - 2026-02-02

### 新增
- **專案 4：Photo Backup Orchestrator**（雲端相片備份系統）
  - Google Photos 備份模組（OAuth 2.0 + Photos Library API）
  - iCloud Photos 備份模組（封裝 icloudpd，支援 2FA）
  - 智慧去重模組（SHA-256 hash 比對）
  - 自動歸檔功能（依拍攝日期整理）
  - 即時進度顯示（rich 套件）
  - SQLite 檔案索引
  - CLI 介面與 Windows 啟動腳本
  - 詳細文件：[photo-backup-orchestrator-pending.md](./photo-backup-orchestrator-pending.md)

### 技術細節
- Python 3.10+
- 核心依賴：google-api-python-client, icloudpd, rich, tqdm
- 程式碼位置：`C:\photo-backup-orchestrator`
- NAS 備份路徑：`X:\photos_archive\`

---

## [v0.1-alpha] - 2026-01-27

### 新增
- 專案文件（README.md、PENDING.md）
- 基礎專案結構規劃
- 火影忍者版本技術驗證（122 張卡片）

### 已完成技術驗證
- ✅ Gemini API 圖片生成整合
- ✅ Matplotlib 中文標籤後製
- ✅ 1:2 比例自動調整
- ✅ 檔案大小壓縮（< 300KB）
- ✅ 雙 API 並行生成（速率控制）

### 已知問題
- 🐛 Gemini 原子序號有時未顯示
- 🐛 底圖比例不穩定（1.50 ~ 2.99）
- 🐛 角色細節不夠精緻
- 🐛 背景色塊不夠清晰

### 技術細節
- Gemini 模型：`gemini-2.0-flash-exp-image-generation`
- API 速率限制：每 6.5 秒生成 1 張
- 輸出規格：512×1024px, JPEG, < 300KB
- 中文字體：Microsoft JhengHei

---

## 版本說明

- **v0.1-alpha**：概念驗證階段，單一 IP（火影忍者）
- **v0.2**（規劃中）：跨 IP 版本，10-15 個動漫 IP
- **v1.0**（規劃中）：LINE Bot 整合，題庫系統

---

**維護提醒**：
- 每次重大變更後更新此檔案
- 使用語義化版本號（Semantic Versioning）
- 記錄新增、變更、修復、移除的功能
