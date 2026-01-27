# 待辦事項與靈感記錄

> 📌 記錄所有 pending 的想法、待優化項目、未來計劃

**最後更新**：2026-01-27

---

## 🔥 優先級高（近期必做）

### 1. Gemini 圖片品質優化

**問題描述**：
- 原子序號經常未顯示（如 He 氦的測試案例）
- 角色細節不夠精緻
- 背景色塊不夠清晰

**可能解決方案**：

#### 方案 A：強化 Prompt 工程
```
當前 Prompt 問題：
- "ONLY in top-right corner" 不夠強制
- 需要更明確的視覺指令

改進方向：
- 加入 "CRITICAL: Atomic number MUST be visible and large"
- 使用更詳細的位置描述（百分比座標）
- 增加負面提示（"Do NOT omit the atomic number"）
```

#### 方案 B：改用更高階模型
```
選項：
- Gemini 2.5 Flash Image（配額更高）
- DALL-E 3（OpenAI）
- Midjourney API（需申請）
- Stable Diffusion XL（自架）

評估：
- 成本
- 品質
- 速度
- API 穩定性
```

#### 方案 C：分離生成 + 合成
```
流程：
1. Gemini 生成角色立繪（純角色，無元素背景）
2. Gemini 生成元素背景（純色 + 符號 + 數字）
3. PIL/OpenCV 合成兩張圖
4. Matplotlib 加中文標籤

優點：
- 更好的控制品質
- 元素區域可以程式化生成（100% 準確）

缺點：
- 流程更複雜
- 需要額外的合成邏輯
```

**待決定**：選擇哪個方案？
**截止日**：2026-01-31

---

### 2. 跨 IP 角色配對表設計

**目標**：將 118 元素配對到各大動漫 IP

**待決定的配對方案**：

#### 方案 A：依元素特性選 IP
```
優點：
- 配對有邏輯性
- 教育意義較高

缺點：
- 某些 IP 會分配較少元素
- 配對規則複雜
```

#### 方案 B：IP 輪替均分
```
優點：
- 每個 IP 都有代表
- 設計工作量平均

缺點：
- 配對可能較牽強
- 失去元素特性關聯
```

#### 方案 C：最佳角色優先
```
優點：
- 每個元素都是最佳配對
- 品質最高

缺點：
- 某些 IP 可能過度集中
- 需要深入研究每個元素特性
```

**待決定**：選擇哪個方案？
**依賴**：需先列出候選 IP 清單

---

### 3. 動漫 IP 清單確定

**待選擇的 IP**（依知名度排序）：

#### Tier 1（必選）
- [ ] 海賊王（One Piece）
- [ ] 火影忍者（Naruto）
- [ ] 鬼滅之刃（Demon Slayer）
- [ ] 咒術迴戰（Jujutsu Kaisen）
- [ ] 進擊的巨人（Attack on Titan）

#### Tier 2（考慮選入）
- [ ] 龍珠（Dragon Ball）
- [ ] 鋼之鍊金術師（Fullmetal Alchemist）
- [ ] 死神（Bleach）
- [ ] 一拳超人（One Punch Man）
- [ ] 我的英雄學院（My Hero Academia）
- [ ] 東京喰種（Tokyo Ghoul）
- [ ] 約定的夢幻島（The Promised Neverland）

#### Tier 3（可選）
- [ ] 獵人（Hunter x Hunter）
- [ ] 七龍珠（Dragon Ball）
- [ ] JoJo 的奇妙冒險
- [ ] 電鋸人（Chainsaw Man）
- [ ] 間諜家家酒（Spy x Family）
- [ ] 排球少年（Haikyuu）

**待決定**：最終選擇哪些 IP？預計選 10-15 個
**截止日**：2026-01-30

---

## 💡 中優先級（一個月內）

### 4. 原子序號後製方案

**背景**：Gemini 經常遺漏原子序號

**解決方案**：用程式後製加上數字

```python
from PIL import Image, ImageDraw, ImageFont

def add_atomic_number(image_path, atomic_number):
    """
    在右上角加上原子序號
    """
    img = Image.open(image_path)
    draw = ImageDraw.Draw(img)

    # 字體設定
    font = ImageFont.truetype("arial.ttf", 48)

    # 位置：右上角（留 10% 邊距）
    width, height = img.size
    x = width * 0.85
    y = height * 0.42  # 在元素區域的右上角

    # 繪製白色數字（黑色外框）
    text = str(atomic_number)
    draw.text((x, y), text, font=font, fill="white", stroke_width=2, stroke_fill="black")

    img.save(image_path)
```

**待實作**：整合到生成流程

---

### 5. 品質自動檢測

**需求**：自動檢測生成圖片的品質問題

```python
def check_image_quality(image_path):
    """
    檢查圖片品質

    檢查項目：
    1. 比例是否為 1:2
    2. 檔案大小是否 < 300KB
    3. 解析度是否適當
    4. 是否有明顯瑕疵（全黑/全白）
    """
    issues = []

    img = Image.open(image_path)
    width, height = img.size
    ratio = height / width

    # 檢查比例
    if abs(ratio - 2.0) > 0.1:
        issues.append(f"比例不正確: {ratio:.2f} (應為 2.00)")

    # 檢查檔案大小
    size_kb = os.path.getsize(image_path) / 1024
    if size_kb > 300:
        issues.append(f"檔案過大: {size_kb:.1f}KB (應 < 300KB)")

    # 檢查解析度
    if width < 400 or height < 800:
        issues.append(f"解析度過低: {width}x{height}")

    return issues
```

**待實作**：批次檢測腳本

---

### 6. LINE Bot Flex Message 設計

**需求**：設計適合 LINE Bot 的卡片展示方式

**Flex Message 結構**：
```json
{
  "type": "bubble",
  "hero": {
    "type": "image",
    "url": "https://example.com/cards/001_H.jpg",
    "size": "full",
    "aspectRatio": "1:2",
    "aspectMode": "cover"
  },
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      {
        "type": "text",
        "text": "氫 (H) #1",
        "weight": "bold",
        "size": "xl"
      },
      {
        "type": "text",
        "text": "角色：魯夫",
        "size": "sm",
        "color": "#999999"
      }
    ]
  },
  "footer": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      {
        "type": "button",
        "action": {
          "type": "message",
          "label": "查看詳情",
          "text": "元素 #1"
        }
      }
    ]
  }
}
```

**待實作**：
- [ ] Flex Message JSON 模板
- [ ] LINE Bot webhook 處理
- [ ] 圖片 CDN 部署

---

### 7. 題庫系統整合

**概念**：將元素卡片變成互動題庫

**題型範例**：
1. **看圖猜元素**：顯示角色圖，選擇元素
2. **看元素猜角色**：顯示元素符號，選擇角色
3. **特性配對**：元素特性與角色能力配對
4. **族群分類**：將角色分類到正確的元素族群

**待設計**：
- [ ] 題目資料結構
- [ ] 答案驗證邏輯
- [ ] 計分系統

---

## 🌟 低優先級（未來考慮）

### 8. 動畫效果

**想法**：卡片翻轉動畫（正面顯示角色，背面顯示元素資訊）

**技術方案**：
- CSS3 flip animation
- 或生成兩張圖（正反面）

---

### 9. 多語言支援

**語言**：
- 繁體中文（預設）
- 簡體中文
- 英文
- 日文

**待設計**：
- 多語言資料結構
- 字型支援

---

### 10. NFT / 數位收藏品

**想法**：將卡片鑄造為 NFT

**考慮因素**：
- 版權問題（動漫角色）
- 區塊鏈選擇（Ethereum / Polygon）
- 智能合約設計

---

### 11. 實體卡片印製

**想法**：製作實體收藏卡

**規格考量**：
- 尺寸：標準撲克牌尺寸（63×88mm）
- 材質：特殊卡紙
- 印刷：高品質彩色印刷

---

### 12. AR 擴增實境

**想法**：掃描實體卡片顯示 3D 角色

**技術**：
- AR.js / AR Core
- 3D 模型資源

---

## 🤔 待討論的概念問題

### 問題 1：版權考量

**疑問**：使用動漫角色是否有版權問題？

**考慮**：
- 僅作為個人學習用途
- 標註來源與版權聲明
- 不進行商業販售
- 或改用「致敬」方式，不直接使用官方圖

---

### 問題 2：教育價值定位

**疑問**：這個專案的教育意義是什麼？

**可能方向**：
1. **化學教育**：用動漫吸引學生學習化學
2. **動漫文化**：整理各 IP 代表性角色
3. **跨領域整合**：科學與藝術結合

---

### 問題 3：目標受眾

**疑問**：這個專案的目標使用者是誰？

**可能受眾**：
- 國高中生（化學學習）
- 動漫愛好者（收藏）
- 教師（教學素材）
- 一般大眾（娛樂）

---

## 📅 時間軸規劃

### 2026-01 末

- [x] 技術驗證（Gemini + Matplotlib）
- [x] 火影忍者版本（122 張已生成）
- [ ] 品質問題分析
- [ ] 決定配對方案

### 2026-02

- [ ] 跨 IP 角色配對表完成
- [ ] Gemini Prompt 優化
- [ ] 批次生成跨 IP 版本

### 2026-03

- [ ] LINE Bot 整合
- [ ] 題庫系統開發
- [ ] 使用者測試

### 2026-04+

- [ ] 根據反饋優化
- [ ] 考慮其他應用方向

---

## 💬 即時想法記錄區

> 這區用來快速記錄臨時的靈感，之後再整理到上面的分類

### 2026-01-27

- Gemini 品質不夠好，考慮改用 DALL-E 3 或 Midjourney
- 跨 IP 版本比單一 IP 更有趣
- 可以做成互動式網頁，讓使用者投票最佳配對
- 考慮加入「元素小知識」欄位
- 可以設計「收集進度」功能（類似寶可夢圖鑑）

---

**維護提醒**：
- 定期檢視這個檔案
- 完成的項目移到 CHANGELOG.md
- 新想法隨時加到「即時想法記錄區」
