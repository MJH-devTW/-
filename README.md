# 標準圖 V1.6 - ZigZag 趨勢交易策略

此策略基於 ZigZag 趨勢分析，透過多空信號偵測和風險管理工具，提供高效的自動化交易解決方案。

---

## Input 參數介紹

以下是此策略中使用的主要 `input` 參數，供用戶進行個性化設置：

### 基本參數
- `zigzag_len`  
  **描述**：ZigZag 長度，控制 ZigZag 分析的區間長度。  
  **類型**：整數 (int)  
  **預設值**：6  
  **用途**：調整趨勢轉折點的靈敏度。

- `peakarea`  
  **描述**：第一個尖點到過去進場觀察區的上下比例。  
  **類型**：浮點數 (float)  
  **預設值**：0.15  
  **用途**：定義多空信號觸發條件的價格區間。

- `HH2LL_len`  
  **描述**：第一到第二個尖點的距離加上第一距離的反轉觀察區比例。  
  **類型**：浮點數 (float)  
  **預設值**：0.5  
  **用途**：調整趨勢信號的靈敏度。

### 信號繪製
- `plot_bull`  
  **描述**：是否繪製多方信號。  
  **類型**：布林值 (bool)  
  **預設值**：`true`  
  **用途**：控制是否顯示多頭的圖形化標示。

- `plot_bear`  
  **描述**：是否繪製空方信號。  
  **類型**：布林值 (bool)  
  **預設值**：`false`  
  **用途**：控制是否顯示空頭的圖形化標示。

### 移動平均線參數
- `lengthbull_grid_1`  
  **描述**：先跌盤整後打勾的移動平均線長度。  
  **類型**：整數 (int)  
  **預設值**：30  
  **用途**：調整策略在盤整後進場的敏感度。

- `lengthbull_grid_2`  
  **描述**：先盤整突破後打勾的移動平均線長度。  
  **類型**：整數 (int)  
  **預設值**：60  
  **用途**：調整策略在突破後進場的敏感度。

### 風險管理
- `equityPercentage`  
  **描述**：下單量佔餘額的比例。  
  **類型**：浮點數 (float)  
  **預設值**：0.2  
  **用途**：用於計算每次交易的下單數量，控制資金使用率。

---
### 1. 陣列操作
- **功能描述**：使用陣列儲存高點與低點的數據，並透過陣列方法進行操作。  
- **關鍵技術**：
  - `array.new_float()` 和 `array.new_int()` 初始化陣列。
  - `array.push()` 添加新的高點或低點資料。
  - 提取陣列值的自定義函數，如 `f_get_high()` 和 `f_get_low()`。

### 2. 趨勢偵測
- **功能描述**：基於 ZigZag 長度判斷市場趨勢，記錄趨勢轉折點。  
- **關鍵技術**：
  - 使用 `ta.highest()` 和 `ta.lowest()` 找出 ZigZag 長度內的高低點。
  - `ta.barssince()` 計算條件改變的距離，定位趨勢改變的時間。

### 3. 條件邏輯
- **功能描述**：通過條件式設計多空信號的判定邏輯。  
- **關鍵技術**：
  - `if` 條件結構組合，動態更新趨勢方向。
  - 多空條件分別定義為 `bull_cond` 和 `bear_cond`，用於觸發交易信號。

### 4. 視覺化繪圖
- **功能描述**：在圖表上清晰地標示趨勢信號與關鍵區域。  
- **關鍵技術**：
  - 使用 `line.new()` 繪製趨勢線與區域框架。
  - 使用 `plotshape()` 加入圖形符號表示多空信號。
  - 使用 `label.new()` 創建標籤，標註重要點位。

### 5. 策略條件
- **功能描述**：設置多重進場條件，進一步篩選有效信號。  
- **關鍵技術**：
  - 定義進場條件如 `bullcheck1st` 和 `bullcheck2st`，篩選符合標準的信號。
  - 動態計算止損與止盈條件，並自動管理風險。

### 6. 移動平均線
- **功能描述**：結合移動平均線分析，確認趨勢方向是否符合策略條件。  
- **關鍵技術**：
  - 使用 `ta.sma()` 計算移動平均線，用於輔助進場條件的判定。

### 7. 數據輸出與警報
- **功能描述**：生成信號通知，幫助即時執行交易或記錄策略結果。  
- **關鍵技術**：
  - 使用 `alert()` 和 `alertcondition()` 發送交易信號通知。
  - 格式化時間與數據，生成結構化的交易訊息。
透過上述參數的調整，您可以根據個人交易需求與市場特性，自行設置策略的觸發條件與風險管理。
