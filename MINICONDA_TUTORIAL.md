# Miniconda 安裝與基礎使用教程

這份文檔將一步一步地引導你完成 Miniconda 的安裝與配置。

### 為什麼我們需要 Miniconda？

在開始之前，你可能會問：什麼是 Miniconda，我們為什麼需要它？

你可以把 Miniconda 想像成一個為你的編程項目準備的、極其乾淨且有組織的「工具箱系統」。

當你在電腦上進行不同的項目時（比如一個網站項目，一個數據分析項目），它們可能需要不同版本的工具（比如不同版本的 Python）。如果把所有工具都隨意安裝在電腦上，它們之間可能會產生衝突，就像把所有螺絲刀和錘子都扔在一個大抽屜裡一樣混亂。

Miniconda 幫助我們為**每一個項目創建一個獨立的、乾淨的工具箱（稱為「環境」，Environment）**。這樣，A 項目的工具就絕對不會影響到 B 項目。

在我們這次的工作坊中，我們使用 Miniconda 的主要目的，是為了獲得一個穩定、可靠的 Python 環境，用來運行一個本地的網頁伺服器，方便我們實時預覽對網站所做的修改。

---

## 第一部分：安裝 Miniconda

請根據你的操作系統，跟隨對應的詳細步驟。

### macOS 安裝步驟

1.  **下載安裝包:**
    *   請用瀏覽器打開 Miniconda 官方文檔的下載頁面：[https://docs.conda.io/projects/miniconda/en/latest/](https://docs.conda.io/projects/miniconda/en/latest/)
    *   在頁面中找到 "macOS installers" 部分。
    *   根據你的 Mac 晶片類型，下載對應的 `.pkg` 文件。
        *   如果你用的是較新的 Mac (M1, M2, M3 晶片)，請選擇 **Apple M1 (arm64)** 的版本。
        *   如果你用的是較舊的 Intel 晶片 Mac，請選擇 **Intel (x86_64)** 的版本。

2.  **運行安裝程序:**
    *   下載完成後，在你的「下載」文件夾中找到這個 `.pkg` 文件並雙擊它。
    *   安裝程序會啟動。你會看到「簡介」、「許可證」等步驟。
    *   你**不需要**做任何複雜的選擇，只需一路點擊 **「繼續 (Continue)」** -> **「同意 (Agree)」** -> **「安裝 (Install)」** 即可。
    *   系統可能會要求你輸入你的電腦密碼來授權安裝。
    *   安裝完成後，你可以將安裝包文件移到垃圾桶。

### Windows 安裝步驟

1.  **下載安裝包:**
    *   請用瀏覽器打開 Miniconda 官方文檔的下載頁面：[https://docs.conda.io/projects/miniconda/en/latest/](https://docs.conda.io/projects/miniconda/en/latest/)
    *   在頁面中找到 "Windows installers" 部分。
    *   點擊 **`Miniconda3-latest-Windows-x86_64.exe`** 這個鏈接進行下載。

2.  **運行安裝程序:**
    *   下載完成後，找到這個 `.exe` 文件並雙擊它。
    *   點擊 **「Next」**。
    *   點擊 **「I Agree」** 來同意許可協議。
    *   在 "Installation Type" 頁面，保持默認的 **「Just Me」** 選項，點擊 **「Next」**。
    *   在 "Choose Install Location" 頁面，直接點擊 **「Next」**，使用默認的安裝路徑即可。
    *   在 "Advanced Installation Options" 頁面，**請保持所有選項為默認狀態（即所有框都不要勾選）**。這是最安全、最推薦的設置。直接點擊 **「Install」**。
    *   等待安裝進度條走完，然後點擊 **「Finish」**。

---

## 第二部分：驗證安裝與初次使用

安裝完成後，我們需要驗證它是否成功，並學會如何打開正確的工具。

1.  **打開正確的命令行工具:**
    *   **macOS 用戶:** 關閉所有已打開的 Terminal 窗口，然後重新打開一個新的 **Terminal** 窗口。
    *   **Windows 用戶:** **這一點至關重要！** 從現在開始，請**不要**再使用 `CMD`。點擊你的「開始」菜單，輸入 `Anaconda`，然後選擇打開 **`Anaconda Prompt (miniconda3)`**。這是一個專門為 Conda 配置好的命令行工具。

2.  **驗證 Conda:**
    *   在新的命令行窗口中，輸入以下指令，然後按 Enter：
        ```bash
        conda --version
        ```
    *   如果你看到類似 `conda 24.5.0` 的輸出，恭喜你，Conda 已經成功安裝！

3.  **驗證 Python:**
    *   在同一個窗口中，輸入以下指令，然後按 Enter：
        ```bash
        python --version
        ```
    *   你應該會看到 Python 的版本號，例如 `Python 3.11.7`。這證明 Miniconda 也為你安裝好了 Python。

---

## 第三部分：核心指令入門

現在你已經擁有了這個工具，讓我們來學習幾個最核心的指令。

#### `conda env list` (查看所有工具箱)

這個指令會列出你電腦上所有已經創建的 Conda 環境（你的所有「工具箱」）。
```bash
conda env list
```
剛安裝完，你應該只會看到一個名為 `base` 的環境，它前面會有一個 `*` 號，表示你當前正處於這個基礎環境中。

#### `conda create` (創建新工具箱)

為一個新項目創建一個全新的、獨立的環境。

**用法:**
```bash
conda create --name [你的環境名] python=[python版本]
```

**範例:**
假設我們要為這次工作坊創建一個名為 `web-workshop` 的環境，並指定使用 Python 3.10 版本。
```bash
conda create --name web-workshop python=3.10
```
執行後，Conda 會計算需要安裝的包，並詢問你是否繼續 `(Proceed [y]/n)?`。輸入 `y` 然後按 Enter 即可。

#### `conda activate` (進入工具箱)

激活或進入一個你已經創建好的環境。

**用法:**
```bash
conda activate [你的環境名]
```

**範例:**```bash
conda activate web-workshop
```
執行後，你會發現命令行提示符的最前面，出現了 `(web-workshop)` 的字樣。這是一個非常清晰的標誌，告訴你現在正處於 `web-workshop` 這個獨立的環境中，在這裡進行的所有操作都不會影響到 `base` 環境或其他環境。

#### `conda deactivate` (離開工具箱)

當你完成工作，可以離開當前環境，回到 `base` 基礎環境。

**用法:**
```bash
conda deactivate
```
執行後，提示符前面的 `(web-workshop)` 字樣就會消失。

---

### 我們工作坊的實踐 (Practical Application)

在我們的工作坊中，為了簡化流程，我們甚至**不需要創建新環境**，可以直接使用 Miniconda 自帶的 `base` 環境中的 Python。

當你需要預覽網站時，你的操作步驟將是：

1.  打開命令行工具 (macOS 的 Terminal 或 Windows 的 Anaconda Prompt)。
2.  使用 `cd` 指令進入你的項目文件夾。
3.  運行以下指令來啟動本地網頁伺服器：
    ```bash
    python -m http.server 8000
    ```
