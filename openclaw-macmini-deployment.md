# OpenClaw Mac mini 部署記錄

> 2026-02-03 在 Mac mini 上成功部署 OpenClaw + LINE Bot

**專案狀態**：已完成基礎部署

---

## 環境資訊

| 項目 | 值 |
|------|-----|
| 主機 | Michelle的Mac mini (M 系列 ARM64) |
| macOS | 15.6 (Darwin 24.6.0) |
| SSH | michelle@192.168.1.95 (金鑰認證) |
| Node.js | 22.22.0 (via nvm) |
| OpenClaw | 2026.2.1 |
| Gateway | ws://127.0.0.1:18789 |

---

## 安裝歷程

### 1. 環境準備

Mac mini 沒有 Homebrew（安裝需要 sudo，非 TTY SSH 無法輸入密碼），改用 **nvm** 安裝 Node.js：

```bash
# 安裝 nvm（無需 sudo）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# 安裝 Node.js 22
source ~/.zshrc
nvm install 22
```

### 2. 安裝 OpenClaw

```bash
npm install -g openclaw
openclaw onboard  # 互動式設定（需在 Mac mini 終端執行）
```

### 3. 安裝 Bun + qmd

```bash
# 安裝 Bun
curl -fsSL https://bun.sh/install | bash

# 安裝 qmd（token 節省工具）
source ~/.zshrc
bun install -g github:tobi/qmd
```

---

## LINE Bot 設定

### Bot 資訊

| 項目 | 值 |
|------|-----|
| Bot 名稱 | 歐噴克勞 |
| Bot ID | @951brjwq |
| Channel ID | 2009038599 |
| Provider | bingo |

### 環境變數（~/.zshrc）

```bash
# OpenClaw LINE Bot
export LINE_CHANNEL_ACCESS_TOKEN="Lu+ij4qUTaugglhBwMYBqIe+XPahB5J2NYTqUmuraEykuj4p4Zy46II2C2K2ThlwFLpuDwzIWYFWXYFqyqgrve+0ENOfi1RjK0Ff0AkM8HWDveIhWeXl+lgLprp/W7KFwEku5Zra9EDCpFv+RWQsygdB04t89/1O/w1cDnyilFU="
export LINE_CHANNEL_SECRET="c75d7dcc805e9815c589580fee0d34b9"
```

### 啟動 Gateway

```bash
source ~/.zshrc
nohup openclaw gateway > /tmp/openclaw-gateway.log 2>&1 &
```

---

## SSH 金鑰認證

Windows 公鑰已加入 Mac mini：

```bash
# Windows 端公鑰位置
C:\Users\user\.ssh\id_rsa.pub

# 加入 Mac mini
ssh michelle@192.168.1.95
echo "ssh-rsa AAAAB3NzaC1yc2E... bingo@mynet.com.tw" >> ~/.ssh/authorized_keys
```

之後可免密碼連線：`ssh michelle@192.168.1.95`

---

## 待完成項目

| 項目 | 狀態 | 說明 |
|------|------|------|
| LaunchAgent | ✅ 已完成 | Gateway 開機自啟 |
| Claude Max 認證 | ⏸️ 等 3 天 | Token 回血後再設定 |
| Webhook URL | ⏸️ 待設定 | 需公開 URL（ngrok 或網域） |

---

## LaunchAgent 設定（已完成）

### plist 檔案位置

`~/Library/LaunchAgents/ai.openclaw.gateway.plist`

### 重點：環境變數必須寫在 plist 中

LaunchAgent 不會載入 `~/.zshrc`，所以 LINE 環境變數必須直接寫在 plist：

```xml
<key>EnvironmentVariables</key>
<dict>
    <key>PATH</key>
    <string>/Users/michelle/.nvm/versions/node/v22.22.0/bin:/usr/local/bin:/usr/bin:/bin</string>
    <key>HOME</key>
    <string>/Users/michelle</string>
    <key>LINE_CHANNEL_ACCESS_TOKEN</key>
    <string>your-token-here</string>
    <key>LINE_CHANNEL_SECRET</key>
    <string>your-secret-here</string>
</dict>
```

### 管理指令

```bash
# 載入
launchctl load ~/Library/LaunchAgents/ai.openclaw.gateway.plist

# 卸載
launchctl unload ~/Library/LaunchAgents/ai.openclaw.gateway.plist

# 查看日誌
tail -f /tmp/openclaw-gateway.log
```

### Claude Max 認證步驟（3 天後執行）

```bash
ssh michelle@192.168.1.95
source ~/.zshrc
openclaw models auth setup-token --provider anthropic
# 用 bingo@mynet.com.tw 登入
```

---

## 踩坑記錄

### 1. Homebrew 需要 sudo
**問題**：SSH 非 TTY 環境無法輸入 sudo 密碼
**解法**：改用 nvm 安裝 Node.js（無需 sudo）

### 2. 主機名稱解析失敗
**問題**：`MichelledeMac-mini.local` 有時無法解析
**解法**：改用 IP `192.168.1.95`

### 3. SSH 公鑰編碼問題
**問題**：Windows 的 id_rsa.pub 是 UTF-16 編碼，cat 輸出亂碼
**解法**：用 PowerShell `Get-Content -Encoding Unicode` 讀取

### 4. SSH 公鑰換行問題
**問題**：複製公鑰時被斷成多行，認證失敗
**解法**：用單行 echo 指令寫入

### 5. LINE 環境變數未被 Gateway 讀取
**問題**：用 nohup 啟動時環境變數沒載入
**解法**：啟動前必須 `source ~/.zshrc`

### 6. qmd 安裝方式
**問題**：誤以為是 npm 套件
**解法**：qmd 需用 Bun 安裝 `bun install -g github:tobi/qmd`

---

## 相關連結

- OpenClaw GitHub: https://github.com/openclaw/openclaw
- qmd GitHub: https://github.com/tobi/qmd
- LINE Developers Console: https://developers.line.biz/

---

**最後更新**：2026-02-03
