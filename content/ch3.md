# ch3. 其他主題

本章將為 Python 初學者詳細介紹幾個重要的進階主題，包括正規表示式、裝飾器與閉包、程序管理以及平行處理。

## 3-1.正規表示式 (Regular Expressions)

正規表示式 (Regular Expressions，簡稱 regex 或 regexp) 是一種強大且靈活的字串處理工具，用於描述、匹配和操作字串中的特定模式。在 Python 中，我們主要使用內建的 `re` 模組來處理正規表示式。

### 3-1-1. 核心概念

- **模式 (Pattern)**：定義要搜尋的字串規則。
- **匹配 (Match)**：判斷字串是否符合模式。
- **搜尋 (Search)**：在字串中尋找符合模式的部分。
- **替換 (Substitute)**：將符合模式的部分替換為其他字串。

### 3-1-2. `re.search()`

`re.search(pattern, string, flags=0)` 函數會在整個字串中搜尋第一個符合 `pattern` 的位置。如果找到，它會回傳一個 `Match Object`；如果沒有找到，則回傳 `None`。

- **`pattern`**: 要搜尋的正規表示式模式。
- **`string`**: 要被搜尋的目標字串。
- **`flags`**: 可選參數，用於修改匹配行為 (例如 `re.IGNORECASE` 忽略大小寫)。

**範例**: 搜尋字串中是否包含 "Python"

```python
import re

text = "Hello, Python is a great programming language."
pattern = "Python"

match = re.search(pattern, text)

if match:
    print(f"找到匹配: {match.group()}") # match.group() 回傳匹配到的字串
    print(f"匹配起始位置: {match.start()}")
    print(f"匹配結束位置: {match.end()}")
else:
    print("未找到匹配")

# 範例：使用更複雜的 模式
text2 = "The year is 2023."
pattern2 = r"\d+" # \d+ 表示一個或多個數字

match2 = re.search(pattern2, text2)
if match2:
    print(f"找到數字: {match2.group()}")
```

### 3-1-3. `re.match()`

`re.match(pattern, string, flags=0)` 函數只會嘗試從字串的**開頭**匹配 `pattern`。如果字串開頭符合模式，則回傳 `Match Object`；否則回傳 `None`。

- **`pattern`**: 要匹配的正規表示式模式。
- **`string`**: 要被匹配的目標字串。
- **`flags`**: 可選參數。

**範例**: 匹配字串開頭是否為 "Hello"

```python
import re

text1 = "Hello, world!"
text2 = "world, Hello!"
pattern = "Hello"

match1 = re.match(pattern, text1)
match2 = re.match(pattern, text2)

if match1:
    print(f"字串開頭匹配 'Hello': {match1.group()}")
else:
    print("字串開頭未匹配 'Hello'")

if match2:
    print(f"字串開頭匹配 'Hello': {match2.group()}")
else:
    print("字串開頭未匹配 'Hello'")
```

### 3-1-4. `re.findall()`

`re.findall(pattern, string, flags=0)` 函數會在字串中找到所有不重疊的 `pattern` 匹配，並以**列表 (list)** 的形式回傳所有匹配到的字串。

- **`pattern`**: 要搜尋的正規表示式模式。
- **`string`**: 要被搜尋的目標字串。
- **`flags`**: 可選參數。

**範例**: 找出字串中所有的數字

```python
import re

text = "There are 12 apples, 3 oranges, and 45 bananas."
pattern = r"\d+" # 匹配一個或多個數字

numbers = re.findall(pattern, text)
print(f"找到所有數字: {numbers}")

# 範例：找出所有單詞
text2 = "Python is fun. Java is also popular."
pattern2 = r"\b\w+\b" # \b 匹配單詞邊界，\w+ 匹配一個或多個字母、數字或底線

words = re.findall(pattern2, text2)
print(f"找到所有單詞: {words}")
```

### 3-1-5. `re.sub()`

`re.sub(pattern, repl, string, count=0, flags=0)` 函數會將字串中所有符合 `pattern` 的部分替換為 `repl`。

- **`pattern`**: 要搜尋的正規表示式模式。
- **`repl`**: 替換用的字串，也可以是一個函數。
- **`string`**: 要被處理的目標字串。
- **`count`**: 可選參數，指定最大替換次數，預設為 0 (替換所有匹配)。
- **`flags`**: 可選參數。

**範例**: 將字串中的數字替換為 "[NUMBER]"

```python
import re

text = "My phone number is 123-456-7890 and my age is 30."
pattern = r"\d+"
replacement = "[NUMBER]"

new_text = re.sub(pattern, replacement, text)
print(f"替換後的字串: {new_text}")

# 範例：只替換第一次出現的數字
new_text_once = re.sub(pattern, replacement, text, count=1)
print(f"只替換一次的字串: {new_text_once}")
```

### 3-1-6. 正規表示式總結

正規表示式是處理字串模式匹配和操作的強大工具。`re.search()` 用於在整個字串中尋找第一個匹配；`re.match()` 則要求從字串開頭匹配；`re.findall()` 找出所有匹配並回傳列表；而 `re.sub()` 則用於替換匹配到的部分。熟練掌握這些函數和正規表示式語法，將能大幅提升字串處理的效率和靈活性。

### 3-1-7. 正規表示式練習

1.  **提取電子郵件地址**：
    請撰寫一個 Python 程式，使用 `re.findall()` 從以下字串中提取所有有效的電子郵件地址。

    ```
    text = "Contact us at info@example.com or support@mycompany.org. Invalid email: user@.com"
    ```

    提示：一個簡單的電子郵件正規表示式模式可以是 `r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'`。

2.  **驗證電話號碼格式**：
    請撰寫一個 Python 程式，使用 `re.match()` 檢查以下電話號碼是否符合 `XXX-XXX-XXXX` 的格式 (X 為數字)。
    ```python
    phone_number1 = "123-456-7890"
    phone_number2 = "1234567890"
    phone_number3 = "abc-def-ghij"
    ```

---

## 3-2. 程序管理 (Process) 

在 Python 中，執行外部命令或管理系統程序是常見的需求。Python 提供了 `os` 模組中的一些函數以及更現代、功能更強大的 `subprocess` 模組來實現這些功能。

### 3-2-1. `os` 模組中的程序相關函數 (較舊且功能有限)

`os` 模組提供了一些與作業系統互動的函數，其中一些可以用來執行外部命令。然而，它們的功能相對有限，且在處理複雜情境時不如 `subprocess` 模組靈活。

#### 3-2-1-1. `os.system(command)`

執行一個 shell 命令。它會啟動一個子 shell 來執行命令，並回傳命令的退出狀態碼。

- **優點**: 使用簡單。
- **缺點**: 無法捕獲命令的輸出 (stdout/stderr)，安全性較低 (容易受到 shell 注入攻擊)，且在不同作業系統上的行為可能不一致。

**範例**:

```python
import os

# 執行一個簡單的命令
# 在 Linux/macOS 上會列出當前目錄內容
# 在 Windows 上會執行 dir 命令
return_code = os.system("ls -l" if os.name == "posix" else "dir")
print(f"命令退出碼: {return_code}")

# 執行一個不存在的命令
return_code_fail = os.system("non_existent_command")
print(f"不存在命令的退出碼: {return_code_fail}")
```

#### 3-2-1-2. `os.popen(command, mode='r', bufsize=-1)`

執行一個 shell 命令，並回傳一個檔案物件 (file object)，可以透過這個物件讀取或寫入命令的標準輸入/輸出。

- **優點**: 可以捕獲命令的標準輸出。
- **缺點**: 無法直接捕獲標準錯誤 (stderr)，且在處理複雜的輸入/輸出重定向時仍顯不足。

**範例**:

```python
import os

# 執行命令並讀取輸出
with os.popen("echo Hello from os.popen") as f:
    output = f.read()
    print(f"os.popen 輸出: {output.strip()}")

# 執行一個會產生多行輸出的命令
with os.popen("ls -a" if os.name == "posix" else "dir") as f:
    output_lines = f.readlines()
    print("os.popen 列出檔案:")
    for line in output_lines:
        print(f"- {line.strip()}")
```

#### 3-2-1-3. `os.spawn*()` 家族函數 (較少使用)

`os.spawnl`, `os.spawnle`, `os.spawnlp`, `os.spawnlpe`, `os.spawnv`, `os.spawnve`, `os.spawnvp`, `os.spawnvpe` 這些函數用於在新的程序中執行程式。它們提供了對新程序執行方式的更多控制 (例如傳遞參數、環境變數)，但使用起來相對複雜，且在現代 Python 中，`subprocess` 模組通常是更好的選擇。

由於其複雜性及 `subprocess` 的優越性，這裡不提供詳細範例。

### 3-2-2. `subprocess` 模組 (現代 Python 推薦)

`subprocess` 模組是 Python 中執行外部命令和管理子程序的推薦方式。它提供了更強大、更靈活且更安全的介面，可以完全控制子程序的標準輸入、標準輸出和標準錯誤，並處理其退出狀態碼。

#### 3-2-2-1. `subprocess.run()` (Python 3.5+ 推薦)

`subprocess.run()` 是 `subprocess` 模組中最簡單且最推薦的函數，用於執行命令並等待其完成。它回傳一個 `CompletedProcess` 物件，其中包含子程序的輸出、錯誤和退出狀態碼。

- **`args`**: 要執行的命令和其參數，通常建議以列表形式傳遞 (例如 `["ls", "-l"]`)，這樣可以避免 shell 注入問題。如果 `shell=True`，則可以傳遞字串。
- **`capture_output=True`**: 捕獲標準輸出和標準錯誤。
- **`text=True` (或 `encoding='utf-8'`)**: 將輸出解碼為文字字串。
- **`check=True`**: 如果子程序回傳非零退出狀態碼，則會拋出 `CalledProcessError` 異常。
- **`shell=True`**: 如果為 `True`，則命令會透過 shell 執行。這在某些情況下很方便 (例如使用 shell 的管道、萬用字元)，但通常建議設為 `False` 以提高安全性。

**範例**: 執行命令並捕獲輸出

```python
import subprocess
import os

# 執行一個簡單的命令並捕獲輸出
try:
    # 使用列表形式傳遞命令和參數，更安全
    result = subprocess.run(
        ["ls", "-l"], # 或 ["dir"] 在 Windows 上
        capture_output=True,
        text=True,
        check=True,
        encoding='utf-8' # 顯式指定編碼
    )
    print("--- subprocess.run 成功範例 ---")
    print(f"命令輸出:\n{result.stdout}")
    print(f"命令錯誤輸出:\n{result.stderr}")
    print(f"命令退出碼: {result.returncode}")
except subprocess.CalledProcessError as e:
    print(f"命令執行失敗: {e}")
    print(f"錯誤輸出:\n{e.stderr}")
except FileNotFoundError:
    print("命令未找到，請檢查路徑或命令名稱。 mogę")

# 執行一個會失敗的命令
try:
    result_fail = subprocess.run(
        ["non_existent_command"],
        capture_output=True,
        text=True,
        check=True, # 設置為 True 會在命令失敗時拋出異常
        encoding='utf-8'
    )
except subprocess.CalledProcessError as e:
    print("\n--- subprocess.run 失敗範例 ---")
    print(f"命令執行失敗 (退出碼 {e.returncode}): {e}")
    print(f"錯誤輸出:\n{e.stderr}")
except FileNotFoundError:
    print("\n--- subprocess.run 失敗範例 ---")
    print("命令 'non_existent_command' 未找到。")

# 範例：使用 shell=True (較不推薦，除非有特殊需求)
try:
    result_shell = subprocess.run(
        "echo Hello && echo World", # 這裡可以使用 shell 語法
        shell=True,
        capture_output=True,
        text=True,
        check=True,
        encoding='utf-8'
    )
    print("\n--- subprocess.run (shell=True) 範例 ---")
    print(f"命令輸出:\n{result_shell.stdout}")
except subprocess.CalledProcessError as e:
    print(f"命令執行失敗: {e}")
```

#### 3-2-2-2. pipeline

**範例**: 實現 `pipeline` (`subprocess.Popen`)

```python
import subprocess

# 實現 'ls -l | grep ".txt"'
try:
    # 第一個命令: ls -l
    # stdout=subprocess.PIPE 表示將其標準輸出連接到一個管道
    process1 = subprocess.Popen(['ls', '-l'], stdout=subprocess.PIPE)

    # 第二個命令: grep ".txt"
    # stdin=process1.stdout 表示將其標準輸入連接到 process1 的標準輸出
    # capture_output=True 捕獲 grep 的輸出
    # text=True 將輸出解碼為文字
    process2 = subprocess.run(
        ['grep', '.txt'],
        stdin=process1.stdout,
        capture_output=True,
        text=True,
        check=True
    )

    # 關閉 process1 的 stdout 管道，以確保 process2 收到 EOF
    process1.stdout.close()

    # 等待 process1 完成 (雖然 run 會等待 process2，但確保 process1 資源釋放)
    process1.wait()

    print("Pipeline 輸出:")
    print(process2.stdout)
    print("錯誤輸出 (如果有的話):")
    print(process2.stderr)
    print(f"退出碼: {process2.returncode}")

except subprocess.CalledProcessError as e:
    print(f"Pipeline 執行失敗，錯誤碼: {e.returncode}")
    print(f"錯誤輸出: {e.stderr}")
except FileNotFoundError:
    print("命令未找到，請檢查路徑或命令名稱。")
```

### 3-2-3. 程序管理總結

**`os` 家族與 `subprocess` 模組比較**

| feature      | `os.system()` / `os.popen()`                         | `subprocess` module (`.run()`, `.Popen()`)    |
| :----------- | :--------------------------------------------------- | :-------------------------------------------- |
| **功能**     | 簡單執行命令，功能有限                               | 功能強大，完全控制子程序                      |
| **輸出捕獲** | `os.system()` 無法捕獲，`os.popen()` 只能捕獲 stdout | 可同時捕獲 stdout 和 stderr                   |
| **錯誤處理** | 僅回傳退出碼，需手動檢查                             | 可自動檢查退出碼並拋出異常 (`check=True`)     |
| **安全性**   | 易受 shell 注入攻擊                                  | 預設更安全 (使用列表傳遞參數)                 |
| **靈活性**   | 較差                                                 | 最佳 (可控制 stdin/stdout/stderr，非阻塞執行) |
| **推薦程度** | 不推薦用於新程式碼                                   | **強烈推薦**                                  |

在 Python 中執行外部命令時，`subprocess` 模組是現代且推薦的選擇，特別是 `subprocess.run()` 函數，它提供了安全、靈活且功能完整的介面來管理子程序。應盡量避免使用 `os.system()` 和 `os.popen()`，除非是極其簡單且對安全性要求不高的情境。

### 3-3-4. 程序管理練習

1.  **執行並檢查 Git 版本**：
    撰寫一個 Python 程式，使用 `subprocess.run()` 執行 `git --version` 命令。

    - 如果 Git 已安裝，請列印其版本資訊。
    - 如果 Git 未安裝或命令執行失敗，請捕獲異常並列印錯誤訊息。
      請確保捕獲標準輸出並以文字形式顯示。

2.  **列出檔案並過濾**：
    撰寫一個 Python 程式，使用 `subprocess.run()` 執行 `ls -l` (或 `dir` 在 Windows 上) 命令，並將其輸出捕獲。然後，使用 Python 的字串處理功能，從輸出中找出所有包含 "txt" 字樣的檔案或目錄行。

---

## 3-3. 多執行緒 (multithreading)

多執行緒 (Multithreading) 是指在單一程序中同時執行多個執行緒 (threads) 的能力。每個執行緒都是一個獨立的執行流程，它們共享同一個程序的記憶體空間，這使得執行緒之間的資料共享比多程序 (multiprocessing) 更為方便。然而，這也帶來了資料同步的挑戰。

Python 的 `threading` 模組提供了建立和管理執行緒的工具。

### 3-3-1. 子執行緒 (Child Thread)

在主程式流程 (主執行緒) 中，建立並啟動一個新的執行緒 (子執行緒) 來執行特定的任務，而主執行緒可以繼續執行其他工作。

**範例**

```python
import threading
import time

# 這是子執行緒要執行的工作
def job():
  for i in range(5):
    print("Child thread:", i)
    time.sleep(1)

# 1. 建立一個 Thread 物件
#    - target=job 指定了這個執行緒要執行的函式
t = threading.Thread(target = job)

# 2. 啟動執行緒
#    - .start() 會讓子執行緒開始執行 job() 函式
t.start()

# 主執行緒繼續執行自己的工作
for i in range(3):
  print("Main thread:", i)
  time.sleep(1)

# 3. 等待子執行緒結束
#    - .join() 會讓主執行緒停在這裡，直到 t 這個子執行緒執行完畢
t.join()

print("Done.")
```

### 3-3-2. 多個子執行緒 (Multi Child Thread)

建立並執行多個子執行緒，讓它們並行處理任務。

**範例**

```python
import threading
import time

# 執行緒要執行的工作，接收一個參數
def job(num):
  print("Thread", num)
  time.sleep(1)

threads = []
# 1. 建立 5 個執行緒
for i in range(5):
  # 建立 Thread 物件，並使用 args 傳遞參數
  # 注意：args 必須是一個 tuple，所以單一參數也要寫成 (i,)
  threads.append(threading.Thread(target = job, args = (i,)))
  # 2. 立即啟動執行緒
  threads[i].start()

# 主執行緒可以繼續執行自己的工作
# ...

# 3. 等待所有子執行緒結束
for i in range(5):
  threads[i].join()

print("Done.")
```

### 3-3-3. 使用類別建立執行緒 (With Class)

除了將函式作為 `target`，也可以透過繼承 `threading.Thread` 類別並覆寫 `run()` 方法來定義執行緒的行為。這是一種更具物件導向風格的作法。

**範例**

```python
import threading
import time

# 1. 繼承 threading.Thread 來建立自訂的執行緒類別
class MyThread(threading.Thread):
  def __init__(self, num):
    # 必須先呼叫父類別的 __init__
    threading.Thread.__init__(self)
    self.num = num

  # 2. 覆寫 run() 方法
  #    當呼叫 .start() 時，這個 run() 方法會被執行
  def run(self):
    print("Thread", self.num)
    time.sleep(1)

threads = []
for i in range(5):
  threads.append(MyThread(i))
  threads[i].start()

for i in range(5):
  threads[i].join()

print("Done.")
```

### 3-3-4. 佇列 (Queue)

`queue` 模組提供了一個執行緒安全的佇列 `Queue`，是實現多執行緒之間資料交換和通訊的常用工具。它遵循「先進先出」(FIFO, First-In-First-Out) 的原則。因為它是執行緒安全的，所以不需要額外的鎖 (Lock) 來保護。

**範例**

```python
import time
import threading
import queue

# Worker 類別，代表一個工作執行緒
class Worker(threading.Thread):
  def __init__(self, queue, num):
    threading.Thread.__init__(self)
    self.queue = queue
    self.num = num

  def run(self):
    # 檢查佇列中是否還有項目
    # 注意：在複雜應用中，qsize() > 0 不是最可靠的方式
    # 更好的方式是使用 while True 搭配哨兵值 (sentinel value) 或 get 的 timeout
    while self.queue.qsize() > 0:
      # 1. 從佇列中取得一個項目
      #    如果佇列是空的，.get() 預設會阻塞，直到有項目可用
      msg = self.queue.get()

      # 處理資料
      print("Worker %d: %s" % (self.num, msg))
      time.sleep(1)

# 建立一個 Queue 物件
my_queue = queue.Queue()

# 2. 將資料放入佇列
for i in range(10):
  my_queue.put("Data %d" % i)

# 建立兩個 Worker 執行緒
my_worker1 = Worker(my_queue, 1)
my_worker2 = Worker(my_queue, 2)

# 啟動 Worker
my_worker1.start()
my_worker2.start()

# 等待 Worker 結束
my_worker1.join()
my_worker2.join()

print("Done.")
```

- **`queue.Queue()`**: 建立一個執行緒安全的佇列。
- **`my_queue.put(item)`**: 將一個項目放入佇列的尾部。
- **`my_queue.get()`**: 從佇列的頭部取出一個項目。如果佇列為空，此操作會阻塞執行緒，直到有新的項目被放入。
- **`my_queue.qsize()`**: 返回佇列中大約的項目數量。

這個範例展示了典型的「生產者-消費者」模式：主執行緒是生產者，負責將資料放入佇列；`Worker` 執行緒是消費者，負責從佇列中取出資料並處理。

### 3-3-5. 鎖 (Lock)

當多個執行緒需要存取或修改同一個共享資源 (例如一個變數、檔案或裝置) 時，可能會發生「競爭條件」(Race Condition)，導致資料不一致或程式崩潰。`Lock` (鎖) 是一種同步機制，用來保護「臨界區段」(Critical Section) — 即一次只能被一個執行緒執行的程式碼區塊。

**範例**

```python
import time
import threading
import queue

class Worker(threading.Thread):
  def __init__(self, queue, num, lock):
    threading.Thread.__init__(self)
    self.queue = queue
    self.num = num
    self.lock = lock

  def run(self):
    while self.queue.qsize() > 0:
      msg = self.queue.get()

      # 1. 取得鎖
      #    如果鎖已被其他執行緒持有，此處會阻塞，直到鎖被釋放
      self.lock.acquire()
      print("Lock acquired by Worker %d" % self.num)

      # --- 臨界區段開始 ---
      # 這段程式碼一次只能由一個持有鎖的執行緒執行
      print("Worker %d: %s" % (self.num, msg))
      time.sleep(1)
      # --- 臨界區段結束 ---

      # 2. 釋放鎖
      #    讓其他等待中的執行緒可以取得鎖
      print("Lock released by Worker %d" % self.num)
      self.lock.release()

my_queue = queue.Queue()
for i in range(5):
  my_queue.put("Data %d" % i)

# 建立一個 Lock 物件
lock = threading.Lock()

my_worker1 = Worker(my_queue, 1, lock)
my_worker2 = Worker(my_queue, 2, lock)

my_worker1.start()
my_worker2.start()

my_worker1.join()
my_worker2.join()

print("Done.")
```

- **`threading.Lock()`**: 建立一個鎖物件。
- **`lock.acquire()`**: 嘗試取得鎖。如果鎖是空閒的，執行緒會立即獲得鎖並繼續執行；如果鎖已被其他執行緒持有，則當前執行緒會被阻塞，直到鎖被釋放。
- **`lock.release()`**: 釋放鎖。這應該在臨界區段執行完畢後呼叫。
- **更安全的作法**: 使用 `with` 陳述式可以自動管理鎖的取得與釋放，即使在臨界區段發生錯誤也能確保鎖被釋放。
  ```python
  with self.lock:
      # 臨界區段的程式碼
      print("Worker %d: %s" % (self.num, msg))
      time.sleep(1)
  # 離開 with 區塊時，鎖會自動被釋放
  ```

### 3-3-6. 號誌 (Semaphore)

`Semaphore` (號誌) 是一個更廣義的鎖。它內部維護一個計數器，`acquire()` 會使計數器減一，`release()` 會使計數器加一。當計數器為 0 時，`acquire()` 會阻塞。

這允許有限數量的執行緒同時存取一個資源。例如，如果有一個資源池 (如資料庫連線池) 只能同時處理 2 個連線，就可以使用一個初始值為 2 的 `Semaphore`。

**範例**

```python
import time
import threading
import queue

class Worker(threading.Thread):
  def __init__(self, queue, num, semaphore):
    threading.Thread.__init__(self)
    self.queue = queue
    self.num = num
    self.semaphore = semaphore

  def run(self):
    while self.queue.qsize() > 0:
      msg = self.queue.get()

      # 1. 取得號誌
      #    如果號誌的內部計數器大於 0，則計數器減 1，執行緒繼續。
      #    如果計數器為 0，執行緒阻塞，直到有其他執行緒釋放號誌。
      self.semaphore.acquire()
      print("Semaphore acquired by Worker %d" % self.num)

      # 這段程式碼最多只允許指定數量的執行緒同時進入
      print("Worker %d: %s" % (self.num, msg))
      time.sleep(1)

      # 2. 釋放號誌
      #    使號誌的內部計數器加 1，喚醒一個等待中的執行緒。
      print("Semaphore released by Worker %d" % self.num)
      self.semaphore.release()

my_queue = queue.Queue()
for i in range(5):
  my_queue.put("Data %d" % i)

# 建立一個 Semaphore，允許最多 2 個執行緒同時存取
semaphore = threading.Semaphore(2)

# 建立 3 個 Worker
my_worker1 = Worker(my_queue, 1, semaphore)
my_worker2 = Worker(my_queue, 2, semaphore)
my_worker3 = Worker(my_queue, 3, semaphore)

my_worker1.start()
my_worker2.start()
my_worker3.start()

my_worker1.join()
my_worker2.join()
my_worker3.join()

print("Done.")
```

- **`threading.Semaphore(value)`**: 建立一個號誌物件，`value` 是計數器的初始值，代表同時允許的執行緒數量。
- 在這個範例中，雖然有 3 個 `Worker`，但由於 `Semaphore` 的值是 2，所以任何時候最多只有 2 個 `Worker` 能同時執行 `print` 和 `sleep` 的區塊。

### 3-3-7. 可重入鎖 (RLock)

`RLock` (Re-entrant Lock) 是一種特殊的可重入鎖。一般的 `Lock` 不允許同一個執行緒在尚未釋放鎖的情況下再次取得它，這樣做會導致死鎖 (Deadlock)。而 `RLock` 允許同一個執行緒多次 `acquire()` 它，但必須有對應次數的 `release()` 才能將鎖完全釋放，供其他執行緒使用。

這在遞迴函式中特別有用，當一個函式需要遞迴呼叫自己，並且在每次呼叫時都需要持有同一個鎖時，就必須使用 `RLock`。

**範例**

```python
import threading

# 建立一個 RLock
rlock = threading.RLock()

def recursive_function(n):
  # 使用 with 陳述式來自動管理 RLock
  with rlock:
    print(f"Thread {threading.current_thread().name}: Acquired lock, n = {n}")
    if n > 0:
      # 在持有鎖的情況下，遞迴呼叫自己
      # 如果這是普通的 Lock，這裡會發生死鎖
      recursive_function(n - 1)
    print(f"Thread {threading.current_thread().name}: Releasing lock, n = {n}")

# 建立並啟動執行緒
thread = threading.Thread(target=recursive_function, args=(3,))
thread.start()
thread.join()

print("Done.")
```

- **`threading.RLock()`**: 建立一個可重入鎖。
- 在範例中，`recursive_function` 在每次進入時都會 `acquire` `rlock`。因為是 `RLock`，所以同一個執行緒可以成功地再次取得它。
- 只有當最外層的 `with` 區塊結束時 (即 `n=3` 的那次呼叫結束)，鎖才會被完全釋放。

### 3-3-8. 執行緒池 (ThreadPool)

手動建立、啟動和管理大量的執行緒是很繁瑣且低效率的。`ThreadPool` (執行緒池) 是一種管理模式，它會預先建立一定數量的執行緒放在一個「池」中。當有任務需要執行時，就從池中取一個空閒的執行緒來執行，任務結束後，執行緒會返回池中等待下一個任務，而不是被銷毀。

這樣做的好處是：

1.  **減少開銷**: 避免了頻繁建立和銷毀執行緒的系統開銷。
2.  **控制併發數**: 可以限制同時執行的執行緒數量，防止因執行緒過多而耗盡系統資源。

Python 3.2 之後，推薦使用 `concurrent.futures` 模組中的 `ThreadPoolExecutor` 來實現執行緒池。

**範例**

```python
import concurrent.futures
import time

def do_job(seconds):
  """一個耗時的任務"""
  print(f"Starting job that takes {seconds} second(s)...")
  time.sleep(seconds)
  return f"Job finished after {seconds} second(s)."

# 1. 建立一個 ThreadPoolExecutor
#    max_workers=3 表示執行緒池中最多有 3 個執行緒
#    使用 with 陳述式可以確保在結束時自動關閉執行緒池 (executor.shutdown())
with concurrent.futures.ThreadPoolExecutor(max_workers=3) as executor:

  # 2. 提交任務到執行緒池
  #    executor.submit(fn, *args, **kwargs) 會立即返回一個 Future 物件
  tasks = [2, 3, 1, 4]
  future_to_task = {executor.submit(do_job, task): task for task in tasks}

  # 3. 取得任務結果
  #    concurrent.futures.as_completed(futures) 會在任務完成時逐一返回 Future 物件
  for future in concurrent.futures.as_completed(future_to_task):
    task = future_to_task[future]
    try:
      # .result() 會取得任務的返回值
      # 如果任務執行中發生例外，.result() 會將其重新拋出
      result = future.result()
      print(f"Task {task} completed with result: '{result}'")
    except Exception as exc:
      print(f"Task {task} generated an exception: {exc}")

print("All tasks submitted.")
```

- **`ThreadPoolExecutor(max_workers=N)`**: 建立一個最多包含 `N` 個工作執行緒的執行緒池。
- **`executor.submit(func, ...)`**: 將一個任務 (`func`) 提交給執行緒池執行。它會非同步地執行，並立即返回一個 `Future` 物件。
- **`Future` 物件**: 代表一個尚未完成的計算。你可以用它來檢查任務狀態 (`.done()`) 或取得結果 (`.result()`)。呼叫 `.result()` 會阻塞，直到任務完成並返回結果。
- **`concurrent.futures.as_completed(futures)`**: 這是一個非常有用的函式，它接收一個 `Future` 物件的集合，並在任何一個 `Future` 完成時，就用 `yield` 將其返回。

### 3-3-9. 多執行緒總結

本章節介紹了 Python 中多執行緒程式設計的核心概念：

1.  **建立執行緒**: 可以透過 `threading.Thread(target=...)` 或繼承 `threading.Thread` 並覆寫 `run()` 方法來建立執行緒。
2.  **執行緒管理**: 使用 `start()` 啟動執行緒，使用 `join()` 等待執行緒完成。
3.  **執行緒通訊**: `queue.Queue` 是在執行緒之間安全地傳遞資料的首選方式。
4.  **執行緒同步**:
    - `Lock`: 用於保護共享資源，確保一次只有一個執行緒能存取臨界區段。
    - `RLock`: 可重入鎖，適用於同一個執行緒需要多次取得同一個鎖的遞迴場景。
    - `Semaphore`: 允許多個執行緒同時存取資源，但會限制最大數量。
5.  **高效管理**: `concurrent.futures.ThreadPoolExecutor` 是現代 Python 中管理執行緒池、簡化非同步任務提交與結果獲取的高階工具，應優先使用。

多執行緒程式設計的關鍵挑戰在於正確地管理共享狀態和同步，以避免競爭條件和死鎖。理解並善用 `Lock`, `Queue` 等工具是寫出健壯多執行緒程式的基礎。

### 3-3-10. 多執行緒練習

**目標**: 模擬一個多執行緒的網頁爬蟲。

1.  **建立一個任務佇列**: 使用 `queue.Queue` 建立一個佇列，並在其中放入 10 個假的 URL (例如 `"http://example.com/page/1"`, `"http://example.com/page/2"`, ...)。
2.  **建立一個結果列表**: 建立一個普通的 Python `list` 來存放爬取結果。
3.  **建立一個鎖**: 建立一個 `threading.Lock`，用來保護對結果列表的寫入操作。
4.  **建立工作函式**:
    - 定義一個 `worker` 函式，它接收任務佇列、結果列表和鎖作為參數。
    - 在函式中，寫一個 `while True` 迴圈，不斷從佇列中用 `queue.get(timeout=1)` 取出 URL。如果超時 (表示佇列空了)，就跳出迴圈。
    - 模擬爬取過程：`time.sleep(random.uniform(0.5, 1.5))`。
    - 爬取完成後，取得鎖 (`lock.acquire()`)，將結果 (例如一個 f-string: `f"Crawled {url}"`) 加入到結果列表中，然後釋放鎖 (`lock.release()`)。
5.  **使用 `ThreadPoolExecutor`**:
    - 建立一個 `ThreadPoolExecutor`，設定 `max_workers=3`。
    - 使用 `executor.submit()` 提交 3 個 `worker` 任務到執行緒池。
6.  **等待與驗證**:
    - 在 `with` 區塊結束後 (執行緒池已關閉)，印出結果列表的長度，確認是否為 10。

這個練習將綜合運用 `Queue`, `Lock`, 和 `ThreadPoolExecutor`。

---

## 3-4. 多程序 (multiprocessing)

多程序 (Multiprocessing) 是指在作業系統中同時執行多個獨立的程序 (processes)。與共享記憶體的多執行緒不同，每個程序都擁有自己獨立的記憶體空間、資源和 Python 直譯器。

`multiprocessing` 模組的主要優勢在於它能夠**完全繞過 Python 的全域直譯器鎖 (Global Interpreter Lock, GIL)**。這意味著它可以利用多核心 CPU 實現真正的平行計算，特別適合 CPU 密集型的任務。

**與 `threading` 的主要區別**:

- **記憶體**: 程序之間不共享記憶體，資料傳遞需要透過序列化 (pickling) 進行，例如使用 `Queue` 或 `Pipe`。
- **GIL**: `multiprocessing` 不受 GIL 限制，可以實現真正的 CPU 平行運算。`threading` 則受 GIL 限制，無法在單一程序中實現 CPU 平行運算。
- **資源開銷**: 建立和管理程序的系統開銷遠大於執行緒。
- **適用場景**: `multiprocessing` 適用於 **CPU 密集型**任務 (如科學計算、資料處理)，而 `threading` 適用於 **I/O 密集型**任務 (如網路請求、檔案讀寫)。

**重要提示**: 在使用 `multiprocessing` 時，必須將主程式的執行程式碼放在 `if __name__ == '__main__':` 區塊內。這是為了防止子程序在被建立時，又重新匯入並執行主程式的程式碼，從而導致無限遞迴地建立子程序。

### 3-4-1. 子程序 (Child Process)

與 `threading` 類似，可以在主程式中建立並啟動一個新的子程序來執行特定任務。

**範例**

```python
import multiprocessing
import time
import os

def job():
  # os.getpid() 可以取得當前程序的 ID
  print(f"Child process (PID: {os.getpid()}) starts.")
  time.sleep(2)
  print(f"Child process (PID: {os.getpid()}) finishes.")

if __name__ == '__main__':
  print(f"Main process (PID: {os.getpid()}) starts.")

  # 1. 建立一個 Process 物件
  p = multiprocessing.Process(target=job)

  # 2. 啟動程序
  p.start()

  # 主程序繼續執行
  print("Main process continues to do other things.")

  # 3. 等待子程序結束
  p.join()

  print("Done.")
```

- **`multiprocessing.Process`**: 用於建立程序物件，其介面與 `threading.Thread` 幾乎相同。
- **`if __name__ == '__main__':`**: 保護主程式碼，確保只有在直接執行此腳本時才會建立子程序。
- **`p.start()`**: 啟動一個新的子程序來執行 `target` 指定的函式。
- **`p.join()`**: 阻塞主程序，直到子程序 `p` 執行完畢。

### 3-4-2. 多個子程序 (Multi Child Process)

建立並執行多個子程序，讓它們在不同的 CPU 核心上平行處理任務。

**範例**

```python
import multiprocessing
import time
import os

def job(num):
  print(f"Process {num} (PID: {os.getpid()}) starts.")
  time.sleep(1)

if __name__ == '__main__':
  processes = []
  # 1. 建立 5 個程序
  for i in range(5):
    p = multiprocessing.Process(target=job, args=(i,))
    processes.append(p)
    # 2. 啟動程序
    p.start()

  # 3. 等待所有子程序結束
  for p in processes:
    p.join()

  print("Done.")
```

- 程式碼結構與多執行緒版本幾乎完全相同，只是將 `threading.Thread` 換成了 `multiprocessing.Process`。
- 每個 `job` 函式都會在一個全新的、獨立的程序中執行。

---

### 3-4-3. 使用類別建立程序 (With Class)

同樣地，可以透過繼承 `multiprocessing.Process` 並覆寫 `run()` 方法來定義程序的行為。

**範例**

```python
import multiprocessing
import time
import os

class MyProcess(multiprocessing.Process):
  def __init__(self, num):
    super().__init__() # 呼叫父類別的 __init__
    self.num = num

  def run(self):
    print(f"Process {self.num} (PID: {os.getpid()}) starts.")
    time.sleep(1)

if __name__ == '__main__':
  processes = []
  for i in range(5):
    p = MyProcess(i)
    processes.append(p)
    p.start()

  for p in processes:
    p.join()

  print("Done.")
```

- 繼承 `multiprocessing.Process` 並覆寫 `run()` 方法。
- 當呼叫 `start()` 時，`run()` 方法內的程式碼會在新的子程序中被執行。

---

### 3-4-4. 佇列 (Queue)

`multiprocessing.Queue` 是用於跨程序通訊的主要工具之一。與 `threading.Queue` 不同，放入 `multiprocessing.Queue` 的物件在內部會被**序列化 (pickled)**，傳送到另一個程序後再**反序列化 (unpickled)**。這表示只有可序列化的物件才能在程序之間傳遞。

**範例**

```python
import multiprocessing
import time

def producer(queue):
  """生產者，將資料放入佇列"""
  print("Producer starts.")
  for i in range(10):
    item = f"Data {i}"
    queue.put(item)
    print(f"Produced: {item}")
  queue.put(None) # 使用 None 作為結束信號
  print("Producer finished.")

def consumer(queue):
  """消費者，從佇列取出資料"""
  print("Consumer starts.")
  while True:
    item = queue.get()
    if item is None: # 收到結束信號
      break
    print(f"Consumed: {item}")
    time.sleep(0.5)
  print("Consumer finished.")

if __name__ == '__main__':
  # 建立一個 Queue 物件
  q = multiprocessing.Queue()

  # 建立並啟動生產者和消費者程序
  p_producer = multiprocessing.Process(target=producer, args=(q,))
  p_consumer = multiprocessing.Process(target=consumer, args=(q,))

  p_producer.start()
  p_consumer.start()

  p_producer.join()
  p_consumer.join()

  print("Done.")
```

- **`multiprocessing.Queue()`**: 建立一個跨程序的佇列。
- **哨兵值 (Sentinel Value)**: 在這個範例中，我們使用 `None` 作為一個特殊的「結束信號」。當消費者收到 `None` 時，就知道所有資料都已處理完畢，可以安全地退出迴圈。這比檢查 `qsize()` 更為可靠。

### 3-4-5. 鎖 (Lock)

`multiprocessing.Lock` 用於同步不同程序，防止它們同時存取共享資源。雖然程序有獨立的記憶體，但它們仍然可能存取共享的外部資源，例如檔案、資料庫或由 `multiprocessing.Value` / `multiprocessing.Array` 建立的共享記憶體。

**範例**

```python
import multiprocessing
import time

def worker(lock, shared_value, process_id):
  with lock:
    print(f"Process {process_id} acquired lock.")
    # 存取共享記憶體
    current_val = shared_value.value
    time.sleep(0.1)
    shared_value.value = current_val + 1
    print(f"Process {process_id} updated value to {shared_value.value}")
    print(f"Process {process_id} releases lock.")

if __name__ == '__main__':
  # 建立一個 Lock 物件
  lock = multiprocessing.Lock()
  # 建立一個共享的整數值，初始為 0
  counter = multiprocessing.Value('i', 0)

  processes = []
  for i in range(10):
    p = multiprocessing.Process(target=worker, args=(lock, counter, i))
    processes.append(p)
    p.start()

  for p in processes:
    p.join()

  print(f"Final counter value: {counter.value}")
  print("Done.")
```

- **`multiprocessing.Value(typecode, value)`**: 建立一個可以在程序之間共享的記憶體物件。`'i'` 代表一個帶正負號的整數。
- **`with lock:`**: `multiprocessing.Lock` 同樣支援 `with` 陳述式，這是管理鎖的最安全方式。如果沒有鎖，多個程序同時對 `counter` 進行「讀取-修改-寫入」操作，會導致競爭條件，最終結果將小於 10。

### 3-4-6. 號誌 (Semaphore)

`multiprocessing.Semaphore` 的作用與 `threading.Semaphore` 相同，但它是跨程序的。它允許有限數量的程序同時進入一個臨界區段。

**範例**

```python
import multiprocessing
import time

def worker(semaphore, process_id):
  with semaphore:
    print(f"Process {process_id} acquired semaphore.")
    time.sleep(1)
    print(f"Process {process_id} releases semaphore.")

if __name__ == '__main__':
  # 建立一個 Semaphore，允許最多 3 個程序同時執行
  semaphore = multiprocessing.Semaphore(3)

  processes = []
  for i in range(10):
    p = multiprocessing.Process(target=worker, args=(semaphore, i))
    processes.append(p)
    p.start()

  for p in processes:
    p.join()

  print("Done.")
```

- **`multiprocessing.Semaphore(3)`**: 建立一個初始計數為 3 的號誌。
- 在這個範例中，雖然我們啟動了 10 個程序，但任何時候最多只有 3 個程序能處於 `with semaphore:` 區塊內。

### 3-4-7. 可重入鎖 (RLock)

`multiprocessing.RLock` 是一個可重入鎖，其行為與 `threading.RLock` 類似，但作用於程序之間。它允許同一個程序多次取得鎖，而不會造成死鎖。

**範例**

```python
import multiprocessing
import os

def recursive_worker(lock, n):
  with lock:
    print(f"Process {os.getpid()} acquired lock, n = {n}")
    if n > 0:
      recursive_worker(lock, n - 1)
    print(f"Process {os.getpid()} releasing lock, n = {n}")

if __name__ == '__main__':
  # 建立一個 RLock 物件
  rlock = multiprocessing.RLock()

  p = multiprocessing.Process(target=recursive_worker, args=(rlock, 3))
  p.start()
  p.join()

  print("Done.")
```

- 這個範例展示了在遞迴函式中使用 `RLock`。如果換成 `multiprocessing.Lock`，程序會在第二次 `acquire` 時永遠阻塞，造成死鎖。

### 3-4-8. 程序池 (Process Pool)

`multiprocessing.Pool` 是管理一組工作程序 (Worker Process) 的便捷方式。它提供了一個簡單的介面，可以將任務分配給程序池中的工作程序執行，非常適合處理「資料平行」(data parallelism) 的問題，例如對一個列表中的每個元素執行相同的操作。

**範例**

```python
import multiprocessing
import time

def cpu_intensive_task(x):
  """一個模擬 CPU 密集型計算的任務"""
  print(f"Processing {x}...")
  # 這裡可以是一些複雜的計算
  time.sleep(1)
  return x * x

if __name__ == '__main__':
  # 1. 建立一個程序池
  #    如果未指定數量，預設為系統的 CPU 核心數 (os.cpu_count())
  with multiprocessing.Pool(processes=4) as pool:

    tasks = range(10)

    # 2. 將任務提交給程序池
    #    pool.map 會將 tasks 中的每個元素作為參數傳給 cpu_intensive_task
    #    它會阻塞，直到所有任務完成，並返回一個包含所有結果的列表
    results = pool.map(cpu_intensive_task, tasks)

    print("All tasks finished.")
    print("Results:", results)

  print("Done.")
```

- **`multiprocessing.Pool(processes=N)`**: 建立一個包含 `N` 個工作程序的程序池。
- **`pool.map(func, iterable)`**: 這是一個非常強大的方法。它會將 `iterable` 中的每一個元素作為參數，傳遞給 `func` 函式，並行地在程序池中執行。它會保持結果的順序，並在所有任務完成後返回結果列表。
- **`with ... as pool:`**: 使用 `with` 陳述式可以確保在區塊結束時，程序池會被正確地關閉 (`pool.close()`) 和清理 (`pool.join()`)。

### 3-4-9. 多程序總結

`multiprocessing` 是 Python 中實現真正平行計算的標準工具，是解決 CPU 密集型問題的利器。

1.  **核心優勢**: 透過建立獨立程序來繞過 GIL，充分利用多核心 CPU。
2.  **API 相似性**: 其 API 與 `threading` 模組高度相似，降低了學習成本。
3.  **資料通訊**: 程序間通訊 (`Queue`, `Pipe`) 比執行緒間通訊更重，因為涉及資料序列化。
4.  **`if __name__ == '__main__':`**: 使用 `multiprocessing` 時必須的保護措施。
5.  **程序池 (`Pool`)**: 對於資料平行任務，`Pool` 提供了極其方便和高效的高階抽象，是首選工具。

**選擇 `multiproaching` 還是 `threading`？**

- 當你的任務是 **CPU 密集型** (例如：影像處理、數學運算、機器學習模型訓練的某些部分)，並且你希望在多核心機器上獲得顯著的加速時，請選擇 **`multiprocessing`**。
- 當你的任務是 **I/O 密集型** (例如：同時下載多個網頁、讀寫多個檔案、查詢資料庫)，程式大部分時間都在等待外部資源回應時，請選擇 **`threading`**，因為它的資源開銷更小。

### 3-4-10. 多程序練習

**目標**: 使用 `multiprocessing.Pool` 平行計算一個數字列表中每個數字的階乘。

1.  **建立一個計算函式**:
    - 定義一個函式 `calculate_factorial(n)`，它接收一個整數 `n`，計算並返回 `n` 的階乘。為了模擬耗時，可以在函式中加入 `time.sleep(0.01)`。
2.  **準備資料**:
    - 建立一個包含 20 個隨機數字 (例如 10 到 20 之間) 的列表。
3.  **使用程序池**:
    - 在 `if __name__ == '__main__':` 區塊內。
    - 建立一個 `multiprocessing.Pool`。
    - 使用 `pool.map()` 將計算任務分配給程序池。
4.  **輸出結果**:
    - 印出 `pool.map()` 返回的結果列表。
    - 可以同時記錄一下總執行時間，與單程序執行的時間做個對比。

這個練習可以直觀地感受到 `multiprocessing` 在處理 CPU 密集型任務時帶來的效能提升。

---

## 3-5. 平行化 (parallelism)

在探討 `multiprocessing` 和 `threading` 之前，先釐清兩個重要概念：

- **並行 (Concurrency)**: 指的是**看起來像**同時執行。系統擁有多個任務，並在它們之間快速切換，讓每個任務都有進展。這就像一個廚師同時在顧好幾個鍋子，他輪流翻炒每一個鍋，但任何一個瞬間他只專注於一個鍋。
- **平行 (Parallelism)**: 指的是**實際上**同時執行。系統擁有多個處理單元 (例如多核心 CPU)，可以讓多個任務在完全相同的時刻被執行。這就像廚房裡有好幾個廚師，每個人都有自己的爐子，他們可以同時炒菜。

在 Python 中，`threading` 主要實現了**並行**，而 `multiprocessing` 則實現了**平行**。

### 3-5-1. `threading` (多執行緒)

- **運作方式**: 在單一程序中建立多個執行緒，所有執行緒共享同一個記憶體空間。
- **關鍵限制：全域直譯器鎖 (Global Interpreter Lock, GIL)**: 在 CPython (最主流的 Python 直譯器) 中，GIL 是一把大鎖，它確保任何時候只有一個執行緒能執行 Python 的位元組碼 (bytecode)。這意味著，即使在多核心 CPU 上，Python 的多執行緒也無法實現 CPU 運算的真正平行。
- **優點**:
  1.  **低開銷**: 建立和切換執行緒的成本遠低於程序。
  2.  **資料共享方便**: 因為共享記憶體，執行緒之間可以直接讀寫同一個變數，溝通非常方便。
- **缺點**:
  1.  **無法利用多核心進行 CPU 運算**: 受到 GIL 的限制，對於 CPU 密集型任務 (如大量計算)，多執行緒甚至可能比單執行緒更慢 (因為有執行緒切換的開銷)。
  2.  **競爭條件 (Race Conditions)**: 因為共享資料太方便，所以很容易出現多個執行緒同時修改一個資料而導致資料錯亂的問題，必須小心使用 `Lock` 等同步工具。
- **最佳使用情境**: **I/O 密集型 (I/O-Bound) 任務**。
  - **說明**: 當一個任務需要等待外部資源時 (例如：讀寫檔案、請求網路資料、查詢資料庫)，執行緒會釋放 GIL，讓其他執行緒可以執行。這使得程式可以在等待 I/O 的空檔期去執行其他工作，從而大大提高整體效率。

### 3-5-2. `multiprocessing` (多程序)

- **運作方式**: 建立多個獨立的作業系統程序，每個程序都有自己獨立的記憶體空間和 Python 直譯器。
- **關鍵優勢：繞過 GIL**: 因為每個程序都有自己的直譯器和 GIL，所以它們之間互不影響，作業系統可以將它們分配到不同的 CPU 核心上，實現真正的平行運算。
- **優點**:
  1.  **能利用多核心進行 CPU 運算**: 對於 CPU 密集型任務，可以透過增加程序數量來獲得近乎線性的效能提升。
  2.  **資料隔離，更安全**: 每個程序都有獨立的記憶體，從根本上避免了多執行緒中常見的資料競爭問題。
- **缺點**:
  1.  **高開銷**: 建立和管理程序的系統開銷遠大於執行緒，佔用更多記憶體。
  2.  **資料共享複雜**: 程序間通訊 (IPC, Inter-Process Communication) 必須透過 `Queue`, `Pipe`, `Value` 等特殊機制進行。資料在傳遞時需要序列化 (pickling)，速度較慢。
- **最佳使用情境**: **CPU 密集型 (CPU-Bound) 任務**。
  - **說明**: 當一個任務需要大量的 CPU 計算時 (例如：科學運算、影像處理、資料分析)，`multiprocessing` 可以將任務拆分給多個程序，在多個核心上同時執行，從而顯著縮短總執行時間。

### 3-5-3. 優缺差異比較表

| 特性           | `threading` (多執行緒)      | `multiprocessing` (多程序)  |
| :------------- | :-------------------------- | :-------------------------- |
| **記憶體模型** | 共享記憶體                  | 獨立記憶體                  |
| **CPU 平行**   | **否** (受 GIL 限制)        | **是** (可利用多核心)       |
| **適用場景**   | **I/O 密集型** (網路、檔案) | **CPU 密集型** (計算、分析) |
| **資源開銷**   | 低                          | 高                          |
| **資料共享**   | 簡單、快速 (直接存取)       | 複雜、較慢 (需序列化)       |
| **風險**       | 競爭條件 (Race Conditions)  | 程序間通訊的複雜性          |

### 3-5-4. 同步工具常用情境

在並行或平行程式設計中，為了協調不同執行緒/程序的行為，避免混亂，我們需要使用同步工具。

#### 3-5-4-1. `Queue` (佇列)

- **核心作用**: 在生產者和消費者之間安全地傳遞資料。
- **常見情境**: **生產者-消費者模型 (Producer-Consumer Pattern)**。
- **舉例**:
  一個網路爬蟲程式。
  - **生產者 (Producer)**: 一個或多個執行緒/程序負責解析網頁，找到新的 URL，然後將這些 URL `put` 到一個共享的 `Queue` 中。
  - **消費者 (Consumer)**: 多個「工作」執行緒/程序從 `Queue` 中 `get` URL，然後去下載、解析這些頁面。
- **為何使用 `Queue`?**:
  - **解耦 (Decoupling)**: 生產者不需要知道是誰在消費，消費者也不需要知道是誰在生產。它們只透過 `Queue` 這個中間人溝通。
  - **執行緒/程序安全**: `Queue` 內部已經處理好了鎖定問題，你不需要手動加鎖就可以安全地在多個執行緒/程序中 `put` 和 `get`。
  - **流量緩衝**: 如果生產者的速度快於消費者，`Queue` 可以作為一個緩衝區，暫存任務，避免任務遺失。

#### 3-5-4-2. `Lock` (鎖)

- **核心作用**: 保護「臨界區段」(Critical Section)，確保同一時間只有一個執行緒/程序可以存取某個共享資源。這稱為**互斥 (Mutual Exclusion)**。
- **常見情境**: 任何「讀取-修改-寫入」一個共享變數的操作都需要被保護。
- **舉例**:
  1.  **共享計數器**: 一個網站需要計算總訪問次數。多個執行緒/程序同時處理用戶請求，並對一個全域變數 `total_visits` 進行 `+1` 操作。如果沒有 `Lock`，`total_visits += 1` 這個非原子操作會產生競爭條件，導致最終計數遠小於實際值。
      ```python
      # 臨界區段
      with lock:
          total_visits += 1
      ```
  2.  **寫入日誌檔案**: 多個執行緒/程序需要將日誌訊息寫入同一個檔案。如果沒有 `Lock`，來自不同執行緒的訊息可能會交錯在一起，導致日誌內容混亂不堪。
      ```python
      # 臨界區段
      with lock:
          logfile.write(f"[{timestamp}] {message}\n")
      ```

#### 3-5-4-3. `RLock` (可重入鎖)

- **核心作用**: `Lock` 的變體，允許**同一個**執行緒/程序在釋放鎖之前，**多次**取得該鎖。
- **常見情境**: 在遞迴或多層巢狀函式中，需要重複取得同一個鎖。
- **舉例**:
  一個物件的方法需要同步，且方法之間會互相呼叫。

  ```python
  class SyncedObject:
      def __init__(self):
          self.lock = threading.RLock()

      def method_A(self):
          with self.lock:
              print("Entered method_A")
              # ... do something ...
              self.method_B() # 呼叫另一個需要鎖的方法

      def method_B(self):
          with self.lock:
              print("Entered method_B")
              # ... do something else ...
  ```

- **為何使用 `RLock`?**:
  在上面的例子中，執行 `method_A` 時取得了鎖，然後它呼叫了 `method_B`，`method_B` 也嘗試取得同一個鎖。
  - 如果用的是 `Lock`，`method_B` 會永遠等待 `method_A` 釋放鎖，但 `method_A` 必須等 `method_B` 執行完才能釋放鎖，這就造成了**死鎖 (Deadlock)**。
  - 如果用的是 `RLock`，因為是同一個執行緒在操作，所以 `method_B` 可以成功地再次取得鎖。`RLock` 內部會維護一個計數，只有當最外層的 `with` 區塊結束時，鎖才會被真正釋放。

#### 3-5-4-4. `Semaphore` (號誌)

- **核心作用**: `Lock` 只允許 0 或 1 個執行緒進入，而 `Semaphore` 允許**有限數量** (N > 1) 的執行緒/程序同時進入。
- **常見情境**: 控制對一個有限資源池的存取。
- **舉例**:
  1.  **資料庫連線池**: 一個應用程式的資料庫連線池中只有 10 個可用連線。你可以使用 `Semaphore(10)` 來管理。任何時候，最多只有 10 個執行緒可以從池中取得連線。第 11 個執行緒在呼叫 `semaphore.acquire()` 時將會被阻塞，直到前面有執行緒完成工作並 `release()` 了號誌 (歸還了連線)。
  2.  **API 速率限制**: 你正在使用一個第三方 API，它限制你的帳號最多只能有 5 個並發請求。你可以用 `Semaphore(5)` 來包裝你的 API 呼叫函式，確保你的程式不會因為請求過多而被封鎖。

### 3-5-5. 總結

- `threading` 和 `multiprocessing` 提供了不同的併發/平行模型，前者適用於 I/O 密集型任務，後者適用於 CPU 密集型任務。
- `Queue`, `Lock`, `RLock`, `Semaphore` 是解決並行/平行程式設計中資料同步和資源管理問題的關鍵工具。
- **`Queue`** 用於**通訊**，**`Lock`** 用於**互斥**，**`RLock`** 用於**遞迴互斥**，**`Semaphore`** 用於**資源計數**。理解它們各自的適用情境是寫出健壯、高效的並行程式的基礎。
