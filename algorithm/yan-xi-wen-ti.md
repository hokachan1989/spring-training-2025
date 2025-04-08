# 演習問題

### 解答例

#### 問題1-1: 変数への代入

```python
# 変数に自分の名前を代入してください
name = "山田太郎"

# 変数に自分の年齢を代入してください
age = 25

# 変数の内容を表示してください
print(name)
print(age)
```

#### 問題1-2: 変数の値の更新

```python
# 変数を宣言します
count = 5
print("最初の値:", count)

# 変数の値を10に更新してください
count = 10

# 更新された値を表示します
print("更新後の値:", count)
```

#### 問題1-3: 文字列の連結

```python
# 2つの文字列変数を宣言します
first_name = "太郎"
last_name = "山田"

# 2つの文字列を連結して、フルネームを作成してください
full_name = last_name + " " + first_name

# 結果を表示します
print(full_name)
```

#### 問題2-1: データ型の確認

```python
a = 42
b = 3.14
c = "Hello"
d = True

# 各変数の値と型を表示する
print("変数a:", a, "型:", type(a))
print("変数b:", b, "型:", type(b))
print("変数c:", c, "型:", type(c))
print("変数d:", d, "型:", type(d))
```

#### 問題2-2: 型変換の基本

```python
# 文字列として表現された数値
number_str = "25"
decimal_str = "7.5"

# 1. 整数に変換
number_int = int(number_str)

# 2. 浮動小数点数（小数）に変換
decimal_float = float(decimal_str)

# 3. 変換した値を使って計算（例: 合計）
total = number_int + decimal_float

# 4. 結果を表示
print(f"{number_str}と{decimal_str}の合計は{total}です。")
```
