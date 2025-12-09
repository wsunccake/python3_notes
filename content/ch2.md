# ch2. 進階語法

本章節將深入探討 Python 的進階主題，包括變數的深層應用、函式的彈性設計、物件導向程式設計、模組化管理、檔案處理以及如何建立穩健的錯誤處理機制。

---

## 2-1. 變數進階用法 (Variable Advanced)

在掌握基礎資料型別後，將詳細介紹純量與容器的區別，並深入探討字串、數字的進階用法以及如何運用巢狀資料結構來處理複雜的資料。

### 2-1-1. 純量 (Scalar) 與容器 (Container)

- **純量 (Scalar)**: 指的是只包含單一數值的資料類型，是資料的基本組成單位。

  - **常見類型**: `int`, `float`, `bool`, `None`
  - **範例**:
    ```python
    # 純量範例
    user_id = 101              # int
    item_price = 499.5         # float
    is_available = True        # bool
    last_login = None          # NoneType
    ```

- **容器 (Container)**: 指的是可以容納多個值的資料結構，用於組織和儲存資料集合。
  - **常見類型**: `str`, `list`, `tuple`, `dict`, `set`
  - **範例**:
    ```python
    # 容器範例
    product_name = "Laptop"   # str (可視為字元的容器)
    order_ids = [10, 15, 22]  # list
    rgb_color = (255, 128, 0) # tuple
    product_spec = {"cpu": "i7", "ram": "16GB"} # dict
    tags = {"tech", "sale", "laptop"}          # set
    ```

### 2-1-2. 字串 (String) 進階用法

#### 2-1-2-1. `replace()` - 替換字串

- **描述**: 回傳一個新字串，其中所有指定的子字串都被替換為另一個子字串。
- **語法**: `original_string.replace(old, new, count)`
  - `old`: 要被替換的子字串。
  - `new`: 用來替換的新子字串。
  - `count` (可選): 指定最多替換幾次。
- **範例**:

  ```python
  sentence = "I like cats. Cats are cute."

  # 替換所有 "Cats"
  new_sentence_1 = sentence.replace("Cats", "Dogs")
  print(new_sentence_1)  # 輸出: I like cats. Dogs are cute.

  # 只替換第一個 "cats" (注意大小寫)
  new_sentence_2 = sentence.replace("cats", "dogs", 1)
  print(new_sentence_2)  # 輸出: I like dogs. Cats are cute.
  ```

#### 2-1-2-2. `slice` - 字串切片

- **描述**: 透過 `[start:stop:step]` 索引語法，擷取字串的一部分，產生一個新的子字串。
- **範例**:

  ```python
  url = "https://www.example.com"

  # 擷取協定
  protocol = url[0:5]
  print(f"Protocol: {protocol}") # 輸出: https

  # 擷取域名
  domain = url[8:21]
  print(f"Domain: {domain}")     # 輸出: www.example.com

  # 從後面開始擷取
  top_level_domain = url[-3:]
  print(f"TLD: {top_level_domain}") # 輸出: com

  # 使用 step 反轉字串
  reversed_url = url[::-1]
  print(f"Reversed: {reversed_url}") # 輸出: moc.elpmaxe.www//:sptth
  ```

#### 2-1-2-3. 字串格式化

- **`format()` 方法**

  - **描述**: 功能強大的格式化方法，透過 `{}` 佔位符和 `.format()` 函式來組合字串。
  - **範例**:
    ```python
    template = "Hello, {name}. You are {age} years old."
    formatted_string = template.format(name="Bob", age=30)
    print(formatted_string) # 輸出: Hello, Bob. You are 30 years old.
    ```

- **f-string (格式化字串字面值)**

  - **描述**: 在字串前加上 `f`，並在 `{}` 中直接放入變數或表達式。可讀性高，效能好，是目前最推薦的方式。
  - **範例**:

    ```python
    user = "Alice"
    item_count = 3
    total_price = 250.5

    print(f"User '{user}' bought {item_count} items for a total of ${total_price:.2f}.")
    # 輸出: User 'Alice' bought 3 items for a total of $250.50.
    ```

- **`Template` string**

  - **描述**: 來自 `string` 模組，使用 `$variable` 形式的佔位符。主要優點是更安全，當格式化範本來自使用者輸入時，可以避免執行惡意程式碼。
  - **範例**:

    ```python
    from string import Template

    t = Template('Notification for $user: Your ticket is $ticket_id.')
    message = t.substitute(user='Charlie', ticket_id='T-12345')
    print(message) # 輸出: Notification for Charlie: Your ticket is T-12345.
    ```

### 2-1-3. 數字 (Number) 進階用法與巢狀資料結構

#### 2-1-3-1. 數字中使用底線

- **描述**: 為了提高長數字的可讀性，可以在數字中使用底線 `_` 作為千位分隔符。Python 直譯器會自動忽略這些底線。
- **範例**:

  ```python
  population = 7_888_000_000
  budget = 1_250_500.75

  print(population) # 輸出: 7888000000
  print(budget)     # 輸出: 1250500.75
  ```

#### 2-1-3-2. 複數 (complex)

- **描述**: Python 內建支援複數類型，以 `j` 或 `J` 作為虛數單位。
- **範例**:

  ```python
  c1 = 3 + 5j
  c2 = complex(2, -4) # 使用 complex() 函式建立

  print(f"c1: {c1}")             # 輸出: (3+5j)
  print(f"c2: {c2}")             # 輸出: (2-4j)
  print(f"c1.real: {c1.real}")   # 輸出: 3.0 (實部)
  print(f"c1.imag: {c1.imag}")   # 輸出: 5.0 (虛部)
  print(f"c1 + c2: {c1 + c2}")   # 輸出: (5+1j)
  ```

#### 2-1-3-3. 巢狀資料結構 (Nested Data Structures)

- **list of list (串列的串列)**: 常用於表示二維網格或矩陣。

  ```python
  # 範例：一個井字遊戲的棋盤
  board = [
      ['X', 'O', 'X'],
      ['O', 'X', 'O'],
      ['O', 'O', 'X']
  ]
  # 存取中間的元素
  center_element = board[1][1]
  print(center_element) # 輸出: X
  ```

- **list of dict (串列的字典)**: 常用於表示一組結構化的記錄，例如從 API 或資料庫查詢到的多筆資料。

  ```python
  # 範例：多個使用者的資料
  users = [
      {"id": 1, "name": "Alice", "email": "alice@example.com"},
      {"id": 2, "name": "Bob", "email": "bob@example.com"}
  ]
  # 取得第一個使用者的 email
  first_user_email = users[0]["email"]
  print(first_user_email) # 輸出: alice@example.com
  ```

- **dict of list (字典的串列)**: 常用於將相關的項目分組。

  ```python
  # 範例：儲存不同類別的電影
  movies_by_genre = {
      "Sci-Fi": ["Inception", "The Matrix", "Blade Runner"],
      "Comedy": ["The Hangover", "Superbad"]
  }
  # 取得所有科幻電影
  sci_fi_movies = movies_by_genre["Sci-Fi"]
  print(sci_fi_movies) # 輸出: ['Inception', 'The Matrix', 'Blade Runner']
  ```

- **dict of dict (字典的字典)**: 常用於表示複雜的、具有階層關係的資料。
  ```python
  # 範例：儲存員工的詳細資料
  employees = {
      "E101": {"name": "Charlie", "position": "Developer", "salary": 80000},
      "E102": {"name": "David", "position": "Designer", "salary": 75000}
  }
  # 取得 E101 員工的職位
  charlie_position = employees["E101"]["position"]
  print(charlie_position) # 輸出: Developer
  ```

---

## 2-2. 函式 (Function)

函式是組織好、可重複使用的程式碼區塊，用於執行單一且相關的動作。

### 2-2-1. 定義與呼叫函式

- **描述**: 使用 `def` 關鍵字定義函式，並透過函式名稱加上括號 `()` 來呼叫。
- **範例**:

  ```python
  def calculate_area(width, height):
      """計算矩形的面積"""
      area = width * height
      return area

  # 呼叫函式並將回傳值存入變數
  rect_area = calculate_area(10, 5)
  print(f"The area is: {rect_area}") # 輸出: The area is: 50
  ```

### 2-2-2. 參數傳遞 (Parameter Passing)

- **位置參數 (Positional Arguments)**: 呼叫時必須按照函式定義的順序傳遞。
- **關鍵字參數 (Keyword Arguments)**: 呼叫時透過 `參數名=值` 的方式傳遞，可以忽略順序。
- **預設參數 (Default Arguments)**: 在定義函式時為參數指定預設值。如果呼叫時未提供該參數，則會使用預設值。
- **可變參數 (`*args` and `**kwargs`)\*\*:

  - `*args`: 將傳入的多個位置參數組合成一個元組 (tuple)。
  - `**kwargs`: 將傳入的多個關鍵字參數組合成一個字典 (dict)。

- **範例**:

  ```python
  def create_user(username, age, city="New York", *hobbies, **social_media):
      print(f"Username: {username}, Age: {age}, City: {city}")

      if hobbies:
          print("Hobbies:")
          for hobby in hobbies:
              print(f"- {hobby}")

      if social_media:
          print("Social Media:")
          for platform, link in social_media.items():
              print(f"- {platform}: {link}")

  # 綜合呼叫
  create_user(
      "Eve",
      28,
      "London",                          # 位置參數 & 預設參數
      "Reading", "Hiking",               # *args
      twitter="@eve", github="eve-dev"   # **kwargs
  )
  ```

### 2-2-3. 回傳值 (Return Value)

- **描述**: 函式可以使用 `return` 語句將結果回傳給呼叫者。函式可以回傳任何型別的物件，也可以回傳多個值（實際上是回傳一個元組）。
- **範例**:

  ```python
  def get_stats(numbers):
      min_val = min(numbers)
      max_val = max(numbers)
      sum_val = sum(numbers)
      return min_val, max_val, sum_val # 回傳多個值

  stats = get_stats([10, 20, 5, 45, 80])
  print(stats) # 輸出: (5, 80, 160)

  min_v, max_v, sum_v = stats # 可以直接解包 (unpack)
  print(f"Min: {min_v}, Max: {max_v}, Sum: {sum_v}")
  ```

### 2-2-4. `lambda` 匿名函式

- **描述**: 小型的、匿名的、單行的函式。它只能包含一個表達式，該表達式的結果就是函式的回傳值。
- **語法**: `lambda arguments: expression`
- **意義**: `lambda` 函式非常適合用於需要一個簡單函式作為參數的場合，例如 `sorted()`, `map()`, `filter()` 等，可以讓程式碼更簡潔。
- **範例**:

  ```python
  # 原始函式
  def square(x):
      return x * x

  # 對應的 lambda 函式
  square_lambda = lambda x: x * x
  print(square_lambda(5)) # 輸出: 25

  # 在 sorted() 中使用 lambda 作為排序的 key
  points = [(1, 5), (3, 2), (5, 8)]
  # 根據每個元組的第二個元素進行排序
  sorted_points = sorted(points, key=lambda point: point[1])
  print(sorted_points) # 輸出: [(3, 2), (1, 5), (5, 8)]
  ```

### 2-2-5. `lambda` 函數與外部變數的互動 (閉包)

- **意義**: `lambda` 函數可存取其定義時所在環境中的變數。當外部函數回傳一個 `lambda` 函數時，即使外部函數已經執行完畢，`lambda` 函數仍然會「記住」外部函數作用域中的變數，這就是閉包的特性。

- **範例**:

  ```python
  def create_multiplier(factor):
      # create_multiplier 函數定義了一個變數 factor
      return lambda x: x * factor # lambda 函數捕捉了 factor 變數

  # 建立兩個不同的乘法器
  double = create_multiplier(2) # double 是一個 lambda 函數，其中 factor 為 2
  triple = create_multiplier(3) # triple 是一個 lambda 函數，其中 factor 為 3

  # 使用這些乘法器
  print(f"5 乘以 2 的結果: {double(5)}")
  print(f"5 乘以 3 的結果: {triple(5)}")
  ```

### 2-2-6. 變數作用域 (Scope)

- **描述**: 變數的可見範圍。Python 遵循 **LEGB** 規則來查找變數：
  1.  **L (Local)**: 區域作用域，指函式內部的範圍。
  2.  **E (Enclosing)**: 閉包作用域，指巢狀函式中，外部函式的範圍。
  3.  **G (Global)**: 全域作用域，指模組層級的範圍。
  4.  **B (Built-in)**: 內建作用域，指 Python 內建的函式和名稱 (如 `print`, `len`)。
- **範例**:

  ```python
  x = "I am global" # Global scope

  def outer_function():
      x = "I am enclosing" # Enclosing scope

      def inner_function():
          x = "I am local" # Local scope
          print(f"Inside inner_function: {x}") # 查找順序: L -> E -> G -> B

      inner_function()
      print(f"Inside outer_function: {x}")

  outer_function()
  print(f"Outside all functions: {x}")

  # 輸出:
  # Inside inner_function: I am local
  # Inside outer_function: I am enclosing
  # Outside all functions: I am global
  ```

---

## 2-3. 類別 (Class)

類別是物件導向程式設計 (Object-Oriented Programming, OOP) 的核心，它是一種建立物件的藍圖。

### 2-3-1. 類別 (Class) 與物件 (Object)

- **類別 (Class)**: 定義了一組物件共同的屬性 (attributes) 和方法 (methods)。
- **物件 (Object)**: 類別的實例 (instance)。
- **`__init__` 方法**: 是一個特殊的「建構子」方法，在建立物件時會自動被呼叫，通常用於初始化物件的屬性。
- **`self` 參數**: 在類別的方法中，`self` 代表物件本身，必須是方法的第一個參數。

- **範例**:

  ```python
  class Car:
      # 建構子
      def __init__(self, brand, model, year):
          self.brand = brand
          self.model = model
          self.year = year
          self.is_running = False

      # 方法
      def start_engine(self):
          self.is_running = True
          print(f"The {self.brand} {self.model}'s engine is now running.")

      def stop_engine(self):
          self.is_running = False
          print(f"The {self.brand} {self.model}'s engine has stopped.")

  # 建立 Car 物件 (實例化)
  my_car = Car("Toyota", "Corolla", 2022)

  # 存取屬性
  print(f"My car is a {my_car.year} {my_car.brand} {my_car.model}.")

  # 呼叫方法
  my_car.start_engine()
  print(f"Is the engine running? {my_car.is_running}")
  ```

### 2-3-2. 繼承 (Inheritance)

- **描述**: 一種允許我們定義一個繼承自另一個類別的類別的機制。子類別 (child class) 會繼承父類別 (parent class) 的所有屬性和方法，並可以新增自己的或覆寫 (override) 父類別的方法。
- **`super()` 函式**: 用於呼叫父類別的方法，特別是在子類別的 `__init__` 中呼叫父類別的建構子。

- **範例**:

  ```python
  # 父類別
  class Animal:
      def __init__(self, name):
          self.name = name

      def speak(self):
          return "Some generic animal sound"

  # 子類別，繼承自 Animal
  class Dog(Animal):
      def __init__(self, name, breed):
          super().__init__(name) # 呼叫父類別的 __init__
          self.breed = breed

      # 覆寫 (override) 父類別的 speak 方法
      def speak(self):
          return "Woof! Woof!"

  # 建立子類別物件
  my_dog = Dog("Buddy", "Golden Retriever")

  print(my_dog.name)        # 繼承自 Animal
  print(my_dog.breed)       # Dog 自己的屬性
  print(my_dog.speak())     # 使用覆寫後的方法
  ```

### 2-3-3. 多重繼承 (Multi-Inheritance)

多重繼承是指一個子類別可以同時繼承多個父類別的屬性與方法。這使得子類別能夠結合多個父類別的功能，實現更複雜的行為。

- **意義**: 在 Python 中，一個類別可以從多個基底類別（父類別）繼承。子類別將擁有所有父類別的公共屬性和方法。
- **優點**: 允許程式碼重用，並能將不同功能模組化後再組合。
- **挑戰**: 可能會導致方法解析順序（Method Resolution Order, MRO）的複雜性，即當多個父類別有同名方法時，Python 決定呼叫哪個方法的順序。Python 使用 C3 線性化演算法來處理 MRO。

- **範例**:

  ```python
  # 父類別 1: 飛行能力
  class Flyer:
      def fly(self):
          return "我會飛翔！"

  # 父類別 2: 游泳能力
  class Swimmer:
      def swim(self):
          return "我會游泳！"

  # 子類別: 鴨子，同時繼承 Flyer 和 Swimmer
  class Duck(Flyer, Swimmer):
      def quack(self):
          return "呱！呱！"

  # 建立鴨子物件
  my_duck = Duck()

  # 呼叫繼承自不同父類別的方法以及自己的方法
  print(f"鴨子說：{my_duck.fly()}")
  print(f"鴨子說：{my_duck.swim()}")
  print(f"鴨子說：{my_duck.quack()}")
  ```

### 2-3-4. 成員可見性與變數類型

在 Python 中，雖然沒有像 Java 或 C++ 那樣嚴格的 `public`, `protected`, `private` 關鍵字，但它提供了一套基於命名約定的機制來建議或實現成員的可見性。

#### 1. 類別變數 (Class Variable) vs. 實例變數 (Instance Variable)

- **實例變數 (Instance Variable)**:

  - **意義**: 每個類別實例（物件）獨有的變數。它們通常在 `__init__` 方法中透過 `self.variable_name` 的方式定義。
  - **特性**: 不同實例之間的實例變數是互相獨立的，修改一個實例的變數不會影響其他實例。

- **類別變數 (Class Variable)**:
  - **意義**: 在類別定義中，但在任何方法之外宣告的變數。它由該類別的所有實例共享。
  - **特性**: 所有實例都可以存取這個變數。如果透過類別名稱（例如 `ClassName.variable`）修改它，所有實例都會看到這個變更。如果透過實例（例如 `instance.variable`）修改它，則會為該實例創建一個同名的實例變數，遮蔽掉類別變數。

#### 2. 保護成員 (Protected Member) - `_single_underscore`

- **意義**: 以單一底線 `_` 開頭的成員（方法或變數），例如 `_protected_member`。
- **約定**: 這是一種命名約定，用來**提示**程式設計師，這個成員不應該在類別外部直接存取，它被視為內部實作的一部分。
- **強制性**: Python 直譯器**不會**強制禁止外部存取。仍可在類別外部存取它，但這被認為是不好的實踐。

#### 3. 私有成員 (Private Member) - `__double_underscore`

- **意義**: 以雙底線 `__` 開頭的成員（方法或變數），例如 `__private_member`。
- **機制**: 這會觸發 Python 的**名稱改寫 (Name Mangling)** 機制。Python 直譯器會將這個成員的名稱改寫為 `_ClassName__memberName` 的形式。
- **目的**: 這種機制旨在避免子類別意外地覆寫父類別的私有成員，而不是為了實現真正的私有性。雖然從外部直接存取會失敗，但只要知道改寫後的名稱，仍然可以存取它。

- **範例**:

  ```python
  class Dog:
      # 類別變數 (Class Variable)
      species = "Canis familiaris"

      def __init__(self, name, age):
          # 實例變數 (Instance Variables)
          self.name = name
          self.age = age
          # 保護成員 (Protected Member)
          self._secret_trick = "翻滾"
          # 私有成員 (Private Member)
          self.__private_thought = "我想吃零食"

      def display_info(self):
          print(f"名字: {self.name}, 年齡: {self.age}")
          print(f"物種: {self.species}") # 存取類別變數
          print(f"牠會的秘密招式: {self._secret_trick}") # 內部存取保護成員
          print(f"牠的內心想法: {self.__private_thought}") # 內部存取私有成員

      def _protected_method(self):
          print("這是一個保護方法，用來準備秘密招式。")

      def __private_method(self):
          print("這是一個私有方法，用來思考零食。")

  # 建立實例
  my_dog = Dog("旺財", 3)

  # --- 存取變數 ---
  print("--- 存取變數 ---")
  # 存取實例變數 (公開)
  print(f"公開存取名字: {my_dog.name}")
  # 存取類別變數
  print(f"公開存取物種: {Dog.species}")
  # 存取保護成員 (不建議，但可行)
  print(f"公開存取秘密招式: {my_dog._secret_trick}")

  # 嘗試存取私有成員 (會失敗)
  try:
      print(my_dog.__private_thought)
  except AttributeError as e:
      print(f"直接存取私有成員失敗: {e}")

  # 透過名稱改寫存取私有成員 (不建議，但可行)
  print(f"透過名稱改寫存取私有成員: {my_dog._Dog__private_thought}")

  # --- 呼叫方法 ---
  print("\n--- 呼叫方法 ---")
  my_dog.display_info()
  my_dog._protected_method() # 呼叫保護方法 (不建議)
  try:
      my_dog.__private_method()
  except AttributeError as e:
      print(f"直接呼叫私有方法失敗: {e}")

  my_dog._Dog__private_method() # 透過名稱改寫呼叫私有方法 (不建議)
  ```

---

## 2-4. 模組與套件 (Import)

模組化是將大型程式拆分成多個小型、易於管理的檔案的過程，有助於程式碼的重用和組織。

### 2-4-1. 模組 (Module)

- **描述**: 在 Python 中，一個 `.py` 檔案就是一個模組。我們可以使用 `import` 語句來使用其他模組中定義的函式、類別或變數。
- **範例**:
  假設我們有以下檔案 `my_math.py`:

  ```python
  # my_math.py
  PI = 3.14159

  def add(a, b):
      return a + b

  def subtract(a, b):
      return a - b
  ```

  現在，在另一個檔案 `main.py` 中使用它：

  ```python
  # main.py

  # 寫法 1: 引入整個模組
  import my_math
  print(my_math.PI)
  print(my_math.add(5, 3))

  # 寫法 2: 從模組中引入特定項目
  from my_math import PI, add
  print(PI)
  print(add(10, 5))

  # 寫法 3: 引入並使用別名 (alias)
  from my_math import subtract as sub
  print(sub(10, 4))

  # 寫法 4: 引入並使用別名 (alias)
  from my_math import * # 導入 my_math 模組中的所有公開名稱
  print(subtract(10, 4))
  ```

### 2-4-2. 套件 (Package)

- **描述**: 套件是一種組織模組的方式。它是一個包含多個模組的資料夾，該資料夾內通常會包含一個 `__init__.py` 檔案 (在 Python 3.3+ 中，這個檔案可以是空的，但它的存在告訴 Python 該目錄是一個套件)。
- **範例**:
  假設有以下檔案結構：

  ```
  my_app/
  ├── __init__.py
  ├── utils/
  │   ├── __init__.py
  │   └── string_helpers.py
  └── main.py
  ```

  `string_helpers.py` 內容:

  ```python
  def reverse(s):
      return s[::-1]
  ```

  `main.py` 內容:

  ```python
  from utils.string_helpers import reverse

  print(reverse("hello")) # 輸出: olleh
  ```

---

## 2-5. 檔案操作 (File I/O)

檔案操作 (Input/Output) 是指程式與外部檔案系統之間的資料讀寫。

### 2-5-1. 讀取與寫入檔案

- **描述**: 使用內建函式 `open()` 來開啟檔案，並搭配 `with` 陳述式來確保檔案在使用完畢後能被自動、安全地關閉，即使在過程中發生錯誤也一樣。
- **語法**: `with open(file_path, mode, encoding='utf-8') as file_variable:`
  - `mode`:
    - `'r'`: 讀取 (預設值)
    - `'w'`: 寫入 (如果檔案已存在，會覆寫內容)
    - `'a'`: 附加 (在檔案末尾追加內容)
    - `'b'`: 二進位模式 (如讀寫圖片)
    - `'+'`: 更新模式 (讀寫)
  - `encoding`: 建議永遠指定為 `'utf-8'` 來處理各種文字。

#### 2-5-1-1. 寫入檔案

- **範例**:

  ```python
  lines_to_write = ["First line.\n", "Second line.\n"]

  # 使用 'w' 模式覆寫或建立新檔案
  with open('my_document.txt', 'w', encoding='utf-8') as f:
      f.write("Hello, World!\n")
      f.writelines(lines_to_write)

  # 使用 'a' 模式附加內容
  with open('my_document.txt', 'a', encoding='utf-8') as f:
      f.write("This is an appended line.\n")
  ```

#### 2-5-1-2. 讀取檔案

- **範例**:

  ```python
  # 一次讀取整個檔案內容
  with open('my_document.txt', 'r', encoding='utf-8') as f:
      content = f.read()
      print("---" Full Content ---")
      print(content)

  # 逐行讀取，這是處理大檔案時更有效率的方式
  with open('my_document.txt', 'r', encoding='utf-8') as f:
      print("--- Line by Line ---")
      for line in f:
          print(line.strip()) # .strip() 去除行首尾的空白或換行符
  ```

---

## 2-6. 錯誤與例外處理 (Exception)

例外處理是一種機制，用於處理程式在執行期間可能發生的錯誤，讓程式不會因此崩潰，而是可以優雅地應對。

### 2-6-1. `try...except` 區塊

- **`try`**: 將可能引發錯誤的程式碼放在 `try` 區塊中。
- **`except`**: 如果 `try` 區塊中發生了特定類型的錯誤，對應的 `except` 區塊會被執行。
- **`else`**: 如果 `try` 區塊沒有發生任何錯誤，`else` 區塊會被執行。
- **`finally`**: 無論是否發生錯誤，`finally` 區塊中的程式碼最後一定會被執行 (常用於釋放資源，如關閉檔案)。

- **範例**:
  ```python
  try:
      numerator = int(input("Enter a numerator: "))
      denominator = int(input("Enter a denominator: "))
      result = numerator / denominator
  except ValueError:
      # 當 int() 轉換失敗時觸發
      print("Error: Please enter valid integers.")
  except ZeroDivisionError:
      # 當除數為零時觸發
      print("Error: Cannot divide by zero.")
  except Exception as e:
      # 捕捉所有其他未預期的錯誤
      print(f"An unexpected error occurred: {e}")
  else:
      # 如果沒有錯誤發生
      print(f"The result is {result}")
  finally:
      # 無論如何都會執行
      print("Execution finished.")
  ```

### 2-6-2. `raise` 在 `except` 區塊中的使用

在 `try...except` 結構中處理異常時，有時我們需要在記錄錯誤或執行某些清理操作後，將異常繼續向上拋出。`raise` 關鍵字在 `except` 區塊中有幾種不同的用法。

#### 1. `raise` (重新引發原始異常)

- **意義**: 在 `except` 區塊中，不帶任何參數地使用 `raise`。
- **行為**: 這會**重新引發**剛剛被捕獲的異常。最重要的是，它會保留原始異常的完整資訊，包括**類型**和**追蹤記錄 (traceback)**。
- **用途**: 當你想要記錄一個錯誤，但又不想「吞掉」這個異常，而是讓更高層級的程式碼來處理它時，這是最佳選擇。

#### 2. `raise NewException from original_exception` (異常鏈)

- **意義**: 捕獲一個異常後，引發一個新的、可能更具描述性的異常，同時將原始異常附加到新異常上。
- **行為**: 這會創建一個**異常鏈 (Exception Chaining)**。新的異常會被拋出，但原始異常會被保存在新異常的 `__cause__` 屬性中。
- **用途**: 當你想要將底層的技術性錯誤（如 `IOError`）轉換為更高層次的應用程式錯誤（如 `DataProcessingError`）時非常有用，同時又不丟失根本原因的資訊。

#### 3. `raise NewException` (引發新異常)

- **意義**: 在 `except` 區塊中，引發一個全新的異常，且不與原始異常關聯。
- **行為**: 這會拋出一個全新的異常。在 Python 3 中，原始異常的上下文會被記錄在 `__context__` 屬性中，但 `__cause__` 會是 `None`。這通常會讓除錯變得更困難，因為原始的追蹤記錄可能會被隱藏。
- **用途**: 較少使用。適用於你確定原始異常與新問題完全無關，或者想要完全隱藏底層錯誤細節的情況。

- **範例**:

  ```python
  def process_file(file_name):
      print(f"\n--- 正在處理檔案: {file_name} ---")
      try:
          # 模擬一個可能發生的底層錯誤
          with open(file_name, 'r') as f:
              # 假設檔案內容必須是數字
              content = f.read()
              value = int(content)
              print(f"檔案內容轉換為數字: {value}")
      except FileNotFoundError as e:
          print("捕獲到底層錯誤: 檔案不存在！")
          # 用法 2: 引發一個新的、更具描述性的應用程式錯誤，並鏈接原始錯誤
          raise RuntimeError(f"設定檔 '{file_name}' 遺失，無法繼續。") from e
      except ValueError as e:
          print("捕獲到底層錯誤: 檔案內容不是有效的數字！")
          # 用法 1: 記錄錯誤並重新引發原始錯誤
          print("日誌記錄: 檔案內容格式錯誤。現在重新引發原始錯誤。 ")
          raise

  # --- 主程式 ---
  files_to_process = ["config.txt", "invalid_data.txt", "non_existent.txt"]

  for file in files_to_process:
      try:
          # 準備測試檔案
          if file == "config.txt":
              with open(file, 'w') as f: f.write("123")
          if file == "invalid_data.txt":
              with open(file, 'w') as f: f.write("abc")

          process_file(file)

      except RuntimeError as e:
          print(f"高層級處理: 捕獲到 RuntimeError: {e}")
          if e.__cause__:
              print(f"根本原因: {e.__cause__}")
      except ValueError as e:
          print(f"高層級處理: 捕獲到 ValueError: {e}")
          print("這個錯誤是從 process_file 中重新引發的。")
      except Exception as e:
          print(f"高層級處理: 捕獲到未預期的錯誤: {e}")
  ```

---

## 2-7. 總結

介紹 Python 進階核心概念。從變數的深層結構，到函式的靈活運用，再到物件導向的設計思想，這些都是編寫大型、可維護應用程式的基礎。同時，模組化管理、檔案 I/O 和穩健的例外處理，確保了程式碼的組織性、實用性與穩定性。掌握這些主題，將使您能夠從撰寫簡單腳本，邁向開發複雜系統的層次。

---

## 2-8. 練習

### 2-8-1. 變數進階

1.  **巢狀資料結構**: 建立一個 `list of dict`，用來儲存多本書的資訊。每本書都是一個字典，包含 `title` (字串), `author` (字串), `pages` (整數) 三個鍵。然後，撰寫程式碼找出頁數最多那本書的書名。
2.  **字串格式化**: 宣告三個變數 `name`, `age`, `city`。使用 f-string 建立一個介紹自己的句子，例如 "My name is [name], I am [age] years old, and I live in [city]."。

### 2-8-2. 函式

1.  **`*args` 與 `**kwargs`**: 撰寫一個名為 `report`的函式，它接受一個必要的位置參數`topic`，任意數量的額外位置參數 `\*metrics`，以及任意數量的關鍵字參數 `\*\*details`。函式需要將所有接收到的資訊格式化並印出。
2.  **`lambda` 應用**: 給定一個名字串列 `names = ["David", "Amy", "Charlie", "Bob"]`，使用 `sorted()` 和 `lambda` 函式，根據每個名字的長度來對串列進行排序。

### 2-8-3. 類別

1.  **建立 `BankAccount` 類別**:
    - 屬性: `owner` (戶名), `balance` (餘額)。
    - `__init__` 方法: 初始化戶名和初始餘額。
    - `deposit(amount)` 方法: 存款，增加餘額。
    - `withdraw(amount)` 方法: 提款，減少餘額。需檢查餘額是否足夠。
    - 建立一個帳戶物件，並進行存款、提款操作來測試。
2.  **繼承**: 建立一個 `SavingsAccount` 子類別，繼承自 `BankAccount`。新增一個 `interest_rate` (利率) 屬性，並新增一個 `add_interest()` 方法來計算並增加利息。
3.  **創建一個 `Car` 類別**:
    - 包含一個類別變數 `number_of_wheels = 4`。
    - 包含公開的實例變數 `color` 和 `brand`。
    - 包含一個保護的實例變數 `_engine_serial_number`。
    - 包含一個私有的實例變數 `__fuel_level`，並提供一個公開的方法 `get_fuel_level()` 來讀取它。
    - 創建一個 `Car` 物件並嘗試從外部存取這些變數。

### 2-8-4. 模組與套件

1.  **建立模組**: 建立一個 `geometry.py` 檔案，在其中定義兩個函式：`circle_area(radius)` 和 `rectangle_area(width, height)`。
2.  **使用模組**: 建立另一個 `app.py` 檔案，從 `geometry` 模組中 `import` 這兩個函式，並呼叫它們來計算圓形和矩形的面積。

### 2-8-5. 檔案操作

1.  **寫入日誌**: 撰寫一個函式 `log_message(message)`，它會將帶有時間戳的訊息附加到 `app.log` 檔案中。例如，呼叫 `log_message("User logged in")` 後，檔案中會新增一行類似 `"2023-10-27 10:30:00 - User logged in"` 的內容。
2.  **讀取與計數**: 讀取您剛才建立的 `app.log` 檔案，並計算檔案中總共有幾行日誌。

### 2-8-6. 例外處理

1.  **安全的字典取值**: 撰寫一個函式 `get_value(data_dict, key)`，它會嘗試回傳字典中指定鍵的值。如果鍵不存在 (`KeyError`)，函式應回傳 `None` 而不是讓程式崩潰。
2.  **檔案讀取器**: 撰寫一個程式，提示使用者輸入檔名，然後讀取並印出檔案內容。使用 `try...except` 來處理檔案不存在 (`FileNotFoundError`) 的情況，並在這種情況下印出友善的錯誤訊息。
3.  **創建一個 `divide(a, b)` 函數**:
    - 在函數內部，使用 `try...except` 捕捉 `ZeroDivisionError`。
    - 在 `except` 區塊中，印出一條錯誤訊息，然後使用 `raise ValueError("除數不能為零") from e` 來引發一個新的異常。
    - 在函數外部呼叫 `divide(10, 0)`，並捕獲 `ValueError`，然後印出異常訊息及其 `__cause__`。
