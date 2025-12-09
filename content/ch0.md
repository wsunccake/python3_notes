# ch0. 開發環境

引導初學者建立 Python 3 的開發環境。內容涵蓋了 Python 3 的安裝步驟、推薦的程式碼編輯器與整合開發環境 (IDE) 選擇，以及多種執行 Python 腳本的方法。透過本章的學習，能夠為後續的 Python 程式設計打下堅實的基礎。

## 0-1. 安裝 Python 3

在開始寫 Python 程式碼前，首要確保系統上已正確安裝 Python 3。Python 3 是目前主流且持續更新的版本，與舊版 Python 2 存在語法差異，因此建議一律使用 Python 3。

### 步驟 1: 檢查 Python 版本

在安裝或更新之前，建議先檢查系統是否已預裝 Python 3，以及其版本為何。

- **操作**: 開啟終端機 (Terminal) 或命令提示字元 (Command Prompt)，輸入以下指令：
  ```bash
  python3 --version
  ```
  或
  ```bash
  python --version
  ```
- **意義**:
  - `python3 --version` 指令會顯示系統中 `python3` 命令所對應的 Python 版本。在 Linux/macOS 環境中，通常會明確區分 `python` (可能指向 Python 2) 和 `python3`。
  - `python --version` 指令則會顯示 `python` 命令所對應的 Python 版本。在 Windows 環境中，`python` 通常直接指向 Python 3。
  - 如果指令執行後顯示類似 `Python 3.x.x` 的輸出，表示 Python 3 已安裝。如果顯示 `Python 2.x.x` 或找不到指令的錯誤，則需要進行安裝。

### 步驟 2: 安裝 Python 3

根據不同的作業系統，安裝方式會有所不同。

#### Windows

- **描述**: Windows 使用者需要從 Python 官方網站下載專用的安裝程式。
- **操作**:
  1.  前往官方 [Python 網站](https://www.python.org/)。
  2.  導航至「Downloads」頁面，選擇適合您 Windows 系統（32 位元或 64 位元）的最新穩定版 Python 3 安裝程式（通常是 `.exe` 檔案）。
  3.  下載完成後，雙擊執行安裝程式。
  4.  **重要**: 在安裝精靈的第一個畫面，務必勾選「**Add Python X.Y to PATH**」（將 Python X.Y 加入 PATH 環境變數）選項。這一步驟至關重要，它會自動設定系統的環境變數，可以在任何命令提示字元或 PowerShell 視窗中直接執行 `python` 或 `pip` 命令，而無需指定 Python 的完整安裝路徑。
  5.  選擇「Install Now」進行預設安裝，或選擇「Customize installation」進行自訂安裝（通常預設安裝即可滿足初學者需求）。
  6.  按照提示完成安裝。
- **意義**: 將 Python 加入 PATH 環境變數後，作業系統才能在任何目錄下找到並執行 Python 直譯器。

#### Linux

- **描述**: 大多數 Linux 發行版通常會預裝 Python 3。如需要更新版本或尚未安裝，可使用發行版提供的套件管理器進行安裝。
- **操作**:
  - **Debian/Ubuntu (基於 APT 的系統)**:
    ```bash
    sudo apt update         # 更新套件列表
    sudo apt install python3 # 安裝 Python 3
    ```
    - **意義**: `sudo apt update` 用於更新本地套件索引，確保您能安裝到最新的軟體資訊。`sudo apt install python3` 則會從官方軟體庫下載並安裝 Python 3 及其相關依賴。
  - **Rocky/Fedora (基於 DNF 的系統)**:
    ```bash
    sudo dnf install python3 # 安裝 Python 3
    ```
    - **意義**: `sudo dnf install python3` 是 DNF 套件管理器在 Rocky 或 Fedora 系統上安裝 Python 3 的指令。
  - **Arch Linux (基於 Pacman 的系統)**:
    ```bash
    sudo pacman -S python # 安裝 Python (通常會安裝 Python 3)
    ```
    - **意義**: `sudo pacman -S python` 是 Pacman 套件管理器在 Arch Linux 上安裝 Python 的指令。在 Arch Linux 中，`python` 套件通常直接指向 Python 3。
- **注意**: 這些指令會從系統的官方軟體庫安裝 Python。如果需要安裝特定版本或多個 Python 版本，可能需要使用 `pyenv` 或 `conda` 等工具。

#### WSL (Windows Subsystem for Linux)

- **描述**: WSL 允許 Windows 使用者在 Windows 系統上運行一個輕量級的 Linux 環境。這提供了一個與原生 Linux 相似的開發體驗，對於希望在 Windows 上進行 Linux 開發的程式設計師來說非常有用。
- **操作**:
  1.  首先，需要在 Windows 上啟用 WSL 功能並安裝一個 Linux 發行版（例如 Ubuntu）。這通常透過 PowerShell 以管理員身份執行指令來完成。
  2.  一旦 WSL 環境設定完成並啟動，您就可以在 WSL 的終端機中，按照上述 Linux 的安裝步驟來安裝 Python 3。
- **意義**: WSL 提供了一個隔離且功能完整的 Linux 環境，避免了 Windows 本機環境可能存在的相容性問題，同時也能利用到 Linux 系統的許多開發工具和套件管理器。

---

## 0-2. Editor / IDE

選擇一個合適的程式碼編輯器 (Editor) 或整合開發環境 (IDE) 對於程式設計的效率和體驗至關重要。編輯器通常輕量且靈活，而 IDE 則提供更全面的開發工具。

### 編輯器 (Editor) 與 整合開發環境 (IDE) 的差異

- **編輯器 (Editor)**: 是一種用於撰寫和編輯純文字檔案的軟體。對於程式碼而言，編輯器通常提供語法高亮、自動完成、程式碼摺疊等基本功能。它們通常啟動快速、資源佔用少，並且可以透過擴充功能來增加更多功能。
- **整合開發環境 (IDE)**: 是一個提供程式開發所需所有工具的綜合性軟體應用程式。除了編輯器的功能外，IDE 通常還包含編譯器、直譯器、除錯器 (Debugger)、版本控制系統整合、自動化建構工具等。IDE 旨在最大化程式設計師的生產力，尤其適用於大型專案。

### 推薦用於 Python 開發

#### VSCode (Editor, 編輯器)

- **描述**: Visual Studio Code (VSCode) 是一款由 Microsoft 開發的免費、開源且功能強大的程式碼編輯器。它以其輕量級、高度可擴充性和豐富的生態系統而聞名，廣受開發者喜愛。
- **優點**:
  - **輕量高效**: 啟動速度快，資源佔用相對較少。
  - **跨平台**: 支援 Windows、macOS 和 Linux。
  - **豐富的擴充功能**: 透過安裝各種擴充功能，可以為 Python 開發提供強大的支援，例如語法檢查 (Linting)、程式碼格式化 (Formatting)、除錯、Jupyter Notebook 支援等。
  - **內建終端機**: 方便在編輯器內直接執行命令。
  - **Git 整合**: 內建對 Git 版本控制的良好支援。
- **操作**: 從 [Visual Studio Code 官方網站](https://code.visualstudio.com/) 下載並安裝。安裝後，建議在 VSCode 內搜尋並安裝 Microsoft 提供的「Python」擴充功能，這將為 Python 開發提供最佳體驗。
- **意義**: VSCode 透過其靈活性和強大的擴充功能，能夠滿足從初學者到專業開發者的多樣化需求，是學習 Python 的絕佳起點。

#### PyCharm Community (IDE)

- **描述**: PyCharm 是一款專為 Python 開發設計的整合開發環境，由 JetBrains 公司開發。它提供了許多專為 Python 程式設計師量身打造的智慧功能。PyCharm 分為專業版 (Professional) 和社群版 (Community)，其中社群版是免費且開源的，足以滿足大多數個人開發和學習需求。
- **優點**:
  - **智慧程式碼編輯**: 提供進階的程式碼自動完成、錯誤檢查、重構 (Refactoring) 工具。
  - **強大的除錯器**: 內建功能完善的除錯器，方便追蹤程式碼執行流程和變數狀態。
  - **專案管理**: 支援虛擬環境 (Virtual Environments) 管理，有助於隔離不同專案的依賴。
  - **測試工具整合**: 內建單元測試框架的支援。
  - **資料庫工具**: 專業版提供資料庫工具，社群版則可透過外掛擴充。
- **操作**: 從 [JetBrains PyCharm 官方網站](https://www.jetbrains.com/pycharm/) 下載並安裝 PyCharm Community 版本。
- **意義**: PyCharm 作為一款專門的 Python IDE，其提供的智慧功能和整合工具能夠顯著提升開發效率，尤其適合處理較為複雜的 Python 專案。

---

## 0-3. 執行 Python 腳本

撰寫完 Python 程式碼後，下一步就是執行它，讓程式碼動起來。本節將以一個簡單的 `hello.py` 腳本為例，介紹多種執行 Python 程式的方法。

```python
# hello.py
print("Hello Python!")
```

這個 `hello.py` 腳本非常簡單，它的功能是向螢幕輸出字串 "Hello Python!"。

### 0-3-1. 命令列介面 (CLI)

命令列介面 (Command Line Interface, CLI) 是執行 Python 腳本最基本且通用的方法。它透過文字命令與作業系統互動。

#### Linux/macOS

- **描述**: 在 Linux 或 macOS 系統上，通常使用 `python3` 命令來執行 Python 腳本。
- **操作**:
  1.  **打開終端機**: 啟動您的終端機應用程式（例如 GNOME Terminal, Konsole, iTerm2 等）。
  2.  **導航到腳本目錄**: 使用 `cd` (change directory) 命令進入 `hello.py` 檔案所在的目錄。
      ```bash
      cd <directory_path>
      # 範例：如果 hello.py 在 /home/user/my_python_scripts
      # cd /home/user/my_python_scripts
      ```
      - **意義**: `cd` 命令用於改變當前工作目錄。在執行腳本時，作業系統會預設在當前工作目錄中尋找檔案。
  3.  **執行腳本**: 輸入 `python3` 命令，後面跟著腳本的檔案名稱。
      ```bash
      python3 hello.py
      ```
      - **意義**: `python3` 命令會啟動 Python 3 直譯器，並將 `hello.py` 作為參數傳遞給它。直譯器會逐行讀取並執行 `hello.py` 中的程式碼。
- **預期輸出**:
  ```
  Hello Python!
  ```

#### Windows

- **描述**: 在 Windows 系統上，通常使用 `python` 命令來執行 Python 腳本。
- **操作**:
  1.  **打開命令提示字元或 PowerShell**: 透過搜尋或開始選單啟動「命令提示字元」或「PowerShell」。
  2.  **導航到腳本目錄**: 使用 `cd` 命令進入 `hello.py` 檔案所在的目錄。
      ```bash
      cd <directory_path>
      # 範例：如果 hello.py 在 C:\Users\YourUser\Documents\PythonScripts
      # cd C:\Users\YourUser\Documents\PythonScripts
      ```
  3.  **執行腳本**: 輸入 `python` 命令，後面跟著腳本的檔案名稱。
      ```bash
      python hello.py
      ```
      - **注意**: 在 Windows 上，如果 Python 3 在安裝時已正確加入 PATH 環境變數，通常可執行檔的名稱是 `python`，而不是 `python3`。這是因為 Windows 系統通常不會像 Linux/macOS 那樣預設安裝多個 Python 版本並區分 `python` 和 `python3`。
- **預期輸出**:
  ```
  Hello Python!
  ```

### 0-3-2. 圖形使用者介面 (GUI) - Windows

在 Windows 環境中，如果 Python 檔案類型 (`.py`) 已正確關聯到 Python 直譯器，使用者可以直接透過圖形介面執行腳本。

- **描述**: 雙擊 `.py` 檔案即可執行。
- **操作**:
  1.  在檔案總管中找到您的 `hello.py` 檔案。
  2.  雙擊該檔案。
- **意義**: Windows 作業系統會根據檔案的副檔名 (`.py`) 找到預設的開啟程式（即 Python 直譯器），然後使用該程式執行腳本。
- **注意事項**:
  - 對於像 `print("Hello Python!")` 這樣簡單且執行快速的腳本，一個命令提示字元視窗可能會瞬間彈出並立即關閉，導致您無法看到輸出。這是因為程式執行完畢後，視窗會自動關閉。
  - 如果腳本包含使用者互動（例如 `input()` 函數）或執行時間較長，視窗可能會保持開啟狀態，直到程式結束或使用者手動關閉。
  - 這種方法不適合需要傳遞命令列參數或在特定環境下執行的複雜腳本。
- **適用情境**: 適合執行簡單的、不需要複雜輸入或長時間觀察輸出的腳本。

### 0-3-3. VSCode

Visual Studio Code 提供了多種便捷的方式來執行 Python 腳本，尤其是在安裝了官方 Python 擴充功能後。

- **描述**: 透過 VSCode 內建的功能，可以直接在編輯器環境中執行 Python 腳本，並在整合終端機中查看輸出。
- **操作**:
  1.  **打開 VSCode**: 啟動 Visual Studio Code。
  2.  **安裝 Python 擴充功能**: 如果尚未安裝，請點擊左側活動欄的「Extensions」圖示（或按下 `Ctrl+Shift+X`），搜尋「Python」，然後安裝由 Microsoft 提供的擴充功能。
  3.  **開啟 `hello.py` 檔案**: 在 VSCode 中打開您的 `hello.py` 檔案（或任何 Python 腳本）。
  4.  **執行腳本**: 有以下幾種方式：
      - **點擊右上角的「執行 Python 檔案」播放按鈕**: 這是最直觀的方式。在編輯器視窗的右上角，通常會有一個綠色的播放按鈕。點擊它，VSCode 會在內建終端機中執行當前開啟的 Python 檔案。
      - **在編輯器中右鍵點擊並選擇「在終端機中執行 Python 檔案」**: 在編輯器區域的任何位置右鍵點擊，會彈出上下文選單。選擇「Run Python File in Terminal」選項。
      - **使用鍵盤快捷鍵 `Ctrl+Shift+P`（或 macOS 上的 `Cmd+Shift+P`）打開命令面板，然後輸入「Python: 在終端機中執行 Python 檔案」**: 命令面板提供了快速存取 VSCode 各種功能的途徑。輸入關鍵字即可找到並執行相關命令。
- **意義**: VSCode 的整合執行功能，讓開發者無需頻繁切換應用程式，即可在同一環境中編輯、執行和除錯程式碼，極大地提高了開發效率。輸出會顯示在 VSCode 內建的終端機中，方便查看。

### 0-3-4. PyCharm

PyCharm 作為一款專為 Python 設計的 IDE，提供了非常完善且智慧的腳本執行和管理功能。

- **描述**: PyCharm 能夠以專案為單位管理程式碼，並提供多種方式來執行 Python 腳本，包括從專案視窗、編輯器或頂部選單。
- **操作**:
  1.  **打開 PyCharm**: 啟動 PyCharm。
  2.  **開啟專案或檔案**:
      - 如果是第一次使用 PyCharm，可以選擇「Open」並導航到 `my_python_projects` 目錄，將其作為一個專案開啟。
      - 或者，直接開啟 `hello.py` 檔案。
  3.  **執行腳本**: 有以下幾種方式：
      - **在專案工具視窗中右鍵點擊 `hello.py` 檔案並選擇「執行 'hello'」**: 在 PyCharm 左側的「Project」工具視窗中，找到 `hello.py` 檔案。右鍵點擊它，然後從上下文選單中選擇「Run 'hello'」。
      - **點擊編輯器頂部或 `if __name__ == "__main__":` 區塊（如果存在）旁邊的綠色播放按鈕**: 當您在編輯器中開啟 `hello.py` 時，PyCharm 會在編輯器視窗的頂部工具列顯示一個綠色的播放按鈕。如果您的腳本包含 `if __name__ == "__main__":` 區塊，該區塊旁邊也會有一個小綠色播放按鈕，點擊即可執行該腳本。
      - **前往 `Run` -> `Run 'hello'`**: 透過 PyCharm 頂部選單欄的「Run」選項，可以找到當前專案中可執行的腳本列表，選擇「Run 'hello'」即可。
- **意義**: PyCharm 的執行功能與其專案管理緊密結合，能夠自動處理虛擬環境等設定，為開發者提供一個高度整合且智慧的執行環境。輸出會顯示在 PyCharm 內建的「Run」工具視窗中。

### 0-3-4 總結

PyCharm 作為專業的 Python IDE，提供了多樣化且智慧的腳本執行方式，包括從專案視窗、編輯器或頂部選單。它與專案管理和虛擬環境的整合，為 Python 開發者帶來了高效且無縫的執行體驗。

---

## 0-4. 參考資料

一些重要的參考資源，這些是學習 Python 和相關開發工具的官方來源。

- **[Python 官方網站](https://www.python.org/)**: Python 官方網站，提供最新的 Python 版本下載、官方文件、教學資源以及社群資訊。
- **[Visual Studio Code](https://code.visualstudio.com/)**: VSCode 官方網站，提供編輯器的下載、安裝指南、擴充功能市場以及詳細的使用文件。
- **[JetBrains PyCharm](https://www.jetbrains.com/pycharm/)**: PyCharm 官方網站，提供不同版本的下載、功能介紹、教學和支援。

---

## 0-5. 總結

說明 Python 3 安裝過程，對於不同系統使用者如何安裝設定。接著，介紹了兩款主流的程式碼編輯器/IDE：輕量級且高度可擴充的 VSCode，以及功能強大且專為 Python 設計的 PyCharm Community。最後，透過一個簡單的 `hello.py` 腳本，示範了四種執行 Python 程式的方法。

---

## 0-6. 練習

1.  **完整環境設定**:
    - 確保電腦上已成功安裝 Python 3。
    - 在作業系統（Windows、Linux 或 WSL）上，開啟終端機或命令提示字元。執行 `python3 --version` 和 `python --version` 指令顯示目前使用版本。
    - 選擇並安裝偏好的程式碼編輯器或 IDE (VSCode 或 PyCharm)。
    - 在工具中，建立一個新的 Python 檔案，並輸入一個簡單的 `print()` 語句。
    - 嘗試使用該工具內建的功能來執行這個 Python 檔案，並觀察輸出。
2.  **PATH 環境變數的理解**:
    - 嘗試在命令列介面中，從一個 **非** Python 腳本所在目錄的位置，執行 Python 腳本（例如，如果腳本在 `C:\my_scripts`，目前在 `C:\` 目錄下嘗試執行 `python my_scripts\your_script.py`）。
    - 思考為什麼這樣可以執行，以及 PATH 環境變數在其中扮演的角色。
3.  **探索編輯器/IDE 功能**:
    - 在編輯器/IDE 中，嘗試尋找並使用以下功能：
      - 程式碼自動完成 (Code Autocompletion)。
      - 語法高亮 (Syntax Highlighting)。
      - 內建終端機 (Integrated Terminal)。
      - 檔案瀏覽器 (File Explorer/Project View)。
    - 這些功能如何幫助您更有效地撰寫程式碼？
4.  **錯誤處理初探**:
    - 故意在 Python 腳本中引入一個簡單的語法錯誤（例如，將 `print("Hello")` 改為 `prnt("Hello")`）。
    - 嘗試執行這錯誤腳本。
    - 觀察錯誤訊息，並嘗試理解它在說什麼。這將是學習除錯的第一步。
