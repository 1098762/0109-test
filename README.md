# 🏙️ 大邱房地產資料探勘期末專案報告 (Daegu Real Estate)

本專案旨在透過資料探勘技術，分析韓國大邱市房地產數據，預測房屋售價等級並進行市場分群。

---

## 👤 專案基本資訊
* **授課教師**：[請填入教授姓名]
* **填表人（自己）**：[系級] 翁敬鈞 409330636
* **被評分人（同學）**：[系級] [姓名] [學號]
* **隨機種子 (Random State)**：`20250102`

---

## 📺 報告與資源連結
* **[YouTube 報告影片]**：(請在此貼上您的影片連結，需含英文字幕)
* **[Google Colab 筆記本]**：(請在此貼上您的 Colab 專案網址)
* **[專案簡報 PPT]**：(請確認檔案已上傳至本儲存庫)

---

## 📊 1. 原始資料集描述 (Data Meta Data)
* **資料來源**：Kaggle - Daegu Apartment Sales
* **資料網址**：[https://www.kaggle.com/datasets/readwithvinay/daegu-apartment-sales-dataset]
* **資料型態**：CSV 格式
* **資料筆數**：5,891 筆
* **變數總個數**：30 個變數 (含目標變數)
* **目標變數 (Target)**：`SalePrice` (售價)
  * *註：為符合分類任務，已將 SalePrice 離散化處理。*

### 變數定義與原始型態 (部分列舉)
| 變數名稱 | 定義 | 資料型態 | 是否遺失值 |
| :--- | :--- | :--- | :--- |
| SalePrice | 房屋成交價 (目標變數) | int64 | 無 |
| YearBuilt | 建築年份 | int64 | 無 |
| Size(sqf) | 坪數 (平方英尺) | int64 | 無 |
| HallwayType | 走廊類型 (corridor/terraced...) | object | 無 |
| SubwayStation | 最近的地鐵站名稱 | object | 無 |

---

## 🧹 2. 資料預處理與清理 (Data Preprocessing)
* **清理理由**：
    * 移除與房價無關之行政編號。
    * 針對地理特徵進行簡化，避免過度維度擴張。
* **衍生變數**：新增 `Apt_Age` (屋齡)，透過 `YrSold` - `YearBuilt` 計算而得。
* **遺失值處理**：本資料集完整度高，如有缺失採用眾數/平均值填補。
* **離散化理由**：為了進行多類別預測，將 `SalePrice` 以分位數切分為三類 (Low, Medium, High)。

---

## 🤖 3. 模型分析結果 (Modeling & Comparison)
所有模型均採用 **80% 訓練 / 20% 測試** 切分。

### 分類模型效能總表 (random_state=20250102)
| 評分項目 | 最佳參數組合 | 訓練正確率 | 測試正確率 |
| :--- | :--- | :--- | :--- |
| **決策樹 (DT)** | Depth: 8, Entropy, M=0.01 | 0.XX | 0.XX |
| **SVM (SVC)** | Kernel: Linear, C: 1.0 | 0.XX | 0.XX |
| **隨機森林 (RF)** | Trees: 100, MaxFeatures: Auto | 0.XX | 0.XX |
| **KNN** | Best K: [填入你的 K] | 0.XX | 0.XX |
| **Voting (Best)** | Hard Voting (DT+RF+SVM) | 0.XX | 0.XX |

### 模型關鍵發現
1. **決策樹**：透過變數挑選 (Model Selection) 發現 `Size` 與 `SubwayStation` 為最關鍵分類特徵。
2. **SVM/KNN**：必須進行 **Standardization (標準化)**，否則 `Size` 的量綱會主導距離計算。
3. **RF**：隨機森林在處理非線性關聯上表現最優，為本次建議之最佳模型。

---

## 🔍 4. 非監督式學習 (Unsupervised Learning)
* **K-means**：利用陡坡圖 (Elbow Method) 決定 K 值，將大邱房產分為 [X] 群，分別命名為「核心商圈房」、「郊區平價房」等。
* **關聯法則**：透過 Apriori 演算法找出變數間關聯，例如：{Subway_Station=Daegu} => {High_Price}。

---

## 📝 5. 綜合結論
本分析透過五種模型驗證，發現大邱房價受 **[關鍵特徵]** 影響最劇。建議採用 **[你的最佳模型]** 作為未來預測工具，其交叉驗證 (CV=5) 之正確率穩定維持在 0.XX。

---
