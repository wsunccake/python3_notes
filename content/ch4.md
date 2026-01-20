# ch4. 其他主題2

深入探討 Python 3 中幾個重要的進階概念，包括迭代器 (iterator) 與可迭代物件 (iterable)、生成器 (generator) 相關的 `yield` 關鍵字及其變體，以及處理並行 (concurrent) 任務的同步 (sync) 與非同步 (async) 編程。這些概念對於編寫高效、資源友善且響應迅速的 Python 程式碼至關重要。

## 4-1. 迭代 / Iteration

在 Python 中，當需要遍歷（iteration）一系列元素時，`iterable` 和 `iterator` 是兩個核心概念。

### 4-1-1. 可迭代物件 / Iterable

- **意義**: `Iterable` 是一個可以被「迭代」的物件，意思是它可以一次提供一個元素。簡單來說，任何可以用 `for ... in ...` 迴圈遍歷的物件都是 `iterable`。
- **判斷方式**: 一個物件如果實作了 `__iter__()` 方法，並且該方法會回傳一個 `iterator`，那麼它就是 `iterable`。常見的 `iterable` 包括串列 (list)、元組 (tuple)、字串 (string)、字典 (dict) 和集合 (set)。
- **範例**:

  ```python
  my_list = [1, 2, 3, 4] # 串列是一個 iterable
  for item in my_list:
      print(item)

  my_string = "hello" # 字串也是一個 iterable
  for char in my_string:
      print(char)
  ```

### 4-1-2. 迭代器 / Iterator

- **意義**: `Iterator` 是一個「狀態記錄器」，它知道如何從 `iterable` 中取得下一個元素。`iterator` 本身會記住它目前遍歷到哪裡。
- **判斷方式**: 一個物件如果實作了 `__iter__()` 方法（回傳自身）和 `__next__()` 方法，那麼它就是 `iterator`。`__next__()` 方法在沒有更多元素時會拋出 `StopIteration` 異常。
- **如何在 Python 3 使用**:
  1.  使用內建函數 `iter()` 從 `iterable` 取得一個 `iterator`。
  2.  使用內建函數 `next()` 從 `iterator` 取得下一個元素。
- **範例**:

  ```python
  my_list = [10, 20, 30]

  # 步驟 1: 從 iterable 取得 iterator
  my_iterator = iter(my_list)
  print(f"取得的迭代器物件: {my_iterator}")

  # 步驟 2: 使用 next() 取得元素
  print(f"第一個元素: {next(my_iterator)}") # 輸出 10
  print(f"第二個元素: {next(my_iterator)}") # 輸出 20
  print(f"第三個元素: {next(my_iterator)}") # 輸出 30

  # 當沒有更多元素時，會拋出 StopIteration 異常
  try:
      next(my_iterator)
  except StopIteration:
      print("已經沒有更多元素了！")

  # for 迴圈的內部機制其實就是這樣運作的：
  # 1. 呼叫 iter() 取得迭代器
  # 2. 不斷呼叫 next() 取得元素，直到遇到 StopIteration
  ```

  ```python
  my_list = [10, 20, 30]
  my_iterator = iter(my_list)

  while True:
    try:
        # 取得下一個元素
        print(next(my_iterator))
    except StopIteration:
        # 當迭代器耗盡時，跳出迴圈
        break
  ```

```python
  my_list = [10, 20, 30]
  my_iterator = iter(my_list)

  for element in my_iterator:
        print(element)
```

#### 4-1-2-1. 使用類別 / Class 自定義迭代器

要讓一個類別成為迭代器，必須實作以下兩個方法：
**iter**()：回傳迭代器物件本身（通常是 return self）。
**next**()：回傳下一個值。如果沒有值了，必須拋出 StopIteration 異常。

```python
from collections.abc import Iterable, Iterator

class MyCounter:
    def __init__(self, low, high):
        self.current = low
        self.high = high

    def __iter__(self):
        return self

    def __next__(self):
        if self.current > self.high:
            raise StopIteration
        else:
            self.current += 1
            return self.current - 1

# 使用方式
counter = MyCounter(1, 3)

print("counter type: ", type(counter))
# 1. 使用 isinstance 檢查類型
print(f"Is counter an Iterable? {isinstance(counter, Iterable)}") # True
print(f"Is counter an Iterator? {isinstance(counter, Iterator)}") # True

# 2. 檢查特定的魔術方法
print(f"Has __iter__: {hasattr(counter, '__iter__')}") # True
print(f"Has __next__: {hasattr(counter, '__next__')}") # True

for num in counter:
    print(num)  # 輸出 1, 2, 3
```

#### 4-1-2-2. 使用生成器 / Generator

這是最推薦且最簡潔的方式。只要在函式中使用 yield 關鍵字，Python 就會自動將該函式轉化為一個生成器（它本質上就是一個迭代器）。

```python
from collections.abc import Iterable, Iterator

def my_generator(low, high):
    current = low
    while current <= high:
        yield current
        current += 1

gen = my_generator(1, 3)

print("gen type: ", type(gen))
# 1. 使用 isinstance 檢查類型
print(f"Is gen an Iterable? {isinstance(gen, Iterable)}") # True
print(f"Is gen an Iterator? {isinstance(gen, Iterator)}") # True

# 2. 檢查特定的魔術方法
print(f"Has __iter__: {hasattr(gen, '__iter__')}") # True
print(f"Has __next__: {hasattr(gen, '__next__')}") # True

for element in gen:
    print(element)
```

### 4-1-3. 迭代總結

- `Iterable` 是「可以被遍歷」的物件，例如串列、字串。
- `Iterator` 是「遍歷的工具」，它會記住遍歷的進度，並提供下一個元素。
- `iter()` 函數將 `iterable` 轉換為 `iterator`。
- `next()` 函數從 `iterator` 取得下一個元素。
- `for` 迴圈是 Python 處理迭代的語法糖，它自動處理了 `iter()` 和 `next()` 以及 `StopIteration` 異常。

### 4-1-4. 迭代練習

1.  **自定義 Iterable 和 Iterator**:
    - 創建一個名為 `MyRange` 的類別，使其行為類似於內建的 `range()` 函數。
    - `MyRange` 應該是一個 `iterable`，並且當對它呼叫 `iter()` 時，會回傳一個 `iterator`。
    - 這個 `iterator` 應該能夠從 `start` 數到 `end` (不包含 `end`)，每次遞增 `step`。
    - 範例使用:
      ```python
      for num in MyRange(1, 10, 2):
          print(num) # 應該輸出 1, 3, 5, 7, 9
      ```

---

## 4-2. 生成器 / yield

`yield` 關鍵字是 Python 中用於創建生成器 (generator) 的核心。生成器是一種特殊的迭代器，它允許在迭代過程中「暫停」函數的執行並回傳一個值，然後在下次請求時從上次暫停的地方繼續執行。

### 4-2-1. `yield`

- **意義**: `yield` 類似於 `return`，但它不會終止函數的執行。當函數執行到 `yield` 時，它會回傳一個值給呼叫者，並暫停函數的狀態。下次呼叫 `next()` 時，函數會從上次 `yield` 的地方繼續執行。
- **應用**: 常用於處理大量資料流、無限序列或需要延遲計算的場景，因為它以惰性 (lazy) 的方式產生值，節省記憶體。
- **範例**:

```python
def simple_generator():
    yield 1
    yield 2
    yield 3

# 呼叫生成器函數會回傳一個生成器物件 (generator object)
gen = simple_generator()
print(f"生成器物件: {gen}")

# 像迭代器一樣使用 next() 取得值
print(f"第一個值: {next(gen)}") # 輸出 1
print(f"第二個值: {next(gen)}") # 輸出 2
print(f"第三個值: {next(gen)}") # 輸出 3

try:
    next(gen)
except StopIteration:
    print("生成器已經沒有更多值了！")

# 生成器也可以直接用 for 迴圈遍歷
print("--- 使用 for 迴圈遍歷生成器 ---")
for value in simple_generator():
    print(value)
```

### 4-2-2. `yield from`

- **意義**: `yield from` 是 Python 3.3 引入的語法，用於將控制權委託給另一個生成器或可迭代物件。它簡化了從子生成器中 `yield` 值的過程。
- **應用**: 當一個生成器需要從另一個生成器中獲取所有值時，`yield from` 可以讓程式碼更簡潔。
- **範例**:

```python
def sub_generator():
    yield 'A'
    yield 'B'

def main_generator():
    yield 1
    yield from sub_generator() # 將控制權委託給 sub_generator
    yield 2

gen = main_generator()
for value in gen:
    print(value)
# 輸出:
# 1
# A
# B
# 2
```

### 4-2-3. `yield comprehension`

- **意義**: `yield comprehension` 實際上是指「生成器表達式 (generator expression)」。它類似於列表推導式 (list comprehension)，但使用圓括號 `()` 而不是方括號 `[]`，並且它不會立即構建整個列表，而是回傳一個生成器物件，以惰性方式產生值。
- **應用**: 當需要一個一次性使用的迭代器，且不希望佔用大量記憶體時，生成器表達式非常有用。
- **範例**:

```python
# 列表推導式 (立即生成所有值並存儲在記憶體中)
my_list_comp = [x * x for x in range(5)]
print(f"列表推導式結果: {my_list_comp}") # 輸出: [0, 1, 4, 9, 16]

# 生成器表達式 (回傳一個生成器物件，惰性生成值)
my_gen_exp = (x * x for x in range(5))
print(f"生成器表達式物件: {my_gen_exp}") # 輸出: <generator object <genexpr> at ...>

# 遍歷生成器表達式
print("--- 遍歷生成器表達式 ---")
for value in my_gen_exp:
    print(value)
# 輸出:
# 0
# 1
# 4
# 9
# 16
```

### 4-2-4. 生成器總結

- `yield` 讓函數變成生成器，實現惰性求值，節省記憶體。
- `yield from` 簡化了生成器之間的委託，讓程式碼更清晰。
- 生成器表達式 `(expression for item in iterable)` 是一種簡潔創建生成器的方式，適用於一次性迭代。

### 4-2-5. 生成器練習

1.  **使用 `yield` 創建檔案讀取生成器**:
    - 編寫一個生成器函數 `read_large_file(filepath)`，它接受一個檔案路徑。
    - 這個生成器函數應該每次讀取檔案的一行，並 `yield` 出去，而不是一次性將所有內容讀入記憶體。
    - 假設有一個 `large_file.txt` 包含多行文字，請示範如何使用這個生成器函數來遍歷檔案內容。

---

## 4-3. 並行處理 / concurrent

`concurrent` 在 Python 中指的是同時處理多個任務的能力。這可以透過多執行緒 (multithreading)、多進程 (multiprocessing) 或非同步 I/O (async I/O) 來實現。

### 4-3-1. `concurrent` 與 `parallelism`

- **Concurrent (並行)**: 指的是程式設計上處理多個任務的能力。任務可能在同一時間段內交錯執行（例如單核心 CPU 上的多執行緒），給人一種同時進行的錯覺。重點在於「處理」多個任務。
- **Parallelism (並行性)**: 指的是多個任務在同一時刻「真正地同時」執行。這通常需要多核心 CPU 或多台機器。重點在於「同時執行」。

簡單來說，並行是關於「結構」，並行性是關於「執行」。一個並行的程式不一定會並行執行，但一個並行執行的程式一定是並行的。

### 4-3-2. `sync` 與 `async`

- **Sync (同步)**: 程式碼按照順序一行一行執行。當遇到一個耗時的操作（例如讀取檔案、網路請求），程式會停下來等待該操作完成，然後才繼續執行下一行程式碼。這會導致程式在等待期間「阻塞 (blocking)」。
- **Async (非同步)**: 程式碼在執行耗時操作時，不會停下來等待。它會發出請求，然後繼續執行其他任務。當耗時操作完成時，程式會收到通知並處理結果。這可以避免程式阻塞，提高效率和響應性。

### 4-3-3. `async` 與 `await`

Python 3.5 引入了 `async` 和 `await` 關鍵字，使得非同步編程變得更加容易和直觀。它們通常與 `asyncio` 庫一起使用。

- **`async def`**: 用於定義一個協程 (coroutine) 函數。協程是一種特殊的函數，它可以在執行過程中暫停和恢復。
- **`await`**: 只能在 `async def` 函數內部使用。它用於「等待」一個非同步操作完成。當 `await` 一個協程時，當前協程會暫停執行，將控制權交還給事件迴圈 (event loop)，直到被等待的協程完成並回傳結果。

- **範例**:

```python
import asyncio
import time

async def task_a():
    print("任務 A 開始")
    await asyncio.sleep(2) # 模擬一個耗時 2 秒的非同步操作
    print("任務 A 結束")
    return "A 完成"

async def task_b():
    print("任務 B 開始")
    await asyncio.sleep(1) # 模擬一個耗時 1 秒的非同步操作
    print("任務 B 結束")
    return "B 完成"

async def main():
    start_time = time.time()
    # 同時運行多個協程
    result_a, result_b = await asyncio.gather(task_a(), task_b())
    end_time = time.time()

    print(f"所有任務完成，結果: {result_a}, {result_b}")
    print(f"總耗時: {end_time - start_time:.2f} 秒")

# 執行主協程
# 注意: 在 Jupyter Notebook 或某些環境中，可能需要使用 nest_asyncio
# import nest_asyncio
# nest_asyncio.apply()
# asyncio.run(main())

# 在一般 Python 腳本中直接執行
if __name__ == "__main__":
    asyncio.run(main())
```

**預期輸出**:

```
任務 A 開始
任務 B 開始
任務 B 結束
任務 A 結束
所有任務完成，結果: A 完成, B 完成
總耗時: 2.xx 秒 (因為兩個任務並行執行，總時間取決於最長的那個)
```

如果使用同步方式，總耗時將會是 2 + 1 = 3 秒。非同步方式則利用等待期間執行其他任務，提高了效率。

### 4-3-4. 並行處理總結

- `concurrent` 處理多任務，`parallelism` 真正同時執行多任務。
- `sync` 阻塞等待，`async` 不阻塞，提高響應性。
- `async def` 定義協程，`await` 等待非同步操作，配合 `asyncio` 實現非同步編程。

### 4-3-5. 並行處理練習

1.  **非同步任務排序**:
    - 創建三個非同步函數 `task_1()`, `task_2()`, `task_3()`。
    - `task_1` 模擬耗時 3 秒，`task_2` 模擬耗時 1 秒，`task_3` 模擬耗時 2 秒。
    - 編寫一個 `main_async()` 協程，使用 `asyncio.gather` 同時執行這三個任務。
    - 計算並印出總共花費的時間。
    - 思考：如果這些任務是同步執行的，總時間會是多少？非同步執行有何優勢？
