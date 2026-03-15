# Meta Programming

## 簡介說明

`Meta Programming`（元程式設計）是一種程式設計技術，其核心思想是**編寫能夠讀取、分析、轉換或生成其他程式的程式**。簡單來說，就是「用程式碼來操作程式碼」。一般的程式在執行時處理的是「數據」，而元程式在執行時處理的則是「程式碼」本身，將程式碼視為一種可以被處理的數據。

這種技術允許開發者在編譯時期 (compile-time) 或執行時期 (run-time) 動態地修改程式的行為和結構，從而實現更高層次的抽象、減少重複的樣板程式碼 (boilerplate code)，並建立更靈活、更具擴展性的框架。

---

## 演進過程與歷史 (History)

- **1960年代 - LISP 的誕生**: `Meta Programming` 的思想根源可以追溯到早期的程式語言 LISP。LISP 的一個革命性特點是其「程式碼即數據」(Code as Data) 的哲學，這得益於其同像性 (homoiconicity)，即程式碼的結構與其數據結構（列表）是相同的。這使得 LISP 程式可以輕易地建立和操作其他的 LISP 程式，是元程式設計最早且最純粹的體現。

- **1970年代 - Smalltalk 與物件導向**: 隨著 Smalltalk 的出現，元程式設計在物件導向的世界中找到了新的形式。Smalltalk 引入了強大的**反射 (Reflection)** 機制，允許程式在執行時期檢查和修改自身的結構，例如查詢一個物件的類別、方法，甚至動態地修改類別定義。這為後來的動態語言（如 Python, Ruby）的元程式設計能力奠定了基礎。

- **1980-90年代 - C++ 樣板元程式設計 (Template Metaprogramming)**: C++ 透過其樣板 (template) 系統，開創了一種在「編譯時期」進行元程式設計的範式。開發者可以利用樣板來編寫在編譯過程中執行的程式碼，進行複雜的計算、類型檢查或程式碼生成。這種技術雖然語法複雜且難以除錯，但它可以在不增加執行時期開銷的情況下，大幅提升程式的效能和泛用性。

- **2000年代至今 - 動態語言的興盛**: Python, Ruby, JavaScript 等動態語言的普及，讓元程式設計變得更加親民和實用。這些語言通常都內建了強大的執行時期元程式設計能力：
  - **Python**: 透過裝飾器 (Decorators)、元類別 (Metaclasses) 和 `type()` 函式，提供了多層次的元程式設計工具。
  - **Ruby**: 以其極致的靈活性而聞名，允許「打開」任何類別並在執行時期修改它 (monkey patching)，其實作了許多基於元程式設計的框架，最知名的就是 Ruby on Rails。
  - **為何如此發展**: 根本驅動力是為了**提高開發效率和軟體的抽象能力**。開發者發現許多程式碼都遵循著某些重複的模式（例如 ORM 中的資料庫欄位對應、Web 框架中的路由註冊）。元程式設計允許開發者將這些模式抽象化，編寫出能「自動生成」這些樣板程式碼的程式，從而讓業務邏輯更清晰，框架更強大。

---

## 概念 (Conpcet)

1. **Code as Data (程式碼即數據)**

   這是元程式設計的核心哲學。將程式的原始碼、類別定義、函式等結構不視為靜態的指令，而是看作可以被存取、分析和操作的數據結構。

2. **Introspection (自省)**

   指程式在執行時期**檢查**自身結構和狀態的能力。例如，一個物件可以知道自己的類別是什麼、它有哪些屬性和方法。這是一種「唯讀」的操作。

3. **Reflection (反射)**

   是自省的更進一步，指程式在執行時期**修改**自身結構和狀態的能力。例如，不僅能發現一個物件有某個屬性，還能動態地為它新增屬性、呼叫方法或甚至建立全新的類別。這是一種「讀寫」操作。

4. **Compile-time vs. Run-time (編譯時期 vs. 執行時期)**
   - **編譯時期元程式設計**
     在程式被編譯成可執行檔的過程中執行，其結果是直接改變了最終產生的程式碼。C++ Templates 是典型代表。優點是沒有執行時期的效能開銷。
   - **執行時期元程式設計**
     在程式執行的過程中動態地改變其行為。Python 的元程式設計大部分屬於此類。優點是極度靈活，可以根據執行環境的變化做出反應。

5. **Code Generation (程式碼生成)**

   元程式設計的一種直接應用，即根據某些輸入或範本，自動產生出程式原始碼。例如，許多框架會根據一個設定檔或資料庫 schema 來自動產生對應的類別定義。

---

## 準則與規範 (Principles)

1. **以簡化為目的，而非炫技 (Simplify, Don't Complicate)**

   元程式設計的初衷是為了減少重複、抽象化模式，從而簡化整體程式碼庫。如果一段元程式碼比它所要取代的樣板程式碼更難理解、更難維護，那就違背了其初衷。

   ```python
   # 反面教材: 過於複雜的元程式碼可能得不償失
   # 假設我們只是想為函式增加一個簡單的日誌

   # 過度設計的元程式:
   class MetaLogger(type):
       def __new__(cls, name, bases, dct):
           for item_name, item_value in dct.items():
               if callable(item_value):
                   # ... 複雜的包裝邏輯 ...
                   pass
           return super().__new__(cls, name, bases, dct)

   # 簡單直接的方式 (使用裝飾器):
   def log_decorator(func):
       def wrapper(*args, **kwargs):
           print(f"Calling {func.__name__}")
           return func(*args, **kwargs)
       return wrapper

   @log_decorator
   def my_function():
       pass
   ```

   對於增加日誌這樣簡單的需求，使用直觀的裝飾器遠比設計一個複雜的元類別要清晰明瞭。元類別是用於解決類別創建層面的問題，用在這裡屬於「殺雞用牛刀」。

2. **優先使用更高層、更簡單的工具 (Prefer Higher-Level Tools)**

   Python 提供了不同層次的元程式設計工具。其複雜度和威力大致排序為：裝飾器 < 動態類型創建 < 元類別。應優先選擇能解決問題的最簡單的工具。只有在裝飾器無法解決問題時，才考慮動態類型；只有在動態類型也無法滿足需求時，才動用元類別。

   ```python
   # 需求: 為一個類別的所有方法都加上日誌

   # 方案1: 使用裝飾器 (需要手動為每個方法添加)
   class MyClass:
       @log_decorator
       def method1(self): pass
       @log_decorator
       def method2(self): pass

   # 方案2: 在 __init__ 中動態替換方法 (一種反射技巧)
   # (這個方案其實更複雜，但展示了另一種思路)

   # 方案3: 使用類別裝飾器或元類別 (更自動化)
   def log_all_methods(cls):
       for name, value in vars(cls).items():
           if callable(value):
               setattr(cls, name, log_decorator(value))
       return cls

   @log_all_methods
   class MyApi:
       def fetch_data(self): pass
       def process_data(self): pass
   ```

   `log_all_methods` 是一個類別裝飾器，它在類別定義完成後，遍歷其所有方法並自動應用 `log_decorator`。這個方案比手動裝飾每個方法更自動化，但又比使用元類別簡單。這是在「手動」和「終極武器元類別」之間的一個很好的平衡點。

3. **魔法應被封印和標記 (Isolate and Document the Magic)**

   元程式設計通常被稱為「魔法」(magic)，因為它在背後隱式地改變了程式的行為。好的實踐是將這些「魔法」程式碼集中在特定的模組中（例如 `core` 或 `utils` 模組），並提供非常詳盡的註解和文件，解釋它們做了什麼、為什麼這麼做以及如何使用。

   ```python
   # in my_framework/magic.py

   def auto_register(registry):
       """
       A class decorator that automatically registers the decorated class
       into the given registry dictionary.

       Usage:
           my_plugin_registry = {}

           @auto_register(my_plugin_registry)
           class MyPlugin:
               pass

           assert "MyPlugin" in my_plugin_registry
       """
       def decorator(cls):
           registry[cls.__name__] = cls
           return cls
       return decorator
   ```

   `auto_register` 這個元程式設計工具放在一個獨立的檔案中，並提供清晰的 docstring 作為文件。這樣，使用者即使不了解其內部實現，也能透過文件知道它的功能和用法，同時也知道「魔法」的來源在哪裡。

---

## 實踐方法

1. **自省與反射 (Introspection & Reflection)**

   這是元程式設計的基礎。自省是獲取物件內部資訊的能力，而反射是動態修改這些資訊的能力。Python 提供了豐富的內建函式來支持這些操作。

   ```python
   class Person:
       def __init__(self, name, age):
           self.name = name
           self.age = age

       def greet(self):
           return f"Hello, my name is {self.name}."

   p = Person("Alice", 30)

   # --- 自省 (Introspection) ---
   print(f"Object type: {type(p)}")  # 獲取類型
   print(f"Has 'age' attribute? {hasattr(p, 'age')}") # 檢查屬性是否存在
   print(f"Value of 'name': {getattr(p, 'name')}") # 獲取屬性的值

   # 使用 inspect 模組獲取更詳細資訊
   import inspect
   print(f"Methods: {[name for name, _ in inspect.getmembers(p, inspect.ismethod)]}")

   # --- 反射 (Reflection) ---
   # 動態設定屬性
   setattr(p, 'email', 'alice@example.com')
   print(f"New email attribute: {p.email}")

   # 動態呼叫方法
   method_name = "greet"
   greeting_method = getattr(p, method_name)
   print(f"Calling '{method_name}': {greeting_method()}")
   ```

   - `type()`, `hasattr()`, `getattr()` 是最常用的自省工具，它們允許你在執行期間查詢物件的類型和屬性。
   - `inspect` 模組提供了更強大的自省能力，可以獲取函式簽名、原始碼等。
   - `setattr()` 是反射的核心，它允許你動態地在物件上新增或修改屬性。
   - 將 `getattr()` 和函式呼叫結合，可以實現依據字串名稱來動態呼叫方法，這在插件系統或事件處理中非常有用。

2. **裝飾器 (Decorators)**

   裝飾器本質上是一個高階函式，它接收一個函式（或類別）作為輸入，並回傳一個新的函式（或類別），用以包裝或修改原始對象的行為。這是 Python 中最常見、最直觀的元程式設計形式。

   ```python
   import time

   def timing_decorator(func):
       """一個計算函式執行時間的裝飾器"""
       def wrapper(*args, **kwargs):
           start_time = time.time()
           result = func(*args, **kwargs)
           end_time = time.time()
           print(f"'{func.__name__}' executed in {end_time - start_time:.4f}s")
           return result
       return wrapper

   @timing_decorator
   def process_large_data(size):
       """模擬一個耗時的操作"""
       print("Processing data...")
       time.sleep(size)
       print("Processing complete.")

   process_large_data(1)
   ```

   `@timing_decorator` 是一種語法糖，它等同於 `process_large_data = timing_decorator(process_large_data)`。
   1. `timing_decorator` 接收原始的 `process_large_data` 函式作為 `func` 參數。
   2. 它定義了一個新的內部函式 `wrapper`。這個 `wrapper` 函式包含了我們想要新增的計時邏輯。
   3. 在 `wrapper` 內部，它仍然會呼叫原始的 `func` 函式來執行核心任務。
   4. 最後，`timing_decorator` 回傳 `wrapper` 函式，Python 會用這個新的 `wrapper` 函式來取代原本的 `process_large_data`。
      這樣，我們就在不修改 `process_large_data` 原始碼的情況下，為其增加了計時功能。

3. **動態類型與工廠 (Dynamic Type Creation)**

   Python 的 `type()` 函式除了可以用來獲取物件的類型外，它本身也是一個元類別。當以三個參數 `type(name, bases, dict)` 的形式呼叫時，它可以在執行時期動態地建立一個新的類別。

   ```python
   def create_dynamic_class(class_name, fields):
       """一個動態建立類別的工廠函式"""

       # 定義 __init__ 方法
       def __init__(self, **kwargs):
           for key, value in kwargs.items():
               if key in fields:
                   setattr(self, key, value)

       # 組合類別的屬性和方法
       class_attributes = {
           '__init__': __init__,
           '__slots__': fields # 優化記憶體
       }

       # 使用 type() 動態建立類別
       DynamicClass = type(class_name, (object,), class_attributes)
       return DynamicClass

   # 根據一個 schema 動態建立一個 'Person' 類別
   PersonClass = create_dynamic_class('Person', ['name', 'age'])

   # 像普通類別一樣實例化
   p1 = PersonClass(name="Bob", age=42)
   print(p1.name, p1.age)

   # 再建立一個 'Car' 類別
   CarClass = create_dynamic_class('Car', ['make', 'model'])
   c1 = CarClass(make='Toyota', model='Corolla')
   print(c1.make, c1.model)
   ```

   `create_dynamic_class` 是一個類別工廠。它接收類別名稱和屬性欄位列表，然後：
   1. 動態定義了一個 `__init__` 方法，這個方法會根據傳入的 `fields` 來設定實例的屬性。
   2. 將這個 `__init__` 方法和其他屬性（如 `__slots__`）放入 `class_attributes` 字典中。
   3. 呼叫 `type(class_name, (object,), class_attributes)`，它會創建一個名為 `class_name`、繼承自 `object`、並擁有 `class_attributes` 中定義的屬性和方法的新類別。
      這個技術在 ORM 或從 JSON/XML schema 動態生成對應物件時非常有用。

4. **元類別 (Metaclasses)**

   元類別是「創建類別的類別」。在 Python 中，`type` 是預設的元類別。當你定義一個類別時 (`class MyClass: ...`)，Python 直譯器實際上是在呼叫元類別來創建 `MyClass` 這個物件。透過自定義元類別，你可以在類別被「創建」的那個時間點進行攔截和修改。

   (實現一個自動註冊插件的元類別)

   ```python
   # 插件註冊表
   PLUGIN_REGISTRY = {}

   class PluginMeta(type):
       """一個在創建類別時自動註冊的元類別"""
       def __new__(cls, name, bases, dct):
           # cls: PluginMeta 本身
           # name: 正在創建的類別名稱 (e.g., "ImagePlugin")
           # bases: 繼承的父類別元組
           # dct: 包含類別屬性和方法的字典

           # 創建新的類別物件
           new_class = super().__new__(cls, name, bases, dct)

           # 自動註冊 (排除基底類別本身)
           if name != "BasePlugin":
               PLUGIN_REGISTRY[name] = new_class
               print(f"Registered plugin: {name}")

           return new_class

   # 所有插件都應繼承自這個基底類別，並使用我們的元類別
   class BasePlugin(metaclass=PluginMeta):
       pass

   # 定義插件時，元類別會自動執行
   class ImagePlugin(BasePlugin):
       def process(self, data):
           print("Processing an image.")

   class VideoPlugin(BasePlugin):
       def process(self, data):
           print("Processing a video.")

   print("Available plugins:", PLUGIN_REGISTRY)
   ```

   1. 定義 `PluginMeta`，它繼承自 `type`。
   2. 重寫 `__new__` 方法，這是類別物件被創建時呼叫的第一個方法。
   3. 在 `__new__` 中，首先呼叫 `super().__new__` 來讓 `type` 完成標準的類別創建工作，得到 `new_class`。
   4. 接著，執行我們的「魔法」：將這個新創建的 `new_class` 添加到全域的 `PLUGIN_REGISTRY` 中。
   5. `BasePlugin` 透過 `metaclass=PluginMeta` 指定了它的元類別。
   6. 之後，任何繼承自 `BasePlugin` 的子類別（如 `ImagePlugin`）在定義時，Python 都會使用 `PluginMeta` 來創建它們，從而觸發我們的自動註冊邏輯。這是裝飾器無法輕易做到的，因為元類別能影響整個繼承體系。

---

## 特色分析

- **優點**:
  - **減少重複 (DRY - Don't Repeat Yourself)**: 能夠將重複的樣板程式碼抽象化，自動生成，極大減少手動編寫的程式碼量。
  - **建立領域特定語言 (DSLs)**: 可以創造出更具表達力、更貼近業務領域的語法。例如，ORM 讓你可以用物件的方式操作資料庫，而不需要寫 SQL。
  - **增強框架能力**: 是許多強大框架（如 Django, SQLAlchemy, FastAPI）的基石。它們利用元程式設計來實現路由、模型定義、依賴注入等核心功能。
  - **高度抽象化**: 允許建立非常通用和可擴展的組件，因為它們可以在執行時期適應不同的需求。

- **缺點**:
  - **增加複雜度與心智負擔**: 元程式碼通常比普通程式碼更難理解和推理。它在背後隱式地做了很多工作，使得程式的實際行為不那麼直觀。
  - **除錯困難**: 當「魔法」出錯時，除錯會變得很困難。錯誤堆疊(traceback)可能會指向元程式碼內部，而不是你所編寫的業務邏輯程式碼，難以定位問題根源。
  - **影響 IDE 分析和開發體驗**: 過多的動態行為會讓靜態分析工具、自動補全和類型檢查器失效，因為它們無法在不執行程式的情況下，預測程式在執行時期的結構。
  - **容易被濫用**: 強大的工具容易被用在不恰當的地方，導致產生過度設計、難以維護的「炫技」程式碼。

- **情境**:
  - **框架開發 (Framework Development)**: 當你需要建立一個提供通用模式給其他人使用的工具時，例如 Web 框架、ORM、測試框架。
  - **插件式架構 (Plugin Architectures)**: 當你需要一個核心系統，並允許第三方動態地註冊新功能或組件時。
  - **API 或程式碼生成器**: 例如，根據一個 OpenAPI/Swagger 規範自動生成客戶端 SDK，或根據資料庫 schema 生成對應的資料模型。
  - **強制執行程式碼規範 (Enforcing Conventions)**: 使用元類別來確保一個繼承體系下的所有子類別都遵循某些規則，例如「所有公開方法都必須有文件字串」。
  - **ORM (Object-Relational Mapping)**: 將程式中的物件對應到資料庫中的表格，元程式設計在此用於讀取類別定義並生成對應的 SQL。
