# 実践問題 ③

### 1. 探索アルゴリズムの応用

#### 問題 1-1: 数値リストからの探索

数値のリストから特定の条件を満たす数値を探すプログラムを作成してください。

<pre class="language-python"><code class="lang-python"><strong># 数値のリスト
</strong>numbers = [15, 8, 21, 4, 42, 16, 23, 9, 7, 45, 11, 36]

# 線形探索を使って、10より大きく30未満の数値をすべて探す関数を作成してください
def find_between_10_and_30(numbers_list):
    """10より大きく30未満の数値をすべて探す関数"""
    # ここに線形探索のコードを書いてください
    
    
# 関数を実行して結果を表示
result = find_between_10_and_30(numbers)
print("10より大きく30未満の数値:", result)
</code></pre>

<details>

<summary>ヒント</summary>

* 線形探索では、リストの先頭から順番に各要素を調べます

- 条件を満たす数値を見つけたら、結果のリストに追加します

* 10より大きく30未満の条件は `10 < number < 30` のように指定します

- 最終的に条件を満たす数値のリストを返します

</details>

#### 問題 1-2: 名簿データからの探索（辞書の活用）

名簿から特定の条件に合う人を探すプログラムを作成してください。ここでは辞書型を使って名前と年齢の組み合わせを扱います。

```python
# 名前と年齢のリスト（辞書型を使用）
# 辞書とは、キーと値のペアでデータを管理する方法です
# 例えば {"name": "佐藤", "age": 22} は、"name"というキーに"佐藤"という値、
# "age"というキーに22という値が対応しています
people = [
    {"name": "佐藤", "age": 22},
    {"name": "鈴木", "age": 19},
    {"name": "高橋", "age": 25},
    {"name": "田中", "age": 18},
    {"name": "伊藤", "age": 31},
    {"name": "渡辺", "age": 40},
    {"name": "山本", "age": 33}
]

# 線形探索を使って、名前に「田」という文字が含まれる人をすべて探す関数を作成してください
def find_people_with_ta(people_list):
    """名前に「田」が含まれる人をすべて探す関数"""
    # ここに線形探索のコードを書いてください
    
    
# 関数を実行して結果を表示
result = find_people_with_ta(people)
print("名前に「田」が含まれる人:", result)
```

<details>

<summary>ヒント</summary>

* 辞書型では、キー名を使ってデータにアクセスします

- 各人物の名前は `person["name"]` でアクセスできます

* 辞書型はデータをより分かりやすく扱える利点があります

- 線形探索では、リストの先頭から順番に各要素を調べます

* 条件を満たす人を見つけたら、結果のリストに追加します

- 名前に特定の文字が含まれているかは `"田" in person["name"]` で確認できます

* 最終的に条件を満たす人のリストを返します

</details>

### 2. 二分探索の応用

#### 問題 2-1: 予算内の最適商品検索

ソート済みの商品リストから、二分探索を使って予算内で購入できる最も高価な商品を効率的に探すプログラムを作成してください。

```python
# ソート済みの商品データ（価格順）
sorted_products = [
    {"name": "マウス", "price": 3500},
    {"name": "キーボード", "price": 5000},
    {"name": "イヤホン", "price": 8000},
    {"name": "プリンター", "price": 15000},
    {"name": "モニター", "price": 25000},
    {"name": "タブレット", "price": 45000},
    {"name": "スマートフォン", "price": 65000},
    {"name": "ノートパソコン", "price": 85000}
]

# 二分探索を使って、予算内で買える最も高価な商品を探す関数を作成してください
def find_best_product_for_budget(products_list, budget):
    """
    予算内で買える最も高価な商品を見つける関数
    
    引数:
    products_list -- 検索対象の商品リスト（価格順にソート済み）
    budget -- 予算（この価格以下の商品を探す）
    
    戻り値:
    見つかった場合はその商品、見つからなかった場合はNone
    """
    # ここに二分探索を応用したコードを書いてください
    
    
# 関数を実行して結果を表示
print("予算内で最も高価な商品を探す:")
budgets = [10000, 30000, 50000, 2000]
for budget in budgets:
    product = find_best_product_for_budget(sorted_products, budget)
    if product:
        print(f"予算{budget:,}円以内で最適な商品: {product['name']} ({product['price']:,}円)")
    else:
        print(f"予算{budget:,}円以内では商品を購入できません")
```

<details>

<summary>ヒント</summary>

* 商品リストは価格順にソートされているため、二分探索を応用できます

- 「予算内で最も高価な商品」を探す問題は、「指定した価格以下で最も高い価格の商品」を探す問題に言い換えられます

* 二分探索の基本原理を応用して:
  * 探索範囲を設定（最初は全リスト）
  * 中央の商品をチェック
  * 中央の商品の価格が予算を超える場合は左半分を探索
  * 中央の商品の価格が予算以下の場合は、その商品を「候補」として記録し、より高価な商品を探すために右半分を探索

- 商品の価格は `product["price"]` でアクセスできます

* 探索範囲を管理するための left と right の変数を使います

- 探索範囲がなくなったら (left > right)、最後に記録された「候補」を返します

* 候補が見つからなかった場合は None を返します

</details>

#### 問題 2-2: 閾値に最も近い値の検索

ソート済みの数値リストから、指定された値に最も近い値を二分探索を使って効率的に探すプログラムを作成してください。

```python
# ソート済みの数値リスト
sorted_numbers = [2, 5, 8, 12, 16, 23, 38, 42, 56, 72, 91]

# 二分探索を使って、指定した値に最も近い値を探す関数を作成してください
def find_closest_value(numbers_list, target):
    """
    指定した値に最も近い値を見つける関数
    
    引数:
    numbers_list -- 検索対象の数値リスト（ソート済み）
    target -- 探している値
    
    戻り値:
    指定した値に最も近い値
    """
    # ここに二分探索を応用したコードを書いてください
    
    
# 関数を実行して結果を表示
values_to_find = [15, 39, 70, 1, 100]

for value in values_to_find:
    closest = find_closest_value(sorted_numbers, value)
    print(f"{value}に最も近い値は {closest} です")
```

<details>

<summary>ヒント</summary>

* 二分探索の基本原理を応用して、最も近い値を探すことができます

- 完全一致を探すだけでなく、最も近い値を追跡するよう工夫します

* 探索の最終段階では、ターゲットに近い可能性のある2つの値（左右の値）を比較して、より近い方を選びます

- 距離（差の絶対値）が小さい方が「より近い」値です

* 例: ターゲットが15の場合、12と16のどちらが近いかを比較し、|15-12|=3と|15-16|=1を計算して、差が小さい16を選びます

- 探索範囲を管理するための left と right の変数を使います

* 探索が終了した時点で、left と right は通常、探している値に近い2つの値を指しています

</details>

### 3. 選択ソートの応用

#### 問題 3-1: 学生ランキングシステム

学生のテストスコアをソートし、上位3名を表示するプログラムを作成してください。

```python
# 学生のテストスコアデータ
students = [
    {"name": "田中", "score": 85},
    {"name": "佐藤", "score": 72},
    {"name": "鈴木", "score": 91},
    {"name": "高橋", "score": 65},
    {"name": "伊藤", "score": 78},
    {"name": "渡辺", "score": 88},
    {"name": "山本", "score": 75}
]

# 選択ソートを使って学生をスコア順（高い順）にソートする関数を作成してください
def selection_sort_by_score(students_list):
    """選択ソートで学生をスコア順にソートする関数"""
    # ここに選択ソートのコードを書いてください
    
    
# 結果を表示する関数
def display_top_students(students_list, top_n=3):
    """上位n名の学生を表示する関数"""
    sorted_students = selection_sort_by_score(students_list.copy())
    
    print(f"===== 成績上位{top_n}名 =====")
    for i in range(min(top_n, len(sorted_students))):
        student = sorted_students[i]
        print(f"{i+1}位: {student['name']} - {student['score']}点")

# 関数を実行
display_top_students(students)
```

<details>

<summary>ヒント</summary>

* 選択ソートでは、各パスで残りの要素から「最大値」を見つけて適切な位置に配置します

- 成績順（高い順）にソートするには、各パスで最高得点の学生を見つけます

* スコアには `student["score"]` でアクセスします

- リストをコピーして処理し、元のリストを変更しないようにします

* 二重ループを使います：外側のループは配置位置、内側のループは最大値の検索に使用します

- 最終的にスコア順にソートされた学生のリストを返します

</details>

### 4. バブルソートの応用

#### 問題 4-1: 株価の変動分析

ある企業の株価データをバブルソートで並べ替え、値動きの大きい日を分析するプログラムを作成してください。

```python
# 株価データ（日付と価格）
stock_prices = [
    {"date": "2025/04/01", "price": 12500},
    {"date": "2025/04/02", "price": 12800},
    {"date": "2025/04/03", "price": 12400},
    {"date": "2025/04/04", "price": 12600},
    {"date": "2025/04/07", "price": 13000},
    {"date": "2025/04/08", "price": 12900},
    {"date": "2025/04/09", "price": 13200},
    {"date": "2025/04/10", "price": 13100},
    {"date": "2025/04/11", "price": 13500},
    {"date": "2025/04/14", "price": 13300}
]

# 日々の株価変動を計算する関数
def calculate_daily_changes(prices_data):
    """日々の株価変動を計算する関数"""
    daily_changes = []
    
    for i in range(1, len(prices_data)):
        prev_day = prices_data[i-1]
        current_day = prices_data[i]
        
        # 価格の変動と変動率を計算
        price_change = current_day["price"] - prev_day["price"]
        change_rate = (price_change / prev_day["price"]) * 100
        
        daily_changes.append({
            "date": current_day["date"],
            "prev_price": prev_day["price"],
            "current_price": current_day["price"],
            "change": price_change,
            "change_rate": change_rate
        })
    
    return daily_changes

# バブルソートを使って、価格変動の大きい順にソートする関数を作成してください
def bubble_sort_by_price_change(changes_list):
    """バブルソートで価格変動の大きい順にソートする関数"""
    # ここにバブルソートのコードを書いてください
    # 変動の絶対値が大きい順にソートしてください
    
    
# 株価変動を計算
daily_changes = calculate_daily_changes(stock_prices)

# 価格変動の大きい順にソート
sorted_changes = bubble_sort_by_price_change(daily_changes.copy())

# 結果を表示
print("===== 価格変動の大きい順 =====")
for change_data in sorted_changes:
    print(f"{change_data['date']}: {change_data['change_rate']:.2f}% ({change_data['change']}円)")
```

<details>

<summary>ヒント</summary>

* バブルソートでは、隣接する要素を比較して必要に応じて交換します

- 価格変動の絶対値で比較するには、`abs(change_data["change"])` や `abs(change_data["change_rate"])` を使います

* リストをコピーして処理し、元のリストを変更しないようにします

- 二重ループを使います：外側のループはパスの回数、内側のループは隣接要素の比較に使用します

* 最適化として、1回のパスで交換が発生しなかった場合は早期終了できます

- 最終的に変動の大きい順にソートされたリストを返します

</details>

### 5. 挿入ソートの応用

#### 問題 5-1: 図書館の本の並べ替え

図書館の本のデータを著者名の五十音順に並べ替えるプログラムを作成してください。

```python
# 図書データ
books = [
    {"title": "Python入門", "author": "山田太郎", "year": 2018},
    {"title": "データ分析の基礎", "author": "佐藤花子", "year": 2020},
    {"title": "アルゴリズムとデータ構造", "author": "鈴木一郎", "year": 2015},
    {"title": "Webアプリケーション開発", "author": "高橋次郎", "year": 2019},
    {"title": "人工知能入門", "author": "田中三郎", "year": 2021},
    {"title": "クラウドコンピューティング", "author": "伊藤四郎", "year": 2017},
    {"title": "情報セキュリティ", "author": "中村五郎", "year": 2016}
]

# 挿入ソートを使って本を著者名順にソートする関数を作成してください
def insertion_sort_by_author(books_list):
    """挿入ソートで本を著者名順にソートする関数"""
    # ここに挿入ソートのコードを書いてください
    
    
# 結果を表示する関数
def display_books(books_list, title):
    """本のリストを表示する関数"""
    print(f"\n===== {title} =====")
    print("タイトル\t\t\t著者\t\t出版年")
    print("----------------------------------------------------")
    for book in books_list:
        print(f"{book['title']:<20}\t{book['author']:<10}\t{book['year']}")

# 関数を実行
display_books(books, "元のリスト")

sorted_books = insertion_sort_by_author(books.copy())
display_books(sorted_books, "著者名順")
```

<details>

<summary>ヒント</summary>

* 挿入ソートでは、未ソート部分の要素を1つずつ取り出し、ソート済み部分の適切な位置に挿入します

- 著者名には `book["author"]` でアクセスします

* 文字列（著者名）の比較は、通常の比較演算子（<, >）で行えます

- リストをコピーして処理し、元のリストを変更しないようにします

* 外側のループは、未ソート部分の先頭の要素を選びます（通常は2番目の要素から開始）

- 内側のループは、選んだ要素をソート済み部分の適切な位置に挿入するために、ソート済み部分を右から左へ走査します

* 最終的に著者名順にソートされた本のリストを返します

</details>

### 6. 探索とソートの組み合わせ

#### 問題 6-1: 成績分析システム

学生の成績データをソートし、特定の条件に該当する学生を探すプログラムを作成してください。

```python
# 学生の成績データ
students = [
    {"id": "S001", "name": "田中", "math": 85, "english": 72, "science": 90},
    {"id": "S002", "name": "佐藤", "math": 65, "english": 80, "science": 75},
    {"id": "S003", "name": "鈴木", "math": 90, "english": 85, "science": 80},
    {"id": "S004", "name": "高橋", "math": 75, "english": 60, "science": 65},
    {"id": "S005", "name": "伊藤", "math": 80, "english": 75, "science": 85},
    {"id": "S006", "name": "渡辺", "math": 60, "english": 65, "science": 70},
    {"id": "S007", "name": "山本", "math": 95, "english": 80, "science": 90}
]

# 各学生の平均点を計算して追加する関数
def calculate_average_scores(students_list):
    """各学生の平均点を計算して追加する関数"""
    for student in students_list:
        avg = (student["math"] + student["english"] + student["science"]) / 3
        student["average"] = round(avg, 1)  # 小数点第1位まで四捨五入
    return students_list

# 選択ソートを使って平均点順に学生をソートする関数
def selection_sort_by_average(students_list):
    """選択ソートで平均点順（高い順）にソートする関数"""
    # ここに選択ソートのコードを書いてください
    
    
# 線形探索を使って、特定の条件を満たす学生を探す関数
def find_students_by_condition(students_list, condition_func):
    """条件を満たす学生を探す関数"""
    result = []
    for student in students_list:
        if condition_func(student):
            result.append(student)
    return result

# 学生リストを表示する関数
def display_students(students_list, title):
    """学生リストを表示する関数"""
    print(f"\n===== {title} =====")
    print("ID\t名前\t数学\t英語\t理科\t平均")
    print("-------------------------------------------")
    for student in students_list:
        print(f"{student['id']}\t{student['name']}\t{student['math']}\t{student['english']}\t{student['science']}\t{student['average']}")

# 成績分析を実行
# 1. 平均点を計算
students_with_avg = calculate_average_scores(students.copy())

# 2. 平均点順にソート
sorted_students = selection_sort_by_average(students_with_avg)
display_students(sorted_students, "成績順")

# 3. 数学が80点以上の学生を探す
math_stars = find_students_by_condition(
    sorted_students,
    lambda student: student["math"] >= 80
)
display_students(math_stars, "数学が80点以上の学生")

# 4. 全科目が70点以上の学生を探す
good_students = find_students_by_condition(
    sorted_students,
    lambda student: student["math"] >= 70 and student["english"] >= 70 and student["science"] >= 70
)
display_students(good_students, "全科目が70点以上の学生")
```

<details>

<summary>ヒント</summary>

* 選択ソートでは、各パスで残りの要素から最大値を見つけて適切な位置に配置します

- 平均点順（高い順）にソートするには、各パスで最高平均点の学生を見つけます

* 平均点には `student["average"]` でアクセスします

- リストをコピーして処理し、元のリストを変更しないようにします

* 二重ループを使います：外側のループは配置位置、内側のループは最大値の検索に使用します

- `find_students_by_condition` 関数は既に完成していて、特定の条件を満たす学生を探します

* `lambda` 式は特殊な形の関数で、ここでは学生データを評価する条件を表現するために使われています

- 最終的に平均点順にソートされた学生のリストを返します

</details>
