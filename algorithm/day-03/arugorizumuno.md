# アルゴリズムの計算量

### 1. なぜアルゴリズムの効率を考えるのか？

プログラミングでは、「動けばいい」だけでなく「どれだけ速く動くか」も重要です。例えば次の2つのプログラムを見てみましょう。

```python
# 方法1: 1からnまでの合計を計算
def sum_method1(n):
    total = 0
    for i in range(1, n+1):
        total += i
    return total

# 方法2: 公式を使って計算
def sum_method2(n):
    return n * (n + 1) // 2
```

どちらも同じ結果を返しますが、方法1は数が大きくなるほど時間がかかります。方法2は瞬時に答えが出ます。

#### 日常生活の例：

* 電話帳で名前を探すとき、ページをめくるたびに「あ行、か行...」と確認しながら探す方法は、1ページずつ全部見るより効率的です
* お店のレジで会計するとき、商品を1つずつ手計算するより、レジシステムが自動計算する方が高速です

大量のデータを扱う場合、効率の良いアルゴリズムを選ぶことでプログラムの実行時間が数時間から数秒に短縮できることがあります。

### 2. 計算量とオーダー記法

「計算量」とは、アルゴリズムの効率を数学的に表現したものです。主に2つの観点があります：

* **時間計算量 (Time Complexity)**：処理にどれくらいの時間（ステップ数）がかかるか
* **空間計算量 (Space Complexity)**：どれくらいのメモリ容量を必要とするか

本セクションでは主に時間計算量について学びます。

#### 時間計算量の考え方：ステップ数

特定の処理（基本単位）が何回実行されるかを数えることで計算量を評価します。具体例で見ていきましょう。

**例1：リストの中から最大値を探す（線形時間 O(n)）**

```python
def find_max(numbers):
    max_value = numbers[0]  # 最初の要素を仮の最大値とする
    for num in numbers:     # リストの全要素を1回ずつ確認
        if num > max_value:
            max_value = num
    return max_value
```

この場合、リストの要素数を `n` とすると：

* 要素が10個なら、最大10回の比較が必要
* 要素が100個なら、最大100回の比較が必要
* 要素が `n` 個なら、最大 `n` 回の比較が必要

データ量 n に比例してステップ数が増えるため、この計算量は「**O(n)**」と表します。

**例2：2重ループを使った処理（二次時間 O(n²)）**

```python
def print_all_pairs(numbers):
    n = len(numbers)
    for i in range(n):        # n回繰り返す
        for j in range(n):    # それぞれでさらにn回繰り返す
            print(f"({numbers[i]}, {numbers[j]})")
```

この場合：

* 要素が10個なら、10×10=100回の処理
* 要素が100個なら、100×100=10,000回の処理
* 要素が `n` 個なら、`n`×`n`=`n²` 回の処理

データ量 n の **2乗** に比例してステップ数が増えるため、この計算量は「**O(n²)**」と表します。

#### オーダー記法 (O記法 / Big O Notation)

「O(n)」「O(n²)」という表記は「オーダー記法（ビッグオー記法）」と呼ばれ、データ量 n が増えたときに、計算時間（ステップ数）が **どのくらいの勢いで増えるか** を示す記法です。

**オーダー記法のポイント**:

1. **最も影響の大きい項だけを残す**
   * O(n² + n)は単にO(n²)と書きます（nが大きい場合、n²に比べてnは無視できるほど小さくなる）
2. **定数倍は無視する**
   * O(2n)やO(100n)も単にO(n)と書きます（計算量のオーダーだけに着目）
3. **最悪の場合を考える**
   * 最も処理時間がかかるケースを基準に評価します

```python
# この関数の計算量は何でしょう？
def calculate_something(n):
    result = 0
    
    # 部分1: n回の繰り返し - O(n)
    for i in range(n):
        result += i
    
    # 部分2: n²回の繰り返し - O(n²)
    for i in range(n):
        for j in range(n):
            result += i * j
    
    return result
```

部分1はO(n)、部分2はO(n²)なので、全体としては最も増加が激しい項だけを考慮して O(n + n²) = O(n²) となります。

### 3. 代表的な計算量

主要な計算量のオーダーを効率の良い順（小さい順）に並べると次のようになります：

<table><thead><tr><th width="122.79296875">オーダー</th><th>名称</th><th>特徴</th><th>例</th></tr></thead><tbody><tr><td>O(1)</td><td>定数時間</td><td>データ量に関係なく一定時間</td><td>配列の特定要素へのアクセス</td></tr><tr><td>O(log n)</td><td>対数時間</td><td>データが増えても時間はあまり増えない</td><td>二分探索</td></tr><tr><td>O(n)</td><td>線形時間</td><td>データ量に比例して時間が増える</td><td>線形探索、単純なループ</td></tr><tr><td>O(n log n)</td><td>線形対数時間</td><td>効率的なソートアルゴリズム</td><td>マージソート、クイックソート</td></tr><tr><td>O(n²)</td><td>二乗時間</td><td>データ量が増えると急激に時間が増える</td><td>バブルソート、選択ソート</td></tr><tr><td>O(n³)</td><td>三乗時間</td><td>さらに急激に時間が増える</td><td>三重ループの処理</td></tr><tr><td>O(2ⁿ), O(n!)</td><td>指数時間、階乗時間</td><td>現実的な時間で解けるのは小さな問題だけ</td><td>全ての組み合わせを試す問題</td></tr></tbody></table>

#### データ量による計算時間の増加の比較

1回の基本操作が1マイクロ秒(0.000001秒)かかると仮定した場合、データ量nの増加に伴う処理時間の変化：

| データ量    | O(1) | O(log n) | O(n)  | O(n log n) | O(n²) | O(2ⁿ)    |
| ------- | ---- | -------- | ----- | ---------- | ----- | -------- |
| n=10    | 1μs  | 3μs      | 10μs  | 33μs       | 100μs | 1ms      |
| n=100   | 1μs  | 7μs      | 100μs | 664μs      | 10ms  | 10^30秒   |
| n=1000  | 1μs  | 10μs     | 1ms   | 9.97ms     | 1s    | 10^301秒  |
| n=10000 | 1μs  | 13μs     | 10ms  | 130ms      | 100s  | 10^3010秒 |

特にO(2ⁿ)のような指数時間アルゴリズムは、データ量がわずかに増えただけで計算時間が「爆発的」に増加することがわかります。

### 4. アルゴリズムと計算量の実例

#### 探索アルゴリズムの比較

**線形探索（O(n)）**：

```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1
```

**二分探索（O(log n)）**：

```python
def binary_search(sorted_arr, target):
    left, right = 0, len(sorted_arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if sorted_arr[mid] == target:
            return mid
        elif sorted_arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1
```

1000個の要素がある配列で最悪の場合：

* 線形探索：1000回の比較が必要
* 二分探索：約10回（2^10 = 1024）の比較で済む

1,000,000個の要素がある場合：

* 線形探索：1,000,000回の比較
* 二分探索：約20回（2^20 = 1,048,576）

二分探索はデータがソートされている必要がありますが、大規模データセットでは圧倒的に効率的です。

#### ソートアルゴリズムの比較

**バブルソート（O(n²)）**：

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
```

**クイックソート（平均的にO(n log n)）**：

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    
    return quick_sort(left) + middle + quick_sort(right)
```

10,000個の要素をソートする場合の比較回数：

* バブルソート：約1億回（10,000²）の比較操作
* クイックソート：約13万回（10,000 × log₂(10,000) ≈ 10,000 × 13）の比較操作

効率的なソートアルゴリズムを選択することで、データ量が増えるほど処理時間の差が顕著になります。

### 5. データ構造と計算量

アルゴリズムだけでなく、**どのデータ構造を使うか** によっても計算量が大きく変わります。

| 操作          | リスト（配列） | 辞書（ハッシュテーブル） | 理由                  |
| ----------- | ------- | ------------ | ------------------- |
| 要素の検索       | O(n)    | O(1)         | リストは全要素を順に確認する必要がある |
| 要素の追加（末尾）   | O(1)    | O(1)         | どちらも末尾への追加は高速       |
| 要素の追加（任意位置） | O(n)    | -            | リストは要素をずらす必要がある     |
| 要素の削除       | O(n)    | O(1)         | リストは要素をずらす必要がある     |

適切なデータ構造を選ぶことで、同じタスクでも計算量を大幅に削減できる場合があります。

### 6. まとめと実践のポイント

#### 計算量を意識するポイント

1. **ループの入れ子に注意する**
   * 1重ループはO(n)、2重ループはO(n²)、3重ループはO(n³)
   * ループの回数が増えるほど、計算量は指数関数的に増加する
2. **データ量に応じてアルゴリズムを選択する**
   * 少量のデータでは単純なアルゴリズムでも十分速い場合が多い
   * 大量のデータでは効率的なアルゴリズムの違いが劇的に表れる
3. **適切なデータ構造を選択する**
   * 頻繁に検索する場合は辞書（ハッシュテーブル）が有利
   * 要素の順序が重要な場合はリスト（配列）が適している

#### 計算量の重要性

1. **計算量はアルゴリズムの効率を表す指標**
   * 環境に依存しない評価方法として重要
   * データ量に対する処理量の増え方に着目
2. **効率的なアルゴリズム選びが重要**
   * 同じ問題でも、アルゴリズムによって効率が大きく異なる
   * データ量が多いほど、効率の差が顕著になる

効率の良いアルゴリズムとデータ構造を選ぶことで、大規模なデータでも快適に動作するプログラムを作成できます。

### 演習問題

#### 問題1: 計算量の分析

以下の関数の計算量をそれぞれ分析してください。

```python
# 関数A
def function_a(n):
    result = 0
    for i in range(n):
        result += i
    return result

# 関数B
def function_b(n):
    result = 0
    for i in range(n):
        for j in range(i):
            result += j
    return result

# 関数C
def function_c(n):
    result = 0
    i = n
    while i > 0:
        result += i
        i = i // 2
    return result
```

\<details> \<summary>ヒント\</summary>

* 関数Aは1重ループの構造です
* 関数Bは2重ループですが、内側のループの反復回数は変動します
* 関数Cでは、iが毎回半分になっていくことに注目してください

\</details>

#### 問題2: アルゴリズムの選択

10万個の要素を持つリストから特定の値を探すとき、線形探索と二分探索のどちらが適切でしょうか？それぞれの最悪の場合の計算量と予想される比較回数を計算してください。

\<details> \<summary>ヒント\</summary>

* 線形探索の計算量はO(n)です
* 二分探索の計算量はO(log n)ですが、リストがソート済みである必要があります
* log₂(100,000)の値を計算してみましょう

\</details>

#### 問題3: 実装の改善

以下の関数は、リスト内の2つの要素の和が特定の値targetに等しいかどうかを判定します。計算量を分析し、より効率的なアルゴリズムを考えてください。

```python
def find_pair_with_sum(numbers, target):
    n = len(numbers)
    for i in range(n):
        for j in range(i+1, n):
            if numbers[i] + numbers[j] == target:
                return True
    return False
```

\<details> \<summary>ヒント\</summary>

* 現在の実装の計算量はO(n²)です
* ハッシュテーブル（辞書）を使うとO(n)で解決できます
* 「target - 現在の要素」がすでに見た値の中にあるかを効率的に確認する方法を考えてみましょう

\</details>
