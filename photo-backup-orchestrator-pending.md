# 雲端相片備份系統 - Photo Backup Orchestrator

> 將 Google Photos 和 iCloud Photos 自動備份到 Synology NAS

**專案狀態**：✅ v1.0 已完成基礎架構
**最後更新**：2026-02-02
**程式碼位置**：`C:\photo-backup-orchestrator`

---

## 📋 專案概述

建立一套自動化系統，統一管理 Google Photos 和 iCloud Photos 的備份，並提供去重與歸檔功能。

### 核心功能

| 功能 | 狀態 | 說明 |
|------|------|------|
| Google Photos 備份 | ✅ 完成 | OAuth 2.0 授權，Photos Library API |
| iCloud Photos 備份 | ✅ 完成 | 封裝 icloudpd，支援 2FA |
| 智慧去重 | ✅ 完成 | SHA-256 hash 比對 |
| 自動歸檔 | ✅ 完成 | 依拍攝日期整理 |
| 進度顯示 | ✅ 完成 | rich 套件美化輸出 |
| SQLite 索引 | ✅ 完成 | 記錄已處理檔案 |

---

## 📁 專案結構

```
C:\photo-backup-orchestrator\
├── config/
│   ├── settings.yaml           # 主設定檔
│   ├── settings.example.yaml   # 設定範本
│   └── credentials.json        # Google OAuth 憑證（需自行下載）
├── src/
│   ├── main.py                 # CLI 入口
│   ├── google_photos_backup.py # Google Photos 下載模組
│   ├── icloud_photos_backup.py # iCloud Photos 下載模組
│   ├── dedupe_and_archive.py   # 去重與歸檔模組
│   ├── progress_logger.py      # 進度顯示與統計
│   ├── file_index.py           # SQLite 檔案索引
│   └── utils.py                # 共用工具函式
├── data/
│   ├── token.json              # Google OAuth token（自動產生）
│   ├── file_index.db           # SQLite 檔案索引
│   └── icloud_session/         # iCloud session 快取
├── logs/                       # 執行日誌
├── venv/                       # Python 虛擬環境
├── requirements.txt
├── run_backup.bat              # Windows 快速啟動（CMD）
├── run_backup.ps1              # Windows 快速啟動（PowerShell）
└── README.md
```

---

## 📦 NAS 備份目錄結構

```
X:\photos_archive\
├── source_google\          # Google Photos 原始下載
│   ├── 2024\
│   │   ├── 2024-01\
│   │   └── 2024-02\
│   └── 2025\
├── source_icloud\          # iCloud Photos 原始下載
│   └── ...
├── master\                 # 去重後的主檔案庫
│   └── 2024\
│       └── 2024-01-15\
│           ├── IMG_1234.jpg
│           └── ...
└── _duplicates\            # 重複檔案（可手動刪除）
    ├── from_google\
    └── from_icloud\
```

---

## 🔧 技術選擇

### 語言：Python 3.10+

**選擇理由**：
- Google Photos API 有完整的 Python SDK
- icloudpd 是 Python 套件，可直接整合
- 檔案處理、hash 計算生態系成熟
- SQLite 內建支援

### 核心依賴

```
google-auth>=2.0.0
google-auth-oauthlib>=1.0.0
google-api-python-client>=2.0.0
icloudpd>=1.17.0
tqdm>=4.65.0
rich>=13.0.0
pyyaml>=6.0
python-dateutil>=2.8.0
```

---

## 🚀 使用方式

### 快速開始

```powershell
cd C:\photo-backup-orchestrator
.\run_backup.ps1 --all              # 完整備份
.\run_backup.ps1 --sync-google      # 只同步 Google
.\run_backup.ps1 --sync-icloud      # 只同步 iCloud
.\run_backup.ps1 --dedupe           # 只執行去重
.\run_backup.ps1 --stats            # 查看統計
```

### 測試模式

```powershell
.\run_backup.ps1 --sync-google --limit 10   # 測試下載 10 張
.\run_backup.ps1 --dedupe --dry-run         # 預覽去重結果
```

---

## ⏳ 待辦事項

### 高優先級（近期）

- [ ] 設定 Google Cloud 專案並取得 OAuth 憑證
- [ ] 完成首次 Google Photos 同步測試
- [ ] 完成首次 iCloud 認證與同步測試
- [ ] 設定 Windows Task Scheduler 定時執行

### 中優先級（一個月內）

- [ ] 增加斷點續傳功能（大量檔案時中斷後續傳）
- [ ] 增加網路錯誤重試機制
- [ ] 增加 Email 通知功能（備份完成/失敗時通知）
- [ ] 增加 LINE Notify 通知功能

### 低優先級（未來考慮）

- [ ] Web UI 管理介面
- [ ] 支援更多雲端服務（OneDrive、Dropbox）
- [ ] 支援影片縮圖預覽
- [ ] 支援 EXIF 資訊解析與顯示

---

## 📝 設定說明

### Google Cloud 設定步驟

1. 前往 [Google Cloud Console](https://console.cloud.google.com/)
2. 建立新專案
3. 啟用 **Photos Library API**
4. 建立 OAuth 2.0 憑證（Desktop app）
5. 設定 OAuth 同意畫面，新增自己為 Test user
6. 下載憑證 JSON，放到 `config/credentials.json`

### iCloud 設定步驟

1. 編輯 `config/settings.yaml`，填入 Apple ID
2. 執行 `.\run_backup.ps1 --icloud-auth`
3. 輸入密碼和 2FA 驗證碼

---

## 💡 開發筆記

### 2026-02-02

- 完成基礎架構開發
- 所有模組已實作並通過單元測試
- 依賴套件已安裝到虛擬環境
- NAS 備份目錄結構已建立

### 已知限制

1. **Google Photos API 限制**：
   - 無法取得原始檔案名稱的完整路徑
   - 無法取得相簿資訊（需額外 API 呼叫）
   - 需要定期重新整理 OAuth token

2. **iCloud 限制**：
   - 需要定期重新驗證 2FA
   - session 可能過期

3. **效能考量**：
   - 大量檔案時 hash 計算較慢
   - 建議分批執行（使用 --limit 參數）

---

## 🔗 相關資源

- [Google Photos Library API 文件](https://developers.google.com/photos/library/guides/overview)
- [icloudpd GitHub](https://github.com/icloud-photos-downloader/icloud_photos_downloader)
- [rich 套件文件](https://rich.readthedocs.io/)

---

**維護提醒**：
- 定期檢查 OAuth token 是否過期
- 定期檢查 iCloud session 是否有效
- 定期清理 `_duplicates` 目錄
