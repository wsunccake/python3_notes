# ch1. 基礎語法

## 1-1. Python 程式基本結構與註解

撰寫 Python 程式時，了解其基本結構和如何添加註解，是確保程式碼清晰、可讀且易於維護。

### 1-1-1. Python 程式結構

Python 程式碼由一系列的「陳述句 (statement)」組成，直譯器 (interpreter) 會由上到下逐行執行這些陳述句。與許多其他程式語言（如 C++ 或 Java）不同，Python 不需要用分號 (`;`) 來結束陳述句，也不需要用大括號 (`{}`) 來定義程式碼區塊。

#### 1-1-1-1. 程式碼區塊 (Code Blocks) 與縮排 (Indentation)

Python 最具特色的地方在於使用「縮排」來定義程式碼區塊的範圍。程式碼區塊是指一組應被視為一個單位的陳述句，例如 `if` 判斷式的主體、`for` 迴圈的內容，或是函式的定義。

- **規則**: 同一個程式碼區塊內的所有陳述句，必須具有相同層級的縮排。
- **習慣**: 根據 [PEP 8](https://peps.python.org/pep-0008/) (Python 官方風格指南)，建議使用 **4 個空格** 作為一個縮排層級。不建議使用 Tab 鍵，因為不同的編輯器對 Tab 的解釋可能不同，容易造成混亂。

```python
# 範例：一個簡單的 if-else 結構
weather = "sunny"

if weather == "sunny":
    # --- 開始 if 的程式碼區塊 (第一層縮排) ---
    print("It's a sunny day!")
    print("Let's go out and play.")
    # --- 結束 if 的程式碼區塊 ---
else:
    # --- 開始 else 的程式碼區塊 (第一層縮排) ---
    print("It's not sunny today.")
    print("Maybe stay indoors.")
    # --- 結束 else 的程式碼區塊 ---

print("Program finished.") # 這行沒有縮排，不屬於 if 或 else 區塊
```

- **意義**: 縮排在 Python 中是語法的一部分，而非僅僅為了美觀。不正確的縮排會導致 `IndentationError`，這是 Python 初學者最常遇到的錯誤之一。嚴格的縮排規則強迫開發者寫出結構清晰、可讀性高的程式碼。

### 1-1-2 .註解 (Comments)

註解是用於解釋程式碼的文字，它們會被 Python 直譯器忽略，不會被執行。良好的註解可以幫助理解程式碼的用途、邏輯或目的。

#### 1-1-2-1. 單行註解 (Single-line Comments)

- **語法**: 使用井字號 (`#`)。從 `#` 開始到該行結尾的所有內容都會被視為註解。

```python
# 這是一個單行註解，用來解釋下面這行程式碼的功能
name = "Alice"  # 也可以將註解放在程式碼的右側
```

#### 1-1-2-2. 多行註解 (Multi-line Comments)

Python 並沒有像其他語言那樣提供特定的多行註解語法。但有兩種常見的方式可以達到類似的效果：

1.  **使用多個單行註解**: 在每一行的開頭都加上 `#`。

    ```python
    # 這是一個多行註解的範例。
    # 每一行都以井字號開頭。
    # 這種方式很清晰，也是官方推薦的方式之一。
    ```

2.  **使用多行字串 (Docstrings)**: 使用三個單引號 (`'''...'''`) 或三個雙引號 (`"""..."""`) 包圍的文字。雖然這在技術上是一個未被賦值給任何變數的「字串」，但它常被用作多行註解。特別是當它出現在函式、類別或模組的第一行時，它會成為該物件的「文件字串 (docstring)」，可以透過特定工具提取出來作為說明文件。

    ```python
    def add(a, b):
        """
        這是一個文件字串 (docstring)。
        它描述了這個函式的功能：
        接收兩個數字 a 和 b，並返回它們的和。
        """
        return a + b

    '''
    這也是一個多行註解，
    但它不是 docstring，因為它不在函式或類別的開頭。
    直譯器會讀取這個字串，但因為沒有對它做任何操作，所以它會被忽略。
    '''
    ```

---

## 1-2. 資料型別 (Data Types)

在程式設計中，資料型別是用來分類或區分不同類型資料的屬性。Python 是一種「動態型別」語言，這意味著不需要在宣告變數時指定其型別，Python 直譯器會在執行時自動推斷。

可以使用內建函式 `type()` 來檢查任何變數或值的資料型別。

### 1-2-1. 基礎型別

#### 1-2-1-1. 整數 (int)

- **描述**: 用於表示沒有小數部分的數字，可以是正數、負數或零。
- **範例**:
  ```python
  age = 25
  temperature = -10
  count = 0
  print(type(age))  # <class 'int'>
  ```

#### 1-2-1-2. 浮點數 (float)

- **描述**: 用於表示帶有小數部分的數字，或使用科學記號表示的數字。
- **範例**:
  ```python
  pi = 3.14159
  price = 99.99
  scientific_notation = 1.23e4  # 等於 1.23 * 10^4 = 12300.0
  print(type(pi))  # <class 'float'>
  ```

#### 1-2-1-3. 字串 (str)

- **描述**: 用於表示文字資料。字串是字元的序列。可以使用單引號 (`'...'`)、雙引號 (`"..."`) 或三引號 (`'''...'''` 或 `"""..."""`) 來建立。三引號可以用於建立包含換行的多行字串。
- **範例**:
  ```python
  name = "Alice"
  greeting = 'Hello, World!'
  multi_line_text = """This is a
  multi-line string."""
  print(type(name))  # <class 'str'>
  ```

#### 1-2-1-4. 布林值 (bool)

- **描述**: 用於表示兩個邏輯值之一：`True` (真) 或 `False` (假)。布林值在條件判斷中至關重要。注意，首字母必須大寫。
- **範例**:
  ```python
  is_active = True
  is_logged_in = False
  print(type(is_active))  # <class 'bool'>
  ```

### 1-2-2. 集合型別 (Collection Types)

#### 1-2-2-1. 串列 (list)

- **描述**: 一個有序、可變的集合，允許包含重複的元素。串列是 Python 中最常用、最靈活的資料結構之一。
- **特性**:
  - **有序 (Ordered)**: 元素有固定的順序，不會改變。
  - **可變 (Mutable)**: 可以在建立後新增、刪除或修改元素。
  - **允許重複**: 可以包含相同值的元素。
- **語法**: 使用方括號 `[]` 建立，元素之間用逗號 `,` 分隔。
- **範例**:

  ```python
  fruits = ["apple", "banana", "cherry", "apple"]
  numbers = [1, 2, 3, 4, 5]

  # 存取元素 (索引從 0 開始)
  print(fruits[0])  # 'apple'

  # 修改元素
  fruits[1] = "blueberry"
  print(fruits)  # ['apple', 'blueberry', 'cherry', 'apple']

  # 新增元素
  fruits.append("orange")
  print(fruits)  # ['apple', 'blueberry', 'cherry', 'apple', 'orange']

  print(type(fruits))  # <class 'list'>
  ```

#### 1-2-2-2. 元組 (tuple)

- **描述**: 一個有序、不可變的集合，允許包含重複的元素。
- **特性**:
  - **有序 (Ordered)**: 元素有固定的順序。
  - **不可變 (Immutable)**: 建立後不能修改、新增或刪除元素。
  - **允許重複**: 可以包含相同值的元素。
- **語法**: 使用圓括號 `()` 建立，元素之間用逗號 `,` 分隔。
- **範例**:

  ```python
  coordinates = (10.0, 20.5)
  colors = ("red", "green", "blue")

  # 存取元素
  print(coordinates[0])  # 10.0

  # 嘗試修改元組會導致 TypeError
  # coordinates[0] = 5.0  # 這行會報錯

  print(type(coordinates))  # <class 'tuple'>
  ```

- **意義**: 當您有一組不希望被意外修改的資料時，使用元組比串列更安全。

#### 1-2-2-3. 集合 (set)

- **描述**: 一個無序、可變的集合，不允許包含重複的元素。
- **特性**:
  - **無序 (Unordered)**: 元素沒有固定的順序（在 Python 3.7+ 中，集合的內部實現可能使其看起來有序，但不應依賴此特性）。
  - **可變 (Mutable)**: 可以在建立後新增或刪除元素。
  - **不允許重複**: 如果加入重複的元素，集合不會有任何改變。
- **語法**: 使用大括號 `{}` 建立，或使用 `set()` 函式。注意：建立一個空的集合必須使用 `set()`，因為 `{}` 會建立一個空的字典。
- **範例**:

  ```python
  unique_numbers = {1, 2, 3, 4, 4, 5}
  print(unique_numbers)  # {1, 2, 3, 4, 5} (重複的 4 被自動移除)

  # 新增元素
  unique_numbers.add(6)
  print(unique_numbers)  # {1, 2, 3, 4, 5, 6}

  # 集合運算
  set_a = {1, 2, 3}
  set_b = {3, 4, 5}
  print(set_a.union(set_b))      # 聯集: {1, 2, 3, 4, 5}
  print(set_a.intersection(set_b)) # 交集: {3}

  print(type(unique_numbers))  # <class 'set'>
  ```

- **意義**: 集合非常適合用於去重或執行數學上的集合運算。

#### 1-2-2-4. 字典 (dictionary)

- **描述**: 一個無序（在 Python 3.7+ 中為有序）、可變的集合，由「鍵 (key)」和「值 (value)」成對組成。每個鍵都是唯一的。
- **特性**:
  - **鍵值對 (Key-Value Pairs)**: 每個元素都由一個鍵和一個對應的值組成。
  - **鍵唯一且不可變**: 字典中的鍵必須是唯一的，且通常使用不可變的型別（如字串、數字或元組）。
  - **可變 (Mutable)**: 可以在建立後新增、刪除或修改鍵值對。
- **語法**: 使用大括號 `{}` 建立，每個鍵值對格式為 `key: value`。
- **範例**:

  ```python
  person = {
      "name": "John Doe",
      "age": 30,
      "city": "New York"
  }

  # 存取值
  print(person["name"])  # 'John Doe'

  # 修改值
  person["age"] = 31
  print(person)  # {'name': 'John Doe', 'age': 31, 'city': 'New York'}

  # 新增鍵值對
  person["email"] = "john.doe@example.com"
  print(person)

  print(type(person))  # <class 'dict'>
  ```

#### 1-2-2-5. 串列中的串列 (List of Lists)

「串列中的串列」是指一個串列的元素本身也是串列。這種結構非常適合用來表示二維資料，例如數學中的矩陣、表格資料或遊戲地圖。

- **範例**:

  ```python
  # 建立一個 3x3 的矩陣
  matrix = [
      [1, 2, 3],
      [4, 5, 6],
      [7, 8, 9]
  ]
  print(matrix)
  # 輸出: [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

  # 建立一個包含不同長度子串列的串列 (不規則矩陣)
  irregular_list = [
      [10, 20],
      [30, 40, 50],
      [60]
  ]
  print(irregular_list)
  # 輸出: [[10, 20], [30, 40, 50], [60]]

  # 存取第一行 (索引 0) 的第二個元素 (索引 1)
  element = matrix[0][1]
  print(f"matrix[0][1]: {element}") # 輸出: 2

  # 存取第二行 (索引 1) 的第三個元素 (索引 2)
  element = matrix[1][2]
  print(f"matrix[1][2]: {element}") # 輸出: 6
  ```

---

## 1-3. 變數 (Variables)

在程式設計中，變數可以被想像成一個帶有名字的容器，用於儲存資料。透過為資料賦予一個名字，在程式中方便地引用和操作它。

### 1-3-1. 變數的宣告與賦值

在 Python 中，不需要像在某些靜態語言中那样預先「宣告」變數的型別。變數在第一次為它賦值時被建立。賦值操作使用等號 (`=`)。

```python
# 宣告一個名為 message 的變數，並將字串 "Hello, Python!" 存入其中
message = "Hello, Python!"

# 宣告一個名為 quantity 的變數，並將整數 10 存入其中
quantity = 10

# 宣告一個名為 pi 的變數，並將浮點數 3.14159 存入其中
pi = 3.14159

# 可以印出變數中儲存的值
print(message)    # Hello, Python!
print(quantity)   # 10
```

Python 支援「多重賦值」，可以一次為多個變數賦值：

```python
x, y, z = 10, 20, 30
print(x)  # 10
print(y)  # 20
```

### 1-3-2. 動態型別 (Dynamic Typing)

Python 是一種動態型別語言，這意味著同一個變數可以被重新賦值，並儲存不同型別的資料。

```python
my_variable = 100       # 此時 my_variable 是 int 型別
print(type(my_variable)) # <class 'int'>

my_variable = "Now I'm a string" # 現在 my_variable 被重新賦值為 str 型別
print(type(my_variable)) # <class 'str'>
```

- **意義**: 動態型別提供了很大的靈活性，但也需要開發者更加小心，確保變數在特定操作時處於預期的型別狀態，否則可能引發 `TypeError`。

### 1-3-3. 變數命名規則與習慣

選擇有意義的變數名稱是撰寫清晰易讀程式碼的關鍵。Python 對變數名稱有一些硬性規定和廣泛遵循的風格習慣。

#### 1-3-3-1. 硬性規則

1.  變數名稱只能包含字母 (a-z, A-Z)、數字 (0-9) 和底線 (`_`)。
2.  變數名稱不能以數字開頭。
3.  變數名稱不能是 Python 的關鍵字（保留字）。例如 `if`, `for`, `while`, `class` 等都不能作為變數名稱。

    ```python
    # 合法的變數名稱
    user_name
    age2
    _internal_variable
    MAX_CONNECTIONS

    # 不合法的變數名稱
    2age          # 不能以數字開頭
    user-name     # 不能包含連字號
    class         # 是 Python 關鍵字
    ```

#### 1-3-3-2. 命名習慣 (PEP 8 風格指南)

1.  **蛇式命名法 (snake_case)**:
    - **用途**: 用於變數和函式名稱。
    - **格式**: 所有字母小寫，單字之間用底線 `_` 分隔。
    - **範例**: `user_name`, `first_name`, `calculate_total_price`。

2.  **駝峰式命名法 (CamelCase)**:
    - **PascalCase (大駝峰)**:
      - **用途**: 用於類別 (Class) 名稱。
      - **格式**: 每個單字的首字母都大寫。
      - **範例**: `UserAccount`, `DatabaseConnection`。
    - **camelCase (小駝峰)**: 在 Python 中較少使用，通常在與其他語言（如 JavaScript）風格的程式碼庫整合時才會看到。

3.  **全大寫**:
    - **用途**: 用於常數 (Constants)，即在程式執行期間其值不應被改變的變數。
    - **格式**: 所有字母大寫，單字之間用底線 `_` 分隔。
    - **範例**: `MAX_OVERFLOW`, `PI`, `DEFAULT_TIMEOUT`。
    - **注意**: 這只是一種約定，Python 並不會強制阻止您修改一個全大寫的變數。

#### 1-3-3-3. 命名建議

- **使用有意義的名稱**: `user_name` 比 `un` 好，`price_per_item` 比 `ppi` 好。
- **避免單字母名稱**: 除非是用於簡單的迴圈計數器（如 `i`, `j`, `k`）或數學表達式（如 `x`, `y`），否則應避免使用 `l`（易與 `1` 混淆）、`O`（易與 `0` 混淆）等單字母名稱。

---

## 1-4. 運算子 (Operators)

運算子是特殊符號，用於執行資料之間的運算，例如算術計算、比較或邏輯判斷。

### 1-4-1. 算術運算子 (Arithmetic Operators)

用於執行基本的數學運算。

| Operators | 名稱          | 範例      | 結果                   |
| :-------- | :------------ | :-------- | :--------------------- |
| `+`       | 加法          | `10 + 5`  | `15`                   |
| `-`       | 減法          | `10 - 5`  | `5`                    |
| `*`       | 乘法          | `10 * 5`  | `50`                   |
| `/`       | 除法          | `10 / 4`  | `2.5` (結果總是浮點數) |
| `//`      | 整數除法      | `10 // 4` | `2` (結果向下取整)     |
| `%`       | 模數 (取餘數) | `10 % 3`  | `1`                    |
| `**`      | 指數 (次方)   | `2 ** 3`  | `8`                    |

```python
a = 15
b = 4

print(f"a + b = {a + b}")
print(f"a / b = {a / b}")
print(f"a // b = {a // b}")
print(f"a % b = {a % b}")
print(f"2 ** 4 = {2 ** 4}")
```

### 1-4-2. 比較運算子 (Comparison Operators)

用於比較兩個值，其運算結果總是布林值 (`True` 或 `False`)。

| Operators | 名稱     | 範例     | 結果    |
| :-------- | :------- | :------- | :------ |
| `==`      | 等於     | `a == b` | `False` |
| `!=`      | 不等於   | `a != b` | `True`  |
| `>`       | 大於     | `a > b`  | `True`  |
| `<`       | 小於     | `a < b`  | `False` |
| `>=`      | 大於等於 | `a >= b` | `True`  |
| `<=`      | 小於等於 | `a <= b` | `False` |

```python
x = 10
y = 20

print(f"x == 10: {x == 10}")  # True
print(f"x != y: {x != y}")    # True
print(f"y < x: {y < x}")      # False
```

### 1-4-3. 邏輯運算子 (Logical Operators)

用於組合多個布林表達式。

| Operators | 描述                                   | 範例                   | 結果   |
| :-------- | :------------------------------------- | :--------------------- | :----- |
| `and`     | 如果兩個運算元都為 `True`，則為 `True` | `(a > 5) and (b < 10)` | `True` |
| `or`      | 如果任一運算元為 `True`，則為 `True`   | `(a > 10) or (b < 10)` | `True` |
| `not`     | 反轉運算元的布林值                     | `not (a > 10)`         | `True` |

```python
age = 25
has_ticket = True

# 判斷是否可以進入：年齡大於 18 且持有票券
can_enter = (age > 18) and (has_ticket == True)
print(f"Can enter: {can_enter}")  # True

is_weekend = False
has_discount_coupon = True

# 判斷是否有折扣：是週末或持有折價券
has_discount = is_weekend or has_discount_coupon
print(f"Has discount: {has_discount}") # True
```

### 1-4-4. 其他運算子

#### 1-4-4-1. 賦值運算子 (Assignment Operators)

用於為變數賦值。除了基本的 `=`，還有結合算術運算子的簡寫形式。

| 運算子 | 等價於      | 範例 (假設 x=10)      |
| :----- | :---------- | :-------------------- |
| `+=`   | `x = x + 5` | `x += 5` (x 變為 15)  |
| `-=`   | `x = x - 5` | `x -= 5` (x 變為 5)   |
| `*=`   | `x = x * 5` | `x *= 5` (x 變為 50)  |
| `/=`   | `x = x / 5` | `x /= 5` (x 變為 2.0) |

#### 1-4-4-2. 成員運算子 (Membership Operators)

用於測試一個序列（如串列、元組、字串）中是否包含某個值。

| 運算子   | 描述                              | 範例                 | 結果   |
| :------- | :-------------------------------- | :------------------- | :----- |
| `in`     | 如果在序列中找到值，返回 `True`   | `3 in [1, 2, 3, 4]`  | `True` |
| `not in` | 如果在序列中找不到值，返回 `True` | `'a' not in 'hello'` | `True` |

```python
fruits = ["apple", "banana", "cherry"]
print(f"'banana' in fruits: {'banana' in fruits}")  # True
print(f"'orange' not in fruits: {'orange' not in fruits}") # True
```

#### 1-4-4-3. 身分運算子 (Identity Operators)

用於比較兩個物件是否是同一個物件（即它們在記憶體中的位置是否相同）。

| 運算子   | 描述                                    | 範例         |
| :------- | :-------------------------------------- | :----------- |
| `is`     | 如果兩個變數引用同一個物件，返回 `True` | `a is b`     |
| `is not` | 如果兩個變數引用不同物件，返回 `True`   | `a is not b` |

- **`is` vs `==`**:
  - `==` 比較的是兩個物件的「值」是否相等。
  - `is` 比較的是兩個變數是否指向記憶體中的「同一個物件」。

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(f"a == b: {a == b}")  # True (值相等)
print(f"a is b: {a is b}")  # False (是兩個不同的物件)
print(f"a is c: {a is c}")  # True (c 和 a 指向同一個物件)
```

---

## 1-5. 流程控制 (Flow Control)

流程控制結構允許根據特定條件來決定程式碼的執行路徑，或重複執行某段程式碼。

### 1-5-1. 條件判斷 (if/elif/else)

條件判斷用於根據一個或多個條件的真假來執行不同的程式碼區塊。

#### 1-5-1-1. `if` 陳述句

- **描述**: 如果條件為 `True`，則執行 `if` 下方的程式碼區塊。

```python
score = 95
if score > 90:
    print("Excellent!")
```

#### 1-5-1-2. `if-else` 陳述句

- **描述**: 如果 `if` 的條件為 `True`，執行 `if` 區塊；否則，執行 `else` 區塊。

```python
age = 16
if age >= 18:
    print("an adult.")
else:
    print("a minor.")
```

#### 1-5-1-3. `if-elif-else` 陳述句

- **描述**: 用於檢查多個互斥的條件。Python 會從上到下依序檢查 `if` 和 `elif` (else if) 的條件。一旦找到第一個為 `True` 的條件，就會執行其對應的程式碼區塊，並跳過其餘的 `elif` 和 `else`。如果所有條件都為 `False`，則執行 `else` 區塊（如果有的話）。

```python
score = 75

if score >= 90:
    print("Grade: A")
elif score >= 80:
    print("Grade: B")
elif score >= 70:
    print("Grade: C")
elif score >= 60:
    print("Grade: D")
else:
    print("Grade: F")

# 輸出: Grade: C
```

### 1-5-2. 迴圈 (Loops)

迴圈用於重複執行一段程式碼。

#### 1-5-2-1. `for` 迴圈

- **描述**: 用於迭代 (iterate) 一個序列（如串列、元組、字典、集合或字串）中的每个元素。
- **範例 1: 迭代串列**
  ```python
  fruits = ["apple", "banana", "cherry"]
  for fruit in fruits:
      print(fruit)
  ```
- **範例 2: 使用 `range()` 函式**
  `range()` 函式可以生成一個數字序列，非常適合用於需要執行固定次數的迴圈。

  ```python
  # 印出 0 到 4
  for i in range(5):
      print(i)

  # 印出 2 到 5
  for i in range(2, 6):
      print(i)
  ```

#### 1-5-2-2. `while` 迴圈

- **描述**: 只要指定的條件保持為 `True`，就會持續執行其下方的程式碼區塊。
- **注意**: 必須確保迴圈內部有改變條件的程式碼，否則可能導致「無窮迴圈 (infinite loop)」。

```python
count = 0
while count < 5:
    print(f"Count is: {count}")
    count += 1  # 改變條件，使迴圈最終能停止

print("Loop finished.")
```

#### 1-5-2-3. 迴圈控制陳述句

- **`break`**: 立即終止並跳出目前所在的迴圈。

  ```python
  # 找到第一個能被 7 整除的數字就停止
  for number in range(1, 20):
      if number % 7 == 0:
          print(f"Found the number: {number}")
          break
  ```

- **`continue`**: 跳過目前這一輪迴圈的剩餘程式碼，直接進入下一輪迭代。

  ```python
  # 印出 1 到 10 之間的所有奇數
  for number in range(1, 11):
      if number % 2 == 0:
          continue  # 如果是偶數，就跳過 print
      print(number)
  ```

---

## 1-6. 總結

為 Python 初學者介紹最核心的基礎語法。將從 Python 程式的基本結構與註解開始，接著深入探討各種基礎資料型別，包括數值、文字、布林值以及集合型別如串列 (list) 和字典 (dictionary)。此外，還會涵盖變數的命名規則、各類運算子的使用，以及控制程式流程的條件判斷與迴圈。

---

## 1-7. 練習

### 1-7-1. 程式基本結構與註解

1.  **修正縮排錯誤**:
    - 複製以下有錯誤的程式碼，貼到您的編輯器中。
    - 找出並修正其中的 `IndentationError`，讓程式可以成功執行。
      ```python
      name = "Bob"
      age = 25
      if age > 18:
      print(name + " is an adult.") # 錯誤的縮排
          print("He can vote.") # 錯誤的縮排
      else:
          print(name + " is not an adult.") # 錯誤的縮排
      ```
2.  **撰寫註解**:
    - 為以下程式碼添加註解，解釋每一行程式碼的功能。
      ```python
      # (在此處添加註解)
      principal = 1000
      # (在此處添加註解)
      rate = 0.05
      # (在此處添加註解)
      years = 3
      # (在此處添加註解)
      final_amount = principal * (1 + rate) ** years
      # (在此處添加註解)
      print(final_amount)
      ```
3.  **使用 Docstring**:
    - 撰寫一個名為 `subtract` 的函式，它接收兩個參數 `x` 和 `y`，並返回 `x - y` 的結果。
    - 在函式定義的第一行下方，為其添加一個 `docstring`，說明該函式的功能、參數和返回值。

### 1-7-2. 資料型別

1.  **型別檢查**:
    - 建立以下變數，並使用 `type()` 函式印出它們的資料型別：
      - 一個值為 `100` 的變數。
      - 一個值為 `-3.14` 的變數。
      - 一個值為 `"Python is fun"` 的變數。
      - 一個值為 `False` 的變數。
2.  **串列操作**:
    - 建立一個包含 `["cat", "dog", "bird"]` 的串列。
    - 在串列的尾部新增 `"fish"`。
    - 將串列中的 `"dog"` 修改為 `"hamster"`。
    - 印出最終的串列。
3.  **字典操作**:
    - 建立一個字典，描述一本書的資訊，包含 `"title"`, `"author"`, `"year"` 三個鍵。
    - 印出這本書的作者。
    - 為這本字典新增一個鍵 `"genre"`，值為 `"Science Fiction"`。
    - 印出最終的字典。

### 1-7-3. 變數

1.  **宣告與賦值**:
    - 宣告三個變數，分別儲存您的姓名 (字串)、年齡 (整數) 和身高 (浮點數，以公尺為單位)。
    - 將這三個變數的值印出。
2.  **命名風格**:
    - 判斷以下變數名稱哪些是合法的？哪些是符合 PEP 8 風格指南的？
      - `my variable`
      - `MyVariable`
      - `_my_variable`
      - `9th_variable`
      - `MAX_SIZE`
      - `is-active`
3.  **動態型別體驗**:
    - 宣告一個變數 `temp` 並賦值為 `36.5`。
    - 印出 `temp` 的值和它的型別。
    - 將 `temp` 重新賦值為字串 `"Normal"`。
    - 再次印出 `temp` 的值和它的新 型別。

### 1-7-4. 運算子

1.  **算術運算**:
    - 計算 `(25 * 4 + 150 / 2) // 7` 的結果是多少？
    - 計算 `2` 的 `10` 次方。
2.  **比較與邏輯**:
    - 宣告一個變數 `score = 88`。
    - 撰寫一個布林表達式，判斷 `score` 是否在 80 到 90 之間（包含 80 和 90）。
    - 撰寫一個布林表達式，判斷 `score` 是否小於 60 或大於 95。
3.  **成員運算**:
    - 給定一個串列 `allowed_users = ["Alice", "Bob", "Charlie"]`。
    - 宣告一個變數 `current_user = "David"`。
    - 撰寫程式碼檢查 `current_user` 是否在 `allowed_users` 串列中，並印出結果。
4.  **`is` vs `==`**:
    - 解釋以下程式碼的輸出結果為什麼是 `True` 和 `False`。

      ```python
      x = "hello"
      y = "hello"
      z = "".join(['h', 'e', 'l', 'l', 'o'])

      print(x == y)
      print(x == z)
      print(x is y) # 通常為 True (Python 對短字串有優化)
      print(x is z) # 通常為 False
      ```

### 1-7-5. 流程控制

1.  **成績判斷**:
    - 撰寫一個程式，讓使用者輸入一個分數 (0-100)。
    - 使用 `if/elif/else` 結構判斷並印出對應的評語：
      - `>= 90`: "優等"
      - `>= 80`: "甲等"
      - `>= 70`: "乙等"
      - `>= 60`: "丙等"
      - `< 60`: "不及格"
2.  **`for` 迴圈與 `range()`**:
    - 使用 `for` 迴圈和 `range()` 函式，印出 10 到 1 之間的所有偶數（即 10, 8, 6, 4, 2）。
3.  **`while` 迴圈猜數字**:
    - 設定一個秘密數字，例如 `secret_number = 7`。
    - 使用 `while` 迴圈和 `input()` 函式，讓使用者不斷猜數字，直到猜對為止。
    - 如果猜錯了，給予提示（例如 "太大了" 或 "太小了"）。
    - 如果猜對了，印出 "恭喜猜對！" 並結束迴圈。
4.  **`break` 與 `continue`**:
    - 給定一個串列 `data = [12, 45, "N/A", 78, "N/A", 99, 100]`。
    - 使用 `for` 迴圈遍歷此串列，計算所有數字的總和。
    - 如果遇到非數字的元素（如 `"N/A"`），使用 `continue` 跳過。
    - 如果遇到 `100`，使用 `break` 提前結束迴圈。
    - 印出最終的總和。

### 1-7-6. Leecode

1.  **[1. Two Sum (兩數之和)](https://leetcode.com/problems/two-sum/)**
    - **難度**: Easy
    - **說明**: 給定一個整數陣列 `nums` 和一個目標值 `target`，請你在該陣列中找出和為目標值的那兩個整數，並返回它們的陣列索引。
    - **為何適合**: 這道題目非常經典，可以練習使用 `for` 迴圈進行遍歷，以及使用字典 (dictionary) 來優化查找效率，是檢驗迴圈和集合型別掌握程度的絕佳題目。

2.  **[9. Palindrome Number (回文數)](https://leetcode.com/problems/palindrome-number/)**
    - **難度**: Easy
    - **說明**: 判斷一個整數是否是回文數。回文數是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
    - **為何適合**: 這道題目可以練習使用 `while` 迴圈和算術運算子（特別是 `%` 和 `//`）來反轉一個數字，從而判斷其是否為回文數，是練習基本運算和迴圈邏輯的好題目。

3.  **[412. Fizz Buzz](https://leetcode.com/problems/fizz-buzz/)**
    - **難度**: Easy
    - **說明**: 寫一個程式，輸出從 1 到 n 的數字的字串表示。但如果數字是 3 的倍數，輸出 "Fizz"；是 5 的倍數，輸出 "Buzz"；同時是 3 和 5 的倍數，輸出 "FizzBuzz"。
    - **為何適合**: 這是流程控制的經典入門題，完美結合了 `for` 迴圈、`if/elif/else` 條件判斷以及模數運算子 (`%`)。

4.  **[20. Valid Parentheses (有效的括號)](https://leetcode.com/problems/valid-parentheses/)**
    - **難度**: Easy
    - **說明**: 給定一個只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字串，判斷字串是否有效。
    - **為何適合**: 這道題目可以練習使用串列 (list) 作為堆疊 (stack) 資料結構，並結合 `for` 迴圈和 `if/else` 判斷來解決問題。它有助於理解串列的 `append` 和 `pop` 操作，是資料結構應用的好起點。

### 1-7-7. 基本練習

1. **繪製幾何圖形**

1-1. 直角三角形: 在終端機利用 \* 字元印出 `直角三角形`
1-2. 正三角形: 在終端機利用 \* 字元印出 `正三角形`
1-3. 菱形: 在終端機利用 \* 字元印出 `菱形`
1-4. 圓形: 在終端機利用 \* 字元印出 `圓形`
1-5. 9x9乘法表: 輸出 1x1=1 到 9x9=81 的格式化清單。

2. **二維陣列 / 矩陣處理**

2-1. 矩陣加法 / matrix addition: 兩個相同維度的矩陣，對應位置的數值相加。
2-2. 矩陣轉置 / matrix multiplication: 將矩陣的行與列對調（Row 變 Column）。
2-3. 矩陣乘法 / matrix transpose: 根據線性代數規則計算兩矩陣相乘。

3. **進階演算法**

3-1. 8 皇后問題: 8x8 棋盤上放 8 個皇后，使其互不攻擊（橫、豎、斜線都不能重疊）。
