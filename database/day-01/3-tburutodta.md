---
description: >-
  この章では、データベースの作成からテーブルの設計・作成、そしてデータの追加までの一連の操作を学びます。これらはデータ検索（SELECT）の前に必要な準備段階です。実際に手を動かしながら、データベース操作の基本を身につけましょう。
---

# 3. テーブル作成とデータ操作

### 3.1 データベースの作成と選択

#### データベースとは

データベースは、テーブルを格納するための「箱」のようなものです。一般的には1つのアプリケーションに対して1つのデータベースを使います。

#### データベースの作成

新しいデータベースを作成するには、`CREATE DATABASE` コマンドを使います。

```sql
-- 新しいデータベースを作成
CREATE DATABASE online_shop;
```

実行結果:

```
Query OK, 1 row affected (0.02 sec)
```

同名のデータベースが既に存在する場合にエラーを回避するには、`IF NOT EXISTS`を追加します：

```sql
-- 既に存在する場合はエラーを出さない
CREATE DATABASE IF NOT EXISTS online_shop;
```

#### データベースの一覧表示

サーバー上のデータベース一覧を表示するには：

```sql
SHOW DATABASES;
```

実行結果:

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| online_shop        |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
```

#### データベースの選択

作成したデータベースを使用するには、`USE` コマンドで選択します：

```sql
-- データベースを選択
USE online_shop;
```

実行結果:

```
Database changed
```

現在選択しているデータベースを確認するには：

```sql
SELECT DATABASE();
```

実行結果:

```
+-------------+
| DATABASE()  |
+-------------+
| online_shop |
+-------------+
1 row in set (0.00 sec)
```

### 3.2 テーブルの設計と作成

#### テーブルとは

テーブルは行と列で構成される表のような構造で、データベース内のデータを整理して格納します。

#### テーブル設計のポイント

テーブルを作成する前に、基本的な設計を考えましょう：

1. **どんな情報を格納するか**：顧客情報なら名前、メール、住所など
2. **各項目の適切なデータ型は何か**：テキスト、数値、日付など
3. **主キーは何か**：各行を一意に識別する項目（例：ID）
4. **他のテーブルとの関連はあるか**：外部キーの必要性

#### 基本的なデータ型

MySQLの主要なデータ型は以下の通りです：

| データ型            | 説明              | 例                     |
| --------------- | --------------- | --------------------- |
| INT             | 整数値             | `id INT`              |
| VARCHAR(n)      | 可変長文字列（最大n文字）   | `name VARCHAR(50)`    |
| DECIMAL(m,n)    | 小数点を持つ数値        | `price DECIMAL(10,2)` |
| DATE            | 日付 (YYYY-MM-DD) | `birth_date DATE`     |
| TEXT            | 長いテキスト          | `description TEXT`    |
| BOOLEAN/TINYINT | 真偽値             | `active BOOLEAN`      |

#### テーブルの作成

テーブルを作成するには、`CREATE TABLE` コマンドを使います：

```sql
-- 商品カテゴリのテーブルを作成
CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    description VARCHAR(200)
);
```

実行結果:

```
Query OK, 0 rows affected (0.03 sec)
```

```sql
-- 商品テーブルを作成（カテゴリテーブルとの関連あり）
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    stock INT NOT NULL DEFAULT 0,
    category_id INT,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

実行結果:

```
Query OK, 0 rows affected (0.04 sec)
```

> 📝 **ポイント**: 上記の例では、`products`テーブルの`category_id`列が`categories`テーブルの`id`列を参照しています。これにより、各商品は特定のカテゴリに属することができます。

#### 主な制約

テーブル作成時に以下の制約を設定できます：

* **NOT NULL**：NULL値を許可しない
* **DEFAULT 値**：デフォルト値を設定
* **PRIMARY KEY**：主キー（一意の識別子）
* **AUTO\_INCREMENT**：自動的に増加する値（主に主キーで使用）
* **FOREIGN KEY**：他のテーブルへの参照
* **UNIQUE**：値の重複を許可しない

#### テーブル一覧と構造の確認

データベース内のテーブル一覧を表示するには：

```sql
SHOW TABLES;
```

実行結果:

```
+----------------------+
| Tables_in_online_shop|
+----------------------+
| categories           |
| products             |
+----------------------+
2 rows in set (0.00 sec)
```

テーブルの構造を確認するには：

```sql
DESCRIBE products;
-- または
DESC products;
```

実行結果:

```
+-------------+--------------+------+-----+-------------------+-------------------+
| Field       | Type         | Null | Key | Default           | Extra             |
+-------------+--------------+------+-----+-------------------+-------------------+
| id          | int          | NO   | PRI | NULL              | auto_increment    |
| name        | varchar(100) | NO   |     | NULL              |                   |
| price       | decimal(10,2)| NO   |     | NULL              |                   |
| stock       | int          | NO   |     | 0                 |                   |
| category_id | int          | YES  | MUL | NULL              |                   |
| description | text         | YES  |     | NULL              |                   |
| created_at  | timestamp    | YES  |     | CURRENT_TIMESTAMP |                   |
+-------------+--------------+------+-----+-------------------+-------------------+
7 rows in set (0.01 sec)
```

### 3.3 データの追加（INSERT）

#### INSERT文の基本構文

テーブルにデータを追加するには、`INSERT INTO` コマンドを使います：

```sql
INSERT INTO テーブル名 (カラム1, カラム2, ...)
VALUES (値1, 値2, ...);
```

#### 単一レコードの挿入

まずはカテゴリテーブルにデータを入れてみましょう：

```sql
-- カテゴリを追加
INSERT INTO categories (name, description) 
VALUES ('電子機器', 'スマートフォン、パソコン、その他の電子製品');
```

実行結果:

```
Query OK, 1 row affected (0.01 sec)
```

続いて別のカテゴリも追加します：

```sql
INSERT INTO categories (name, description)
VALUES ('家具', 'テーブル、椅子、ソファなど');
```

実行結果:

```
Query OK, 1 row affected (0.01 sec)
```

次に商品テーブルにデータを追加します：

```sql
-- 商品を追加（category_idは既に存在するカテゴリのIDを指定）
INSERT INTO products (name, price, stock, category_id, description)
VALUES ('スマートフォン', 89000, 25, 1, '最新モデルのスマートフォン');
```

実行結果:

```
Query OK, 1 row affected (0.01 sec)
```

#### 複数レコードの一括挿入

複数のレコードを一度に挿入することもできます：

```sql
-- 複数の商品を一括挿入
INSERT INTO products (name, price, stock, category_id, description)
VALUES 
('ラップトップPC', 120000, 15, 1, '高性能ノートパソコン'),
('木製テーブル', 35000, 5, 2, '4人用ダイニングテーブル'),
('オフィスチェア', 12000, 20, 2, '快適な作業用椅子');
```

実行結果:

```
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0
```

#### 特殊なカラム値の扱い

**AUTO\_INCREMENT列**

`id`のような`AUTO_INCREMENT`設定されたカラムは、値を指定しなくても自動的に番号が振られます：

```sql
-- idは自動的に割り当てられる
INSERT INTO categories (name)
VALUES ('書籍');
```

**デフォルト値の使用**

`DEFAULT`キーワードを使うとデフォルト値を明示的に指定できます：

```sql
-- stockはデフォルト値(0)が使われる
INSERT INTO products (name, price, category_id, stock)
VALUES ('ワイヤレスイヤホン', 15000, 1, DEFAULT);
```

**NULL値の挿入**

NULL許容の列には、明示的に`NULL`を指定できます：

```sql
-- descriptionにNULLを指定
INSERT INTO products (name, price, stock, category_id, description)
VALUES ('USBメモリ', 3000, 50, 1, NULL);
```

#### 挿入したデータの確認

データを挿入したら、正しく追加されたか確認するのが良い習慣です：

```sql
-- カテゴリを確認
SELECT * FROM categories;
```

実行結果:

```
+----+----------+-------------------------------------+
| id | name     | description                         |
+----+----------+-------------------------------------+
|  1 | 電子機器 | スマートフォン、パソコン...        |
|  2 | 家具     | テーブル、椅子、ソファなど         |
|  3 | 書籍     | NULL                                |
+----+----------+-------------------------------------+
3 rows in set (0.00 sec)
```

```sql
-- 商品を確認
SELECT * FROM products;
```

実行結果:

```
+----+--------------------+--------+-------+-------------+---------------------+---------------------+
| id | name               | price  | stock | category_id | description         | created_at          |
+----+--------------------+--------+-------+-------------+---------------------+---------------------+
|  1 | スマートフォン       | 89000  |    25 |           1 | 最新モデルの...     | 2025-04-17 09:30:15 |
|  2 | ラップトップPC      | 120000 |    15 |           1 | 高性能ノート...     | 2025-04-17 09:31:22 |
|  3 | 木製テーブル        | 35000  |     5 |           2 | 4人用ダイニング...  | 2025-04-17 09:31:22 |
|  4 | オフィスチェア      | 12000  |    20 |           2 | 快適な作業用椅子    | 2025-04-17 09:31:22 |
|  5 | ワイヤレスイヤホン   | 15000  |     0 |           1 | NULL                | 2025-04-17 09:32:40 |
|  6 | USBメモリ          |  3000  |    50 |           1 | NULL                | 2025-04-17 09:33:15 |
+----+--------------------+--------+-------+-------------+---------------------+---------------------+
6 rows in set (0.00 sec)
```

***

### 演習問題

#### 問題 3-1: データベースとテーブルの作成

1. `book_store` という名前のデータベースを作成し、使用するように設定してください。
2. 以下の列を持つ `authors` テーブルを作成してください。
   * `id`: 整数型、主キー、自動採番
   * `name`: 文字列型(50文字まで)、NOT NULL制約あり
   * `email`: 文字列型(100文字まで)、UNIQUE制約あり
   * `biography`: 長いテキスト型
3. 以下の列を持つ `books` テーブルを作成してください。
   * `id`: 整数型、主キー、自動採番
   * `title`: 文字列型(100文字まで)、NOT NULL制約あり
   * `author_id`: 整数型、`authors` テーブルの `id` を参照する外部キー
   * `price`: 小数型(10桁、2桁小数点)、NOT NULL制約あり
   * `publication_date`: 日付型
   * `stock`: 整数型、デフォルト値は0

\<details> \<summary>解答例\</summary>

```sql
-- 1. データベースの作成
CREATE DATABASE book_store;

-- データベースの選択
USE book_store;

-- 2. 著者テーブルの作成
CREATE TABLE authors (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    biography TEXT
);

-- 3. 書籍テーブルの作成
CREATE TABLE books (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    author_id INT,
    price DECIMAL(10, 2) NOT NULL,
    publication_date DATE,
    stock INT DEFAULT 0,
    FOREIGN KEY (author_id) REFERENCES authors(id)
);
```

\</details>

#### 問題 3-2: データの追加

1. `authors` テーブルに少なくとも3名の著者データを追加してください。
2. `books` テーブルに少なくとも5冊の書籍データを追加してください。各著者につき最低1冊は書籍を追加してください。

\<details> \<summary>解答例\</summary>

```sql
-- 著者データの追加
INSERT INTO authors (name, email, biography)
VALUES 
('山田太郎', 'yamada@example.com', '日本のベストセラー作家。SF小説を多く執筆。'),
('佐藤花子', 'sato@example.com', 'ミステリー小説の第一人者。推理作家賞を受賞。'),
('鈴木一郎', 'suzuki@example.com', 'プログラミング関連の技術書を多数出版。');

-- 書籍データの追加
INSERT INTO books (title, author_id, price, publication_date, stock)
VALUES 
('宇宙への旅', 1, 1500, '2023-05-15', 25),
('銀河の果て', 1, 1800, '2024-02-10', 15),
('完全犯罪', 2, 1200, '2022-10-21', 30),
('謎の暗号', 2, 1300, '2023-08-05', 20),
('Python入門', 3, 2800, '2024-01-20', 50),
('Webアプリケーション構築', 3, 3200, '2024-03-15', 35);
```

\</details>

#### 問題 3-3: データの確認

1. 著者テーブルのすべてのデータを表示してください。
2. 書籍テーブルのすべてのデータを表示してください。
3. 著者IDが1の著者が書いた書籍の情報（タイトル、価格、在庫数）を表示してください。

\<details> \<summary>解答例\</summary>

```sql
-- 1. 著者テーブルのすべてのデータを表示
SELECT * FROM authors;

-- 2. 書籍テーブルのすべてのデータを表示
SELECT * FROM books;

-- 3. 著者IDが1の著者が書いた書籍の情報
SELECT title, price, stock 
FROM books 
WHERE author_id = 1;
```

\</details>
