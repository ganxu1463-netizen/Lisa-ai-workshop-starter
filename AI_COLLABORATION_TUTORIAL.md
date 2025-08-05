# 與 AI 協同進行高級網頁互動開發教程

本教程提供一個方法論框架，用於指導如何與 AI 代理 (AI agent) 進行有效交互，以修改和擴展現有網頁模板的功能。教程包含一個完整的實例，演示如何為我們的作品集網站添加一個模板中不存在的、由 `three.js` 驅動的 3D 互動元素。

---

### 第一部分：AI 交互與 Prompt 撰寫原則

與 AI 協同工作的核心是提出精確、結構化的指令 (Prompt)。一個有效的 Prompt 能夠將你的意圖準確地傳達給 AI，從而獲得可用的、高質量的代碼。以下是四個核心原則。

#### 原則一：提供明確上下文 (Provide Specific Context)

AI 沒有關於你項目文件的先驗知識。你必須提供它完成任務所需的所有相關信息。

*   **低效的 Prompt:**
    > "給我一些 CSS 來讓標題更好看。"

*   **高效的 Prompt:**
    > "我正在修改一個名為 `index-fullscreen-agency-image.html` 的文件。文件中有一個 `<h1>` 元素，它的 class 是 `headline__title`。請提供一段 CSS 代碼，將這個元素的文本顏色更改為從 `#FF00FF` 到 `#800080` 的線性漸變色。"

#### 原則二：定義 AI 的角色與產出格式 (Define Role and Format)

告訴 AI 它應該扮演什麼角色，以及你希望它以何種格式返回結果。

*   **低效的 Prompt:**
    > "寫一些關於我的介紹。"

*   **高效的 Prompt:**
    > "你是一名為實驗藝術家撰寫網站文案的專業寫手。請為我的作品集網站主頁生成三段關於『互動裝置』的簡短介紹文本。每一段都應當包含在一個 `<p>` 標籤內。"

#### 原則三：分解複雜任務 (Deconstruct Complex Tasks)

不要試圖用一個 Prompt 完成所有事情。將一個複雜的目標分解為一系列更小、更具體的步驟。我們將在下面的實例中詳細演示這一點。

#### 原則四：迭代與修正 (Iterate and Refine)

AI 生成的初始代碼可能不是完美的。你需要學會閱讀代碼，找出問題，然後向 AI 提出修正要求。

*   **迭代 Prompt 範例:**
    > "你之前提供的 3D 立方體代碼運行正常。現在，請修改這段代碼，將立方體的顏色從默認顏色改為 `0x00ff00`（綠色），並使其初始旋轉速度減慢 50%。"

---

### 第二部分：實例 - 集成一個新的 3D 互動庫

**目標：** 在 `index-fullscreen-agency-image.html` 頁面的主視覺區域，添加一個能與鼠標位置互動的 3D 立方體。

我們的模板倉庫中**不包含** `three.js` 這個 3D 庫。我們將從零開始，完全依靠與 AI 的交互來完成集成。

#### 步驟 0：準備本地測試環境

在進行任何修改之前，必須先運行本地伺服器以便實時預覽。

1.  **打開命令行工具:**
    *   **macOS:** 打開 Terminal。
    *   **Windows:** 打開 `Anaconda Prompt (miniconda3)`。

2.  **導航至項目目錄:**
    *   使用 `cd` 指令進入你的項目文件夾。
        ```
        cd path/to/your/project/folder
        ```

3.  **啟動本地伺服器:**
    *   運行以下指令。這會使用 Miniconda 提供的 Python 來啟動一個伺服器。
        ```
        python -m http.server 8000
        ```

4.  **打開預覽頁面:**
    *   打開你的網頁瀏覽器 (如 Chrome)，訪問 `http://localhost:8000/index-fullscreen-agency-image.html`。
    *   你現在應該能看到原始的網站模板。**保持這個頁面和命令行窗口的打開狀態。**

#### 步驟一：尋找並引入 `three.js` 庫

我們的第一步是讓 AI 告訴我們如何將 `three.js` 添加到我們的項目中。對於靜態網站，最直接的方法是使用 CDN。

1.  **向 AI 提問:**
    *   **Prompt:**
        > "我需要在一個靜態 HTML 項目中使用 `three.js` 庫。請為我提供引入 `three.js` 最新穩定版的 CDN `<script>` 標籤。"

2.  **獲取 AI 的回答:**
    *   AI 應該會返回類似這樣的代碼：
        ```html
        <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
        ```
        *(注意：版本號可能會有所不同)*

3.  **修改 HTML 文件:**
    *   用你的代碼編輯器打開 `index-fullscreen-agency-image.html` 文件。
    *   滾動到文件的**最底部**，找到 `<!-- Load Scripts Start -->` 部分。
    *   將 AI 提供的 `<script>` 標籤，粘貼在 `js/libs.min.js` 的**上方**。確保 `three.js` 在 `custom.js` 之前被加載。
        ```html
        <!-- Load Scripts Start -->
        <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
        <script src="js/libs.min.js"></script>
        <script src="js/gallery-init.js"></script>
        <script src="js/custom.js"></script>
        <!-- Load Scripts End -->
        ```

4.  **保存文件。** 現在，`three.js` 庫已經可以在你的網站中被使用了。

#### 步驟二：為 3D 場景創建 HTML 容器

3D 圖形需要一個 `<canvas>` 元素來進行繪製。

1.  **向 AI 提問:**
    *   **Prompt:**
        > "為 `three.js` 場景生成一個 HTML `<canvas>` 元素。它的 ID 應該是 `three-canvas`。"

2.  **獲取 AI 的回答:**
    *   AI 會返回：
        ```html
        <canvas id="three-canvas"></canvas>
        ```

3.  **修改 HTML 文件:**
    *   在 `index-fullscreen-agency-image.html` 文件中，找到 `<section id="main" ...>`。
    *   將 `<canvas>` 標籤粘貼到 `<div class="fullscreen-wrapper">` 元素的**內部最前面**。
        ```html
        <section id="main" class="main full">
          <div class="fullscreen-wrapper">
        
            <canvas id="three-canvas"></canvas>
        
            <!-- Fullscreen Background Start-->
            <div class="fullscreen-bg media-full-4">
        ```

4.  **添加基礎樣式:**
    *   為了讓 `<canvas>` 正確顯示，我們需要一些 CSS。
    *   **Prompt:**
        > "為 ID 為 `three-canvas` 的 `<canvas>` 元素提供 CSS 代碼。它應該：
        > 1. 使用 `position: absolute` 定位。
        > 2. 覆蓋整個屏幕 (`top: 0; left: 0; width: 100%; height: 100%;`)。
        > 3. 位於其他內容的下方 (`z-index: 0;`)。"

5.  **修改 CSS 文件:**
    *   打開 `css/main.css` 文件。
    *   將 AI 生成的 CSS 代碼粘貼到文件的**最末尾**。
        ```css
        #three-canvas {
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          z-index: 0;
        }
        ```

6.  **保存所有文件。**

#### 步驟三：生成並集成 3D 交互的 JavaScript 代碼

這是最核心的一步。我們需要撰寫一個詳細的 Prompt 來生成完整的交互邏輯。

1.  **向 AI 提問:**
    *   **Prompt:**
        > "你是一名熟悉 `three.js` 的 JavaScript 開發者。請為我編寫一段完整的 JavaScript 代碼，並遵循以下要求：
        > 1.  在 ID 為 `three-canvas` 的 `<canvas>` 元素上初始化一個 `three.js` 場景。
        > 2.  場景的背景應為透明。
        > 3.  在場景中央創建一個 `BoxGeometry`（立方體），材質為 `MeshNormalMaterial`。
        > 4.  創建一個 `animate` 循環函數來持續渲染場景。
        > 5.  在 `animate` 函數中，讓立方體同時在 X 軸和 Y 軸上緩慢旋轉。
        > 6.  監聽鼠標的 `mousemove` 事件。
        > 7.  根據鼠標在窗口中的 X 和 Y 位置（從 -1 到 1 的範圍），動態改變立方體在 Y 軸和 X 軸上的旋轉速度。
        > 8.  確保代碼在窗口大小改變時能夠自適應更新渲染器和攝像機的尺寸。
        > 9.  所有代碼應當在 DOM 加載完成後執行。"

2.  **修改 JavaScript 文件:**
    *   打開 `js/custom.js` 文件。
    *   將 AI 返回的**完整 JavaScript 代碼**粘貼到該文件的最末尾。

3.  **保存文件。**

#### 步驟四：本地測試與查看結果

1.  **確認本地伺服器仍在運行。**
2.  **刷新瀏覽器。** 回到你打開的 `http://localhost:8000/index-fullscreen-agency-image.html` 頁面並刷新。
3.  **驗證結果:**
    *   你應該能看到一個彩色的 3D 立方體出現在頁面中央。
    *   移動你的鼠標。立方體的旋轉速度和方向應該會根據你的鼠標位置發生實時變化。

---

### 第三部分：高級實例 - 連接真實世界數據

**目標：** 擴展上一步的 3D 場景，使其能夠獲取英國利茲 (Leeds) 的實時天氣數據，並用這些數據來驅動 3D 模型的顏色和形態變化。

#### 步驟五：尋找並理解天氣數據 API

要獲取真實數據，我們需要一個 API (Application Programming Interface)。這是一個允許不同程序相互通信的接口。我們將使用一個免費、無需註冊的公共天氣 API：`Open-Meteo`。

1.  **確定數據源:**
    *   我們需要利茲的經緯度 (約為 53.80° N, 1.55° W)。
    *   我們需要獲取溫度 (`temperature_2m`) 和風速 (`wind_speed_10m`)。
    *   根據 Open-Meteo 的文檔，我們可以構建如下的 API 請求 URL：
        `https://api.open-meteo.com/v1/forecast?latitude=53.80&longitude=-1.55&current=temperature_2m,wind_speed_10m`

2.  **理解數據格式:**
    *   在瀏覽器中訪問上述 URL。你會看到一段 JSON 格式的數據，結構如下：
        ```json
        {
          "current": {
            "time": "...",
            "interval": 900,
            "temperature_2m": 15.0,
            "wind_speed_10m": 12.5
          }
        }
        ```
    *   我們需要從 `current` 對象中提取 `temperature_2m` 和 `wind_speed_10m` 的值。

#### 步驟六：使用 AI 編寫獲取數據並驅動 3D 模型的代碼

現在，我們將向 AI 提出一個更複雜的、整合性的 Prompt，要求它在我們已有的 3D 場景代碼基礎上，增加獲取和應用實時數據的功能。

1.  **向 AI 提問 (整合性 Prompt):**
    *   **Prompt:**
        > "你是一名專精於 `three.js` 和數據可視化的創意編程開發者。請**完整地重寫**我在 `js/custom.js` 中的全部代碼。新的代碼需要在原有功能（一個與鼠標交互的旋轉立方體）的基礎上，增加以下新功能：
        >
        > 1.  **獲取實時數據:**
              >     *   創建一個異步函數 `fetchWeatherData()`。
        >     *   在此函數中，使用 `fetch` API 從這個 URL 獲取數據：`https://api.open-meteo.com/v1/forecast?latitude=53.80&longitude=-1.55&current=temperature_2m,wind_speed_10m`。
        >     *   解析返回的 JSON 數據，並將 `temperature_2m` 和 `wind_speed_10m` 的值存儲在兩個全局變量中，例如 `currentTemperature` 和 `currentWindSpeed`。
        >     *   使用 `setInterval`，每 10 分鐘調用一次 `fetchWeatherData()` 函數以刷新數據。
        >
        > 2.  **數據驅動視覺變化:**
              >     *   將立方體的材質從 `MeshNormalMaterial` 更改為 `MeshStandardMaterial`，並為場景添加基礎的 `AmbientLight` 和 `PointLight` 以便觀察顏色。
        >     *   在 `animate` 循環函數中，根據 `currentWindSpeed` 的值來改變立方體材質的顏色。將風速範圍（例如 0 到 50 km/h）映射到 HSL 色彩空間的色相（Hue）值（例如從 0.7 藍色到 0.0 紅色）。
        >     *   在 `animate` 循環函數中，根據 `currentTemperature` 的值來改變立方體的形態。訪問立方體 `geometry` 的頂點位置 (`geometry.attributes.position`)。基於 `time` 和頂點的初始位置，應用一個噪聲函數（例如 `Math.sin`）來使頂點產生微小的位移。使用 `currentTemperature` 的值來控制這個位移的幅度。溫度越高，形態變化越劇烈；溫度越低，形態越接近標準立方體。
        >
        > 3.  **整合:**
              >     *   確保原有的鼠標交互功能（控制旋轉）被完整保留。
        >     *   確保所有代碼都包含在一個立即執行的函數表達式中，以避免污染全局作用域。"

2.  **修改 JavaScript 文件:**
    *   打開 `js/custom.js` 文件。
    *   **刪除**裡面所有舊的代碼。
    *   將 AI 返回的**全新的、完整的 JavaScript 代碼**粘貼進去。

3.  **保存文件。**

#### 步驟七：最終測試與查看結果

1.  **刷新瀏覽器:** 回到 `http://localhost:8000/index-fullscreen-agency-image.html` 頁面並刷新。
2.  **驗證結果:**
    *   立方體現在應該有了基礎的光照和顏色，不再是 `MeshNormalMaterial` 的彩虹色。
    *   它的顏色會基於當前利茲的風速顯示為某個特定色調。
    *   它的形態會基於當前利茲的溫度產生一定程度的扭曲變形。
    *   原有的鼠標交互功能應當依然有效。

**備註:** 由於真實天氣數據在短時間內變化不大，你可能不會立即看到劇烈的顏色或形態跳變。這個效果是 subtle 的，反映了真實世界的數據流。你可以嘗試手動修改代碼中的 URL（例如更改經緯度到一個天氣差異大的地方）來觀察更明顯的變化。

通過這個實例，你學習了如何將一個複雜的需求分解，並通過一系列精確的 Prompt，引導 AI 將外部實時數據源成功集成到一個已有的視覺項目中。

