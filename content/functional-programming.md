# Functional Programming

## 簡介說明

`Functional Programming` (`FP`) 是一種程式設計範式 (programming paradigm)，其核心思想是將電腦運算視為數學函數的計算，並避免使用可變狀態 (mutable state) 和共享資料。它是一種宣告式程式設計 (declarative programming) 的風格，程式碼主要由表達式 (expressions) 和宣告 (declarations) 組成，而不是指令式的語句 (statements)。

在函數式程式碼中，一個函數的輸出值僅僅取決於它的輸入參數，因此使用相同的參數重複呼叫該函數，永遠會得到相同的結果。這與指令式程式設計 (imperative programming) 形成對比，後者中除了函數的參數外，全域的程式狀態也可能影響函數的執行結果。

---

## 演進過程與歷史 (History)

- **1930年代**: 函數式程式設計的理論基礎由數學家 Alonzo Church 在1930年代發明的 **Lambda Calculus (λ-calculus)** 所奠定。Lambda Calculus 是一個用於研究函數定義、函數應用和遞迴的數理邏輯形式系統。它為函數式程式設計提供了理論基石，解釋了運算如何透過函數來抽象化和執行。

- **1950年代末期**: John McCarthy 在麻省理工學院 (MIT) 開發了第一套實際的函數式程式語言 **LISP (List Processing)**，主要用於人工智慧領域的研究。LISP 的發展動機是為了解決 AI 問題中複雜的符號運算需求，它引入了許多現今函數式語言的共通特性，如 list 結構、遞迴和高階函數。

- **1970年代**:
  - 愛丁堡大學的一組研究人員開發了 **ML (MetaLanguage)**。其中的關鍵人物 Robin Milner 為 ML 開發了一套創新的**型別推論 (type inference)** 演算法。這讓開發者在享受靜態型別帶來的好處時，無需手動編寫繁瑣的型別宣告，大大提高了開發效率。
  - Fortran 的創造者 John Backus 在其圖靈獎演講中，發表了 "Can Programming Be Liberated from the von Neumann Style? A Functional Style and Its Algebra of Programs"，大力提倡函數式程式設計風格。他認為傳統的指令式程式設計（馮·諾伊曼風格）過於複雜且容易出錯，而函數式風格能提供更數學化、更可靠的程式建構方式。

- **1980年代**: 出現了幾種重要的函數式語言，例如 David Turner 開發的 **Miranda**，它是一種**惰性求值 (lazy evaluation)** 的函數式語言，後來深刻影響了 Haskell。惰性求值的特性是運算只在真正需要其結果時才進行，這使得處理無窮資料結構和優化計算成為可能。

- **1990年代**: 一個由研究人員組成的委員會標準化了一種惰性求值函數式語言，最終成果就是 **Haskell**，於1990年首次發布。Haskell 的誕生是為了統一當時各種惰性求值函數式語言的研究，提供一個共通的平台。其命名是為了紀念邏輯學家 Haskell Curry。

- **2000年代至今**: 函數式程式設計的概念越來越被主流程式語言所採納。這主要是因為隨著網路和資料量的爆發，處理並行 (concurrency) 和平行 (parallelism) 運算的需求大增。函數式程式設計強調的**不可變性 (immutability)** 能有效避免多執行緒環境下的資料競爭 (race condition) 問題，使得編寫安全可靠的並行程式變得更加容易。因此，像 Lambda 運算式、`map`、`filter`、`reduce` 等功能，現在在 Python, Java, C#, JavaScript 等語言中都相當普遍。

---

## 概念 (Concept)

1. **First-Class Functions (一級函式)**

   在語言中，函式被視為與其他資料型別（如整數、字串）同等的「一級公民」。這意味著函式可以被指派給變數、可以作為參數傳遞給其他函式，也可以作為函式的回傳值。

   ```python
   def greet(name):
       return f"Hello, {name}"

   # 函式可以被指派給變數
   say_hello = greet
   print(say_hello("World")) # Output: Hello, World

   # 函式可以作為參數傳遞
   def process_greeting(greeter_func, name):
       return greeter_func(name)

   print(process_greeting(greet, "Alice")) # Output: Hello, Alice
   ```

2. **Higher-Order Functions (高階函式)**

   一個函式可以接受另一個函式作為參數，或者將函式作為回傳結果。`map` 和 `filter` 都是典型的高階函式。

   ```python
   def multiplier(factor):
       # 回傳一個函式
       def multiply_by_factor(number):
           return number * factor
       return multiply_by_factor

   # doubler 是一個由 multiplier 回傳的函式
   doubler = multiplier(2)
   print(doubler(5)) # Output: 10
   ```

3. **Pure Functions (純函式)**

   一個函式的回傳值僅由其輸入值決定，並且在執行過程中不會產生任何可觀察的副作用 (side effects)。

   ```python
   # Pure function: 相同的輸入永遠有相同的輸出
   def add(a, b):
       return a + b

   # Impure function: 它的輸出受到外部狀態影響
   discount = 0.9
   def calculate_price(price):
       return price * discount # 依賴於外部變數 discount
   ```

4. **Immutability (不可變性)**

   資料一旦被建立，其狀態就不能再被改變。如果需要修改資料，程式會建立一個帶有更新值的新資料結構。

   ```python
   # Python 的 tuple 是不可變的
   original_tuple = (1, 2, 3)
   # 試圖修改會報錯: TypeError: 'tuple' object does not support item assignment
   # original_tuple[0] = 99

   # 若要 "修改", 實際上是建立一個新的 tuple
   new_tuple = original_tuple + (4,)
   print(original_tuple) # Output: (1, 2, 3)
   print(new_tuple)      # Output: (1, 2, 3, 4)
   ```

5. **Referential Transparency (參照透明性)**

   一個表達式可以用其對應的計算結果替換，而不會改變整個程式的行為，那麼這個表達式就具有參照透明性。純函式呼叫是參照透明的。

   ```python
   def square(x):
       return x * x

   # 在表達式 `square(5) + square(5)` 中
   # `square(5)` 永遠回傳 25
   # 因此，這個表達式等同於 `25 + 25`
   # 可用值 `25` 替換 `square(5)` 這個函式呼叫，而不影響程式結果。
   # 這就是參照透明性。
   result = square(5) + square(5) # 50
   result_transparent = 25 + 25    # 50
   ```

6. **Recursion (遞迴)**

   函數式程式設計不鼓勵使用傳統的 `for` 或 `while` 迴圈，而是使用遞迴（函式呼叫自身）來完成迭代任務。

   ```python
   def factorial(n):
       # Base case: 遞迴的終止條件
       if n == 0:
           return 1
       # Recursive step: 函式呼叫自身
       else:
           return n * factorial(n - 1)

   print(factorial(5)) # Output: 120
   ```

7. **Function Composition (函式組合)**

   將兩個或多個函式組合起來以產生一個新函式的過程，例如，將 `f` 和 `g` 組合會產生一個新函式 `h(x) = f(g(x))`。

   ```python
   def double(x):
       return x * 2

   def add_one(x):
       return x + 1

   # 組合 double 和 add_one 函式
   def double_then_add_one(x):
       return add_one(double(x))

   # 先將數字乘以 2，然後再加 1
   print(double_then_add_one(5)) # Output: 11
   ```

---

## 準則與規範 (Principles)

1. **數據與邏輯分離 (Separation of Data and Logic)**
   - 這是函數式思維的基石。數據應該是單純的、被動的資料結構（如字典、列表、元組），而邏輯則是由一系列無狀態的函式組成。函式接受數據作為輸入，並回傳新的數據作為輸出，但函式本身不應持有狀態或與數據耦合。
   - 極大提高了程式碼的可複用性和可測試性。純粹的邏輯函式可以獨立測試，也可以應用於不同的數據上。數據結構因為不包含邏輯，所以可以輕易地被序列化、傳輸或儲存。
   - 對於習慣物件導向（將數據和操作數據的方法綁定在一起）的開發者來說，需要轉換思維模式。在某些情況下，這種分離可能看起來不如物件導向直觀。

   ```python
   # Data (純粹的數據結構)
   user_data = {"name": "Alice", "credits": 100, "is_active": True}

   # Logic (純粹的、無狀態的函式)
   def has_enough_credits(user, required_credits):
       return user["credits"] >= required_credits

   def deactivate_user(user):
       # 不修改原數據，回傳一個新的數據副本
       new_user = user.copy()
       new_user["is_active"] = False
       return new_user

   # 使用邏輯函式處理數據
   if has_enough_credits(user_data, 50):
       print(f"{user_data['name']} has enough credits.")

   updated_user = deactivate_user(user_data)
   print("Original user:", user_data)
   print("Updated user:", updated_user)
   ```

   `user_data` 是一個簡單的字典，只負責儲存資料。`has_enough_credits` 和 `deactivate_user` 是獨立的函式，它們接收 `user` 數據，執行判斷或轉換，然後回傳結果。`deactivate_user` 遵循了不可變性原則，它回傳了一個新的 user 物件，而不是修改原始的 `user_data`。

2. **避免共享狀態 (No Shared State)**
   - 指多個函式或執行緒不應依賴或修改同一個可變的外部狀態。共享狀態是造成並行程式設計中 bug (如 race condition) 的主要來源。函數式編程透過傳遞和回傳值來管理狀態，而不是透過共享的記憶體位置。
   - 使並行和非同步程式設計變得極為簡單和安全。因為沒有共享狀態需要被鎖定或保護，所以程式碼不容易出錯，也更容易擴展到多核心處理器。
   - 在某些需要全域設定或快取的場景，完全避免共享狀態可能會讓架構變得複雜。有時需要透過更進階的模式（如依賴注入）來管理這些「類狀態」的配置。

   ```python
   # 不好的範例: 使用共享狀態
   class Counter:
       def __init__(self):
           self.count = 0 # 共享狀態

       def increment(self):
           self.count += 1
           return self.count

   # 好的範例: 避免共享狀態
   def increment_pure(count_value):
       # 函式只依賴傳入的參數，並回傳新值
       return count_value + 1

   # 狀態的管理由呼叫者負責
   current_count = 0
   current_count = increment_pure(current_count)
   print(f"New count: {current_count}")
   current_count = increment_pure(current_count)
   print(f"New count: {current_count}")
   ```

   `Counter` 類別將 `count` 作為一個可變的共享狀態。如果多個執行緒同時呼叫 `increment`，結果可能無法預測。而 `increment_pure` 函式本身不持有任何狀態，它只是一個計算工具。狀態 `current_count` 的生命週期由外部程式碼明確地管理，每次更新都是透過函式回傳的新值來取代舊值。

3. **宣告式轉換 (Declarative Data Transformations)**
   - 提倡使用宣告式的方法（如 `map`, `filter`, `reduce`）來描述數據的轉換流程，而不是使用指令式迴圈（如 `for`, `while`）來詳細指定每一步操作。你只需要「宣告」你想要的結果，而不需要關心「如何」實現它。
   - 程式碼意圖更清晰，可讀性更高。將轉換邏輯與迭代機制分離，使程式碼更簡潔、更不容易出錯。
   - 對於複雜的、包含多重條件和中斷的迴圈，用宣告式方法重構可能會變得不那麼直觀。在某些極端情況下，手寫的指令式迴圈可能會有微小的效能優勢。

   ```python
   users = [
       {"name": "Alice", "age": 25},
       {"name": "Bob", "age": 17},
       {"name": "Charlie", "age": 30},
   ]

   # 指令式 (Imperative) 寫法
   adult_names_imperative = []
   for user in users:
       if user["age"] >= 18:
           adult_names_imperative.append(user["name"].upper())

   # 宣告式 (Declarative) 寫法
   adult_names_declarative = list(
       map(lambda name: name.upper(),
           filter(lambda user: user["age"] >= 18,
                  map(lambda user: user["name"], users)
           )
       )
   )
   # 更 Pythonic 的宣告式寫法 (List Comprehension)
   adult_names_pythonic = [user["name"].upper() for user in users if user["age"] >= 18]
   ```

   指令式寫法詳細描述了迴圈、條件判斷和 list 添加的每一步。而宣告式寫法（無論是 `map/filter` 鏈還是列表生成式）都只是在描述一個轉換流程：「從 users 中，過濾出年齡大於等於18的，提取他們的名字，並轉換為大寫」。程式碼本身就像是對需求的描述。

4. **顯性化 (Explicitness)**
   - 函式的依賴和產出都應該是顯性的。這意味著函式所有需要的東西都應該透過參數傳入，所有產生的東西都應該透過回傳值傳出。函式不應依賴隱藏的輸入（如全域變數、環境設定），也不應有隱藏的輸出（如修改傳入的物件、寫入檔案等副作用）。
   - 函式變得完全自包含 (self-contained) 和可移植。你可以輕易地將函式拿到任何地方去用，只要你提供了它需要的參數。這也使得函式的簽名（名稱和參數）像一份清晰的說明書。
   - 如果一個函式需要很多配置或依賴，可能會導致參數列表過長，降低可讀性。這時可以透過傳入一個配置物件或使用部份應用 (Partial Application) 等技巧來緩解。

   ```python
   # 不好的範例: 隱性依賴
   db_connection = get_db_connection() # 全域的連線

   def get_user_from_db(user_id):
       # 隱性地依賴了全域變數 db_connection
       return db_connection.query(f"SELECT * FROM users WHERE id = {user_id}")

   # 好的範例: 顯性依賴
   def get_user_from_db_explicit(user_id, connection):
       # 所有依賴都透過參數傳入
       return connection.query(f"SELECT * FROM users WHERE id = {user_id}")

   # 呼叫時明確傳入依賴
   # user = get_user_from_db_explicit(123, db_connection)
   ```

   `get_user_from_db` 函式看起來很簡單，但它偷偷地依賴了一個我們從函式簽名上看不出來的全域變數 `db_connection`。這使得它難以在沒有這個全域變數的環境中測試或複用。`get_user_from_db_explicit` 則將 `connection` 作為一個顯性的參數，清楚地表明了它的依賴，使其成為一個更健壯、更可預測的函式。

---

## 實踐方法

1. **管道化與組合 (Pipelining & Composition)**
   - 是將多個小而專一的函式串連起來，以執行更複雜任務的技術。數據像水流一樣在函式管道中流動，每個函式都對數據進行一次處理，並將結果傳遞給下一個函式。函式組合 (Composition) 是創建這個管道的過程。
   - 提高程式碼的可讀性和可維護性。每個函式都只做一件事，易於理解和測試。複雜的流程被分解成一系列簡單的步驟，使得修改或重組流程變得容易。
   - 如果沒有輔助工具，在某些語言（包括 Python）中，深度的函式組合可能會導致語法上的巢狀結構，可讀性反而下降。

     ```python
     # 假設我們有一系列操作函式
     def get_name(user):
         return user["name"]

     def uppercase(s):
         return s.upper()

     def greet(name):
         return f"Hello, {name}!"

     # 一般的巢狀組合
     user = {"name": "Alice"}
     result_nested = greet(uppercase(get_name(user)))
     print(result_nested) # Output: Hello, ALICE!

     # 使用管道輔助函式（需要自己實現或使用第三方庫）
     # pip install toolz
     from toolz import pipe

     result_piped = pipe(user, get_name, uppercase, greet)
     print(result_piped) # Output: Hello, ALICE!
     ```

   巢狀寫法從內到外執行，當鏈條很長時會變得很難閱讀。`toolz.pipe` 函式提供了一種更自然的方式，它將第一個參數 `user` 傳遞給 `get_name`，然後將其結果傳遞給 `uppercase`，再將其結果傳遞給 `greet`。這種從左到右的線性流程非常符合人類的閱讀習慣，使得數據的轉換過程一目了然。

2. **柯里化 (Currying) 與 部份應用 (Partial Application)**
   - **柯里化 (Currying)**: 將一個接受多個參數的函式，轉換成一系列只接受單一參數的函式。例如，`f(a, b, c)` 經過柯里化後會變成 `f(a)(b)(c)`。
   - **部份應用 (Partial Application)**: 將一個多參數的函式，預先固定其中一個或多個參數，從而產生一個新的、參數更少的函式。
   - 這是創造可複用函式的重要技巧。透過固定參數，你可以從一個通用的函式中，產生出許多特製化的、更方便使用的新函式。
   - 濫用可能導致程式碼中出現大量細小的、功能單一的函式，增加管理的複雜性。柯里化在 Python 中不是原生支援的，需要手動實現或使用庫，語法上不如原生支援的語言（如 Haskell）優雅。

   ```python
   from functools import partial

   # 一個通用的函式
   def power(base, exponent):
       return base ** exponent

   # 1. 部份應用 (Partial Application)
   # 建立一個特製化的 "square" 函式，固定 exponent 為 2
   square = partial(power, exponent=2)
   # 建立一個 "cube" 函式
   cube = partial(power, exponent=3)

   print(square(5)) # Output: 25
   print(cube(5))   # Output: 125

   # 2. 柯里化 (Currying) 的手動實現
   def curried_power(base):
       def inner_power(exponent):
           return base ** exponent
       return inner_power

   power_of_2 = curried_power(2)
   print(power_of_2(3)) # Output: 8 (2的3次方)
   ```

   `functools.partial` 是 Python 內建的部份應用工具。我們從通用的 `power` 函式中，透過預先填入 `exponent` 參數，輕鬆地創造出了 `square` 和 `cube` 兩個新函式。手動實現的 `curried_power` 展示了柯里化的思想：它先接收 `base` 參數，然後回傳一個等待 `exponent` 參數的新函式。

3. **遞迴代替迴圈 (Recursion over Loops)**
   - 在純函數式編程中，迴圈因為通常伴隨著可變狀態（如迴圈計數器）而被視為一種副作用。因此，函數式編程使用遞迴（函式呼叫自身）來實現迭代。每個遞迴呼叫都處理問題的一小部分，直到達到終止條件 (base case)。
   - 遞迴的寫法通常更接近問題的數學定義，程式碼更簡潔、更具宣告性。它自然地避免了共享狀態和副作用。
   - 最大的缺點是效能和記憶體。每次函式呼叫都會在呼叫堆疊 (call stack) 上新增一幀，如果遞迴深度太大，會導致堆疊溢位 (stack overflow)。很多函數式語言透過「尾呼叫優化 (TCO)」來解決此問題，但 Python 預設並不支援。

     ```python
     # 使用迴圈計算列表總和
     def sum_loop(items):
         total = 0
         for item in items:
             total += item
         return total

     # 使用遞迴計算列表總和
     def sum_recursive(items):
         # Base case: 如果列表為空，總和為 0
         if not items:
             return 0
         # Recursive step: 列表的第一個元素 + 剩餘部分的總和
         return items[0] + sum_recursive(items[1:])

     my_list = [1, 2, 3, 4, 5]
     print(sum_loop(my_list))       # Output: 15
     print(sum_recursive(my_list))  # Output: 15
     ```

   `sum_loop` 使用了一個可變的 `total` 變數。`sum_recursive` 則完全沒有可變變數。它將問題分解為「取第一個元素」和「計算剩餘列表的總和」（這是一個規模更小的相同問題），完美地體現了遞迴的分解思想。但要注意，這個 Python 範例在處理長列表時會有堆疊溢位的風險。

4. **封裝副作用 (Functors & Monads)**
   - 這是函數式編程中處理副作用（如 I/O、錯誤處理、非同步操作）的進階模式。
   - **Functor**: 是一個「容器」(wrapper)，它包裝了一個值。這個容器提供了一個 `map` 方法，可以讓你用一個函式去操作被包裝的值，而無需將值從容器中取出。`map` 會回傳一個包裝了新值的新容器。
   - **Monad**: 是一種更強大的 Functor。除了 `map`，它還提供了一個 `flatMap` (或 `bind`) 方法。`flatMap` 允許你將一個會回傳新容器的函式應用於被包裝的值，並能聰明地將巢狀的容器「攤平」(flatten)。這使得將一系列可能產生副作用的操作串連起來成為可能。
   - 它們提供了一種在保持程式碼純粹性的同時，又能優雅地處理和隔離副作用的方法。Monad 使得錯誤處理、非同步流程控制的程式碼可以寫成清晰的線性管道。
   - 這是函數式編程中最難以理解的概念之一，學習曲線非常陡峭。在 Python 這種沒有原生語法支援的語言中，實現和使用起來比較繁瑣。

   (用一個簡化的 `Maybe` Monad 來展示概念)

   ```python
   # 概念性範例，非生產級代碼
   class Maybe:
       def __init__(self, value):
           self._value = value

       @classmethod
       def of(cls, value):
           return Maybe(value)

       def is_nothing(self):
           return self._value is None

       # Functor 的 map 方法
       def map(self, func):
           if self.is_nothing():
               return Maybe.of(None)
           return Maybe.of(func(self._value))

       # Monad 的 flatMap 方法
       def flatMap(self, func):
           if self.is_nothing():
               return Maybe.of(None)
           # func 預期回傳一個新的 Maybe 容器
           return func(self._value)

   # 假設有兩個可能失敗的函式
   def get_user_by_id(uid):
       users = {1: {"name": "Alice"}}
       return Maybe.of(users.get(uid))

   def get_name(user):
       return Maybe.of(user.get("name"))

   # 使用 Monad 管道來安全地獲取名字
   user_name = get_user_by_id(1).flatMap(get_name)
   print(user_name._value) # Output: Alice

   # 如果 user id 不存在，整個鏈會安全地失敗
   failed_name = get_user_by_id(2).flatMap(get_name)
   print(failed_name._value) # Output: None
   ```

   `Maybe` 是一個容器，它要麼包含一個值 (`Just a value`)，要麼是空的 (`Nothing`)。`get_user_by_id` 和 `get_name` 都回傳 `Maybe` 容器，表示它們的操作可能失敗。`flatMap` 的作用就是安全地串接這些操作：如果前一步是 `Nothing`，後續所有步驟都會被跳過，直接回傳 `Nothing`。這就避免了在每一步都寫 `if value is not None:` 這樣的檢查，讓程式碼變得非常乾淨。

---

## 特色分析

- **優點**
  - **程式碼更簡潔、可預測**: 由於使用了純函式和不可變數據，函式的行為只由其輸入決定，極大地降低了程式的複雜性，使其更容易推理和預測。
  - **易於除錯與單元測試**: 對於純函式，測試變得非常簡單。你只需要提供一組輸入，然後驗證輸出是否符合預期，無需擔心外部狀態或模擬複雜的環境。
  - **天然適合並行/平行運算**: 不可變性從根本上避免了在多執行緒環境中修改共享數據時可能出現的競爭條件 (race conditions) 和死鎖問題，使得編寫並行程式碼更加安全和簡單。
  - **可組合性與可複用性**: 函數式編程鼓勵編寫小而專一的函式，然後像積木一樣將它們組合起來，形成更複雜的功能。這種高度的模組化使得程式碼更容易複用。

- **缺點**
  - **學習曲線較陡峭**: 對於習慣了指令式編程的開發者來說，函數式編程中的一些概念（如高階函式、Monads、遞迴思維）可能需要一些時間來適應。
  - **效能問題**: 不可變性意味著每次資料變更都會建立新的物件，這可能導致記憶體使用量增加和額外的垃圾回收負擔。此外，過度使用遞迴而沒有尾呼叫優化 (Python 預設不支援) 可能會導致堆疊溢位。
  - **處理 I/O 和狀態較為複雜**: 在純函數式編程中，處理有副作用的操作（如檔案讀寫、網路請求）需要一些特殊的模式 (如 Monads) 來隔離副作用，這會增加程式的複雜度。

- **情境**
  - **資料處理與轉換**: ETL (Extract, Transform, Load) 流程、數據清洗、資料分析等場景。`map`, `filter`, `reduce` 等操作可以非常優雅地處理數據流。
  - **並行與分散式系統**: 大數據處理框架（如 Apache Spark、Flink）的核心就是基於函數式編程的思想，因為它能輕易地將計算任務分發到多個節點上而無需擔心狀態同步問題。
  - **複雜的演算法與數學運算**: 對於數學密集型或演算法複雜的應用，函數式編程的數學基礎可以幫助寫出更正確、更可靠的程式碼。
  - **前端 UI 開發**: 現代前端框架（如 React）也深受函數式思想影響。例如，將 UI 視為狀態的函式 `UI = f(state)`，當狀態改變時，重新渲染整個 UI 組件，這簡化了狀態管理。
