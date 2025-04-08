# Untitled

### 7.2 デバッグのヒント

#### 7.2.1 print文を使ったデバッグ ★

最も基本的なデバッグ方法は、`print`文を使って変数の値や実行の流れを確認することです。

```python
def calculate_discount(price, quantity):
    discount_rate = 0
    
    # デバッグ用のprint文
    print(f"入力値: price={price}, quantity={quantity}")
    
    if quantity >= 10:
        discount_rate = 0.1
    elif quantity >= 5:
        discount_rate = 0.05
    
    # 計算途中の値を確認
    print(f"割引率: {discount_rate}")
    
    discount = price * quantity * discount_rate
    final_price = price * quantity - discount
    
    # 計算結果を確認
    print(f"合計金額: {price * quantity}, 割引額: {discount}, 最終金額: {final_price}")
    
    return final_price
```

#### 7.2.2 条件式のデバッグ

条件式が期待通りに動作しない場合は、条件式自体の結果を出力してみるとよいでしょう。

```python
age = 25
has_license = False

# 条件式の評価結果を確認
condition1 = age >= 18
condition2 = has_license
print(f"成人: {condition1}, 免許: {condition2}")
print(f"運転可能か: {condition1 and condition2}")

if age >= 18 and has_license:
    print("運転できます")
else:
    print("運転できません")
```

#### 7.2.3 データ型のデバッグ

予期しない結果が出た場合は、変数のデータ型を確認すると問題の原因が見つかることがあります。

```python
# 型の確認
value1 = "42"
value2 = 42
print(f"value1の型: {type(value1)}, value2の型: {type(value2)}")
print(f"value1 == value2: {value1 == value2}")  # False（型が異なる）
print(f"int(value1) == value2: {int(value1) == value2}")  # True（型変換後）
```

> 📝 **ポイント**: デバッグは問題解決の重要なスキルです。システマティックにアプローチし、仮説を立てて検証することで、効率的にバグを発見できます。

### 7.3 よい習慣とコーディング規約

#### 7.3.1 意味のある変数名 ★

変数名は、その役割や目的を明確に表すものが望ましいです。

```python
# 悪い例
x = 10  # 何を表す変数か不明確
lst = [1, 2, 3]

# 良い例
age = 10  # 年齢を表す変数
numbers = [1, 2, 3]  # 数値のリスト
```

#### 7.3.2 コメントの活用 ★

コメントは、コードの意図や複雑な処理の説明に役立ちます。

```python
# 悪い例（コメントなし）
def calculate_score(correct, total):
    return (correct / total) * 100

# 良い例
def calculate_score(correct, total):
    """
    正解数と全問題数から得点（百分率）を計算する
    
    引数:
    correct -- 正解数
    total -- 全問題数
    
    戻り値:
    得点（0-100の範囲）
    """
    # 0で割ることを防ぐ
    if total == 0:
        return 0
    
    # 百分率に変換
    return (correct / total) * 100
```

#### 7.3.3 一貫したコーディングスタイル ★

Python公式のスタイルガイド（PEP 8）に従うことで、可読性の高いコードが書けます。

```python
# PEP 8に準拠した例
def calculate_average(numbers):
    """数値リストの平均値を計算する"""
    if not numbers:
        return 0
    
    total = sum(numbers)
    count = len(numbers)
    
    return total / count

# 関数呼び出しの例
data = [10, 20, 30, 40, 50]
average = calculate_average(data)
print(f"平均値: {average}")
```

> 📝 **ポイント**: 一貫したコーディングスタイルを守ることで、チーム開発がスムーズになり、自分のコードも読みやすくなります。多くのIDEにはPEP 8に準拠しているかチェックする機能があります。

> 📝 **ポイント**: プログラミングの学習は継続的な実践が重要です。「習うより慣れろ」の精神で、小さなプロジェクトから始めて徐々に複雑なプログラムに挑戦していきましょう。また、エラーやバグは学習の一部として受け入れ、解決プロセスを楽しむことが成長につながります。
