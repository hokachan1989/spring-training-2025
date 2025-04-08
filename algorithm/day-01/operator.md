# Python: 基本演算子

### 4.1 算術演算子

算術演算子は、数値計算を行うための基本的な演算子です。

#### 基本的な算術演算子

```python
a = 10
b = 3

# 加算
print(a + b)   # 13

# 減算
print(a - b)   # 7

# 乗算
print(a * b)   # 30

# 除算
print(a / b)   # 3.3333... (浮動小数点)
```

```python
# 整数除算
print(a // b)  # 3 (小数点以下切り捨て)

# 剰余 (余り)
print(a % b)   # 1

# べき乗
print(a ** b)  # 1000 (10 の 3乗)
```

> 📝 **ポイント**: 除算 ( `/` ) の結果は常に浮動小数点数になります。整数部分だけが必要な場合は整数除算 ( `//` ) を使用します。

#### 演算子の優先順位

数学と同様に、Python でも演算子には優先順位があります。

1. 括弧 `()`
2. べき乗 `**`
3. 単項演算子 (正符号 `+x`, 負符号 `-x` )
4. 乗算 `*`, 除算 `/`, 整数除算 `//`, 剰余 `%`&#x20;
5. 加算 `+`, 減算 `-`&#x20;

```python
# 演算子の優先順位を示す例
result = 2 + 3 * 4
print(result)  # 14 (3 * 4 が先に計算される)

# 括弧を使用して優先順位を変更
result = (2 + 3) * 4
print(result)  # 20 (括弧内の 2 + 3 が先に計算される)
```

> 📝 **ポイント**: 計算の順序が不明確な場合や、特定の計算を優先したい場合は、括弧 `()` を使用して明示的に順序を指定することをお勧めします。これによりコードの読みやすさも向上します。

### 4.2 変数の増分と自己代入

プログラミングでは、変数の値を元の値に基づいて更新することが頻繁に行われます。

#### 変数の増分操作

```python
# 変数の増分操作
count = 0       # 初期値は0

count = count + 1  # 自分の値に 1 を加えて再代入
print(count)    # 出力: 1

count = count + 1  # もう一度増やす
print(count)    # 出力: 2
```

#### 合計の計算

```python
# 合計の計算
total = 0

total = total + 10  # totalに10を加える
print(total)    # 出力: 10

total = total + 5   # さらに5を加える
print(total)    # 出力: 15
```

### 4.3 比較演算子

比較演算子は 2つの値を比較し、結果としてブール値 ( `True` または `False` ) を返します。

```python
x = 5
y = 10

# 等しい
print(x == y)  # False

# 等しくない
print(x != y)  # True

# より大きい
print(x > y)   # False

# より小さい
print(x < y)   # True

# 以上
print(x >= y)  # False

# 以下
print(x <= y)  # True
```

> 📝 **ポイント**: 代入演算子 ( `=` ) と等価比較演算子 ( `==` ) を混同しないように注意してください。`x = 5` は変数xに5を代入しますが、`x == 5` は x が 5 と等しいかどうかを評価します。

#### 比較演算子の応用 ⭐

```python
# 範囲の確認
age = 25
is_adult = age >= 18     # 18歳以上は成人
print(is_adult)          # True

# 文字列の比較（辞書順で比較される）
name1 = "Alice"
name2 = "Bob"
print(name1 < name2)     # True ('A' は 'B' より前)

# 等価性の確認
answer = "yes"
is_affirmative = answer == "yes" or answer == "Yes"
print(is_affirmative)    # True
```

### 4.4 論理演算子

論理演算子は、ブール値や条件式を組み合わせるために使用します。

<table><thead><tr><th width="82.04437255859375">演算子</th><th width="96.6666259765625">名前</th><th>説明</th><th>例</th></tr></thead><tbody><tr><td><code>and</code></td><td>論理積</td><td>両方の条件が True なら True</td><td><code>True and True</code> → <code>True</code></td></tr><tr><td><code>or</code></td><td>論理和</td><td>少なくとも 1つの条件が True なら True</td><td><code>True or False</code> → <code>True</code></td></tr><tr><td><code>not</code></td><td>論理否定</td><td>条件の真偽を反転</td><td><code>not True</code> → <code>False</code></td></tr></tbody></table>

```python
p = True
q = False

# AND 演算 (両方が True の場合のみ True)
print(p and q)  # False

# OR 演算 (少なくとも一方が True なら True)
print(p or q)   # True

# NOT演算 (真偽を反転)
print(not p)    # False
print(not q)    # True
```

#### 論理演算子の応用 ⭐

```python
# 複合条件
age = 25
income = 300000

# AND 条件: 両方の条件を満たす必要がある
is_loan_eligible = age >= 20 and income >= 250000
print(f"ローン申請資格: {is_loan_eligible}")  # True

# OR 条件: どちらかの条件を満たせばよい
day = "土曜日"
is_weekend = day == "土曜日" or day == "日曜日"
print(f"週末ですか？: {is_weekend}")  # True

# NOT 条件: 条件の否定
is_holiday = False
is_working_day = not is_holiday
print(f"仕事の日ですか？: {is_working_day}")  # True
```
