# 推送到 GitHub 指南

## 步驟 1：在 GitHub 建立新 repository

1. 前往 https://github.com/new
2. Repository name: `anime-element-cards`
3. Description: `動漫元素週期表卡片專案 - 將 118 元素與各大動漫 IP 代表性角色配對`
4. **不要**勾選 "Initialize this repository with a README"（我們已經有了）
5. 點選 "Create repository"

---

## 步驟 2：推送本地 repository 到 GitHub

在終端機執行以下指令：

```bash
cd "C:\Users\user\Documents\temp\anime-element-cards"

# 加入遠端 repository（請替換成你的 GitHub 使用者名稱）
git remote add origin https://github.com/YOUR_USERNAME/anime-element-cards.git

# 推送到 GitHub
git push -u origin master
```

---

## 步驟 3：驗證推送成功

前往 https://github.com/YOUR_USERNAME/anime-element-cards 確認以下檔案已上傳：
- ✅ README.md
- ✅ PENDING.md
- ✅ CHANGELOG.md
- ✅ .gitignore

---

## 未來更新流程

當你修改檔案後，使用以下指令同步到 GitHub：

```bash
cd "C:\Users\user\Documents\temp\anime-element-cards"

# 檢查修改狀態
git status

# 加入修改的檔案
git add .

# 建立 commit（請用有意義的訊息）
git commit -m "描述你的修改內容"

# 推送到 GitHub
git push
```

---

## 常用 Git 指令

```bash
# 查看 commit 歷史
git log --oneline

# 查看目前狀態
git status

# 查看修改內容
git diff

# 復原未 commit 的修改
git checkout -- filename

# 查看遠端 repository
git remote -v
```

---

## 注意事項

⚠️ **不要推送敏感資訊**：
- API Keys（已在 .gitignore 排除）
- 輸出圖片（檔案太大，已排除）
- 個人帳號資訊

✅ **建議推送的內容**：
- 文件（README、PENDING、CHANGELOG）
- 程式碼（腳本、設定檔）
- 資料結構設計（JSON schema）

---

**完成後，你就可以在任何地方（家裡、公司）存取這個專案的文件和靈感記錄了！** 🎉
