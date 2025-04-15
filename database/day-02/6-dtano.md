---
description: >-
  この章では、データベース内のデータを集計して分析するための機能について学びます。集計関数とグループ化を使うことで、データの傾向や集計値を効率的に得ることができます。
---

# 6. データの集計

### 6.1 集計関数

集計関数は、複数の行から計算された単一の値を返す関数です。主な集計関数は以下の通りです：

| 関数      | 説明         |
| ------- | ---------- |
| COUNT() | 行数を数える     |
| SUM()   | 数値の合計を計算する |
| AVG()   | 数値の平均を計算する |
| MAX()   | 最大値を見つける   |
| MIN()   | 最小値を見つける   |

#### 6.1.1 COUNT関数

`COUNT()`関数は、指定した条件に一致する行の数をカウントします。

```sql
-- テーブル内の全レコード数を取得
SELECT COUNT(*) AS 商品数 FROM products;
```

実行結果:

```
+--------+
| 商品数 |
+--------+
|      7 |
+--------+
1 row in set (0.00 sec)
```

特定のカラムを指定した場合、NULL値ではない行のみをカウントします：

```sql
-- description列がNULLでない行の数を取得
SELECT COUNT(description) AS 説明あり FROM products;
```

実行結果:

```
+----------+
| 説明あり |
+----------+
|        5 |
+----------+
1 row in set (0.00 sec)
```

特定の条件に一致する行の数をカウント：

```sql
-- 在庫がある商品の数を取得
SELECT COUNT(*) AS 在庫あり商品数
FROM products
WHERE stock > 0;
```

実行結果:

```
+----------------+
| 在庫あり商品数 |
+----------------+
|              6 |
+----------------+
1 row in set (0.00 sec)
```

#### 6.1.2 SUM関数

`SUM()`関数は、指定したカラムの値の合計を計算します。

```sql
-- 商品の在庫数の合計
SELECT SUM(stock) AS 総在庫数 FROM products;
```

実行結果:

```
+----------+
| 総在庫数 |
+----------+
|      145 |
+----------+
1 row in set (0.00 sec)
```

条件と組み合わせて使用：

```sql
-- カテゴリID=1の商品の在庫数合計
SELECT 
    SUM(stock) AS カテゴリ1総在庫数
FROM products
WHERE category_id = 1;
```

実行結果:

```
+--------------------+
| カテゴリ1総在庫数 |
+--------------------+
|                 90 |
+--------------------+
1 row in set (0.00 sec)
```

計算値の合計：

```sql
-- 商品の総資産価値（価格×在庫数の合計）
SELECT 
    SUM(price * stock) AS 総資産価値
FROM products;
```

実行結果:

```
+----------------+
| 総資産価値     |
+----------------+
|     4655000.00 |
+----------------+
1 row in set (0.00 sec)
```

#### 6.1.3 AVG関数

`AVG()`関数は、指定したカラムの値の平均を計算します。

```sql
-- 商品の平均価格
SELECT AVG(price) AS 平均価格 FROM products;
```

実行結果:

```
+------------+
| 平均価格   |
+------------+
| 35142.8571 |
+------------+
1 row in set (0.00 sec)
```

より見やすく小数点以下を丸める：

```sql
-- 小数点以下を2桁に丸めた平均価格
SELECT ROUND(AVG(price), 2) AS 平均価格 FROM products;
```

実行結果:

```
+------------+
| 平均価格   |
+------------+
|   35142.86 |
+------------+
1 row in set (0.00 sec)
```

条件と組み合わせて使用：

```sql
-- 価格が10,000円以上の商品の平均価格
SELECT 
    ROUND(AVG(price), 0) AS 高価格帯平均
FROM products
WHERE price >= 10000;
```

実行結果:

```
+---------------+
| 高価格帯平均  |
+---------------+
|        58000  |
+---------------+
1 row in set (0.00 sec)
```

#### 6.1.4 MAX関数とMIN関数

`MAX()`と`MIN()`関数は、それぞれ指定したカラムの最大値と最小値を取得します。

```sql
-- 最も高い商品価格と最も安い商品価格
SELECT 
    MAX(price) AS 最高価格,
    MIN(price) AS 最低価格
FROM products;
```

実行結果:

```
+------------+------------+
| 最高価格   | 最低価格   |
+------------+------------+
|  120000.00 |    2000.00 |
+------------+------------+
1 row in set (0.00 sec)
```

テキストデータに対しても使用可能（アルファベット順で比較）：

```sql
-- 商品名の最初と最後（アルファベット順）
SELECT 
    MIN(name) AS 最初の商品名,
    MAX(name) AS 最後の商品名
FROM products;
```

実行結果:

```
+--------------------+------------------+
| 最初の商品名       | 最後の商品名     |
+--------------------+------------------+
| キーボード         | ワイヤレスイヤホン |
+--------------------+------------------+
1 row in set (0.00 sec)
```

#### 6.1.5 複数の集計関数の組み合わせ

複数の集計関数を1つのクエリーで使用することもできます：

```sql
-- 商品の統計情報
SELECT 
    COUNT(*) AS 商品数,
    ROUND(AVG(price), 0) AS 平均価格,
    MIN(price) AS 最低価格,
    MAX(price) AS 最高価格,
    SUM(stock) AS 総在庫数
FROM products;
```

実行結果:

```
+--------+------------+------------+------------+----------+
| 商品数 | 平均価格   | 最低価格   | 最高価格   | 総在庫数 |
+--------+------------+------------+------------+----------+
|      7 |     35143  |    2000.00 |  120000.00 |      145 |
+--------+------------+------------+------------+----------+
1 row in set (0.00 sec)
```

### 6.2 GROUP BY句によるグループ化

「カテゴリごとの商品数」「都市別の売上合計」など、データをグループ化して集計したい場合に`GROUP BY`句を使います。

`GROUP BY`句を使用すると、指定したカラムの値が同じ行をグループ化し、各グループに対して集計関数を適用できます。

#### 6.2.1 基本的なグループ化

```sql
-- カテゴリごとの商品数
SELECT 
    category_id,
    COUNT(*) AS 商品数
FROM products
GROUP BY category_id;
```

実行結果:

```
+-------------+--------+
| category_id | 商品数 |
+-------------+--------+
|           1 |      4 |
|           2 |      2 |
|           3 |      1 |
+-------------+--------+
3 rows in set (0.00 sec)
```

カテゴリ名も表示する（複数テーブルの結合）：

```sql
-- カテゴリ名ごとの商品数
SELECT 
    c.name AS カテゴリ,
    COUNT(*) AS 商品数
FROM 
    products p,
    categories c
WHERE 
    p.category_id = c.id
GROUP BY 
    c.name;
```

実行結果:

```
+----------+--------+
| カテゴリ | 商品数 |
+----------+--------+
| 電子機器 |      4 |
| 家具     |      2 |
| 書籍     |      1 |
+----------+--------+
3 rows in set (0.00 sec)
```

#### 6.2.2 集計関数を組み合わせたグループ化

GROUP BY句と複数の集計関数を組み合わせることができます：

```sql
-- カテゴリごとの統計情報
SELECT 
    category_id,
    COUNT(*) AS 商品数,
    ROUND(AVG(price), 0) AS 平均価格,
    MIN(price) AS 最低価格,
    MAX(price) AS 最高価格,
    SUM(stock) AS 総在庫数
FROM products
GROUP BY 
    category_id;
```

実行結果:

```
+-------------+--------+------------+------------+------------+----------+
| category_id | 商品数 | 平均価格   | 最低価格   | 最高価格   | 総在庫数 |
+-------------+--------+------------+------------+------------+----------+
|           1 |      4 |     54500  |    2000.00 |  120000.00 |       90 |
|           2 |      2 |     23500  |   12000.00 |   35000.00 |       25 |
|           3 |      1 |      3500  |    3500.00 |    3500.00 |       30 |
+-------------+--------+------------+------------+------------+----------+
3 rows in set (0.00 sec)
```

### 6.3 HAVINGによるグループのフィルタリング

`WHERE`句がグループ化の前に行を絞り込むのに対し、`HAVING`句はグループ化した後の結果を絞り込みます。

#### 6.3.1 基本的なHAVINGの使用

```sql
-- 商品が2つ以上あるカテゴリを抽出
SELECT 
    category_id,
    COUNT(*) AS 商品数
FROM products
GROUP BY category_id
HAVING COUNT(*) >= 2;
```

実行結果:

```
+-------------+--------+
| category_id | 商品数 |
+-------------+--------+
|           1 |      4 |
|           2 |      2 |
+-------------+--------+
2 rows in set (0.00 sec)
```

#### 6.3.2 WHERE句とHAVING句の違い

`WHERE`句と`HAVING`句は一緒に使うこともできます：

* `WHERE`句: 集計前の行を絞り込む（個々のレコードに対する条件）
* `HAVING`句: 集計後のグループを絞り込む（グループに対する条件）

```sql
-- 価格が5,000円以上の商品だけを対象に、平均価格が50,000円以上のカテゴリを抽出
SELECT 
    category_id,
    ROUND(AVG(price), 0) AS 平均価格,
    COUNT(*) AS 商品数
FROM products
WHERE 
    price >= 5000  -- 個々の商品に対する条件（集計前）
GROUP BY 
    category_id
HAVING 
    AVG(price) >= 50000;  -- グループに対する条件（集計後）
```

実行結果:

```
+-------------+------------+--------+
| category_id | 平均価格   | 商品数 |
+-------------+------------+--------+
|           1 |     71667  |      3 |
+-------------+------------+--------+
1 row in set (0.00 sec)
```

***

### 演習問題

#### 問題 6-1: 基本的な集計

商品テーブルに対して以下の集計クエリーを作成してください：

1. 商品の総数を取得する
2. 在庫がある商品（stock > 0）の数を取得する
3. 最も高い商品価格と最も安い商品価格を求める
4. すべての商品の在庫数の合計を求める

**ヒント:**

* 集計関数 `COUNT()`, `MAX()`, `MIN()`, `SUM()` を使用します
* 条件で絞り込む場合は `WHERE` 句を使います

\<details> \<summary>解答\</summary>

```sql
-- 1. 商品の総数
SELECT COUNT(*) AS 商品総数 FROM products;

-- 2. 在庫がある商品数
SELECT COUNT(*) AS 在庫あり商品数
FROM products
WHERE stock > 0;

-- 3. 最高価格と最低価格
SELECT 
    MAX(price) AS 最高価格,
    MIN(price) AS 最低価格
FROM products;

-- 4. 総在庫数
SELECT SUM(stock) AS 総在庫数 FROM products;
```

\</details>

#### 問題 6-2: グループ別の集計

以下のクエリーを作成してください：

1. カテゴリごとの商品数を求める
2. カテゴリごとの平均価格を求め、平均価格の高い順に表示する
3. カテゴリごとの総在庫数を求める

**ヒント:**

* `GROUP BY` 句でカテゴリごとにグループ化します
* 結果を並べ替えるには `ORDER BY` 句を使います
* 平均値は `AVG()` 関数で求めます

\<details> \<summary>解答\</summary>

```sql
-- 1. カテゴリごとの商品数
SELECT 
    category_id,
    COUNT(*) AS 商品数
FROM products
GROUP BY category_id;

-- 2. カテゴリごとの平均価格（高い順）
SELECT 
    category_id,
    ROUND(AVG(price), 0) AS 平均価格
FROM products
GROUP BY category_id
ORDER BY 平均価格 DESC;

-- 3. カテゴリごとの総在庫数
SELECT 
    category_id,
    SUM(stock) AS 総在庫数
FROM products
GROUP BY category_id;
```

\</details>

#### 問題 6-3: HAVINGによるフィルタリング

以下のクエリーを作成してください：

1. 商品数が2つ以上あるカテゴリを抽出する
2. 平均価格が20,000円以上のカテゴリを抽出する
3. 総在庫数が30個以上のカテゴリを、総在庫数の多い順に表示する

**ヒント:**

* グループ化後の結果をフィルタリングするには `HAVING` 句を使います
* 集計関数 (`COUNT()`, `AVG()`, `SUM()`) を条件に使用できます

\<details> \<summary>解答\</summary>

```sql
-- 1. 商品数が2つ以上のカテゴリ
SELECT 
    category_id,
    COUNT(*) AS 商品数
FROM products
GROUP BY category_id
HAVING COUNT(*) >= 2;

-- 2. 平均価格が20,000円以上のカテゴリ
SELECT 
    category_id,
    ROUND(AVG(price), 0) AS 平均価格
FROM products
GROUP BY category_id
HAVING AVG(price) >= 20000;

-- 3. 総在庫数が30個以上のカテゴリ（多い順）
SELECT 
    category_id,
    SUM(stock) AS 総在庫数
FROM products
GROUP BY category_id
HAVING SUM(stock) >= 30
ORDER BY 総在庫数 DESC;
```

\</details>

#### 問題 6-4: 組み合わせた集計とフィルタリング

以下のクエリーを作成してください：

1. 価格が10,000円以上の商品のみを対象に、カテゴリごとの商品数と平均価格を求める
2. 在庫がある商品（stock > 0）のみを対象に、カテゴリごとの最低価格と最高価格を求める
3. 各カテゴリの「在庫金額」（価格×在庫数の合計）を計算し、10万円以上のカテゴリのみ表示する

**ヒント:**

* 集計前のフィルタリングには `WHERE` 句、集計後のフィルタリングには `HAVING` 句を使います
* 計算値に対する集計も可能です（例: `SUM(price * stock)`）

\<details> \<summary>解答\</summary>

```sql
-- 1. 価格10,000円以上の商品のカテゴリ別集計
SELECT 
    category_id,
    COUNT(*) AS 商品数,
    ROUND(AVG(price), 0) AS 平均価格
FROM products
WHERE price >= 10000
GROUP BY category_id;

-- 2. 在庫がある商品のカテゴリ別価格範囲
SELECT 
    category_id,
    MIN(price) AS 最低価格,
    MAX(price) AS 最高価格
FROM products
WHERE stock > 0
GROUP BY category_id;

-- 3. カテゴリ別在庫金額（10万円以上）
SELECT 
    category_id,
    SUM(price * stock) AS 在庫金額
FROM products
GROUP BY category_id
HAVING SUM(price * stock) >= 100000
ORDER BY 在庫金額 DESC;
```

\</details>
