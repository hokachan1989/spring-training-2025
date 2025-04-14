# PokeAPI 入門

### 1. 準備

#### 1.1 PokeAPI とは

PokeAPI は、ポケモンに関するデータを提供する Web API です。ポケモンの名前、タイプ、特性、画像など、様々な情報を取得することができます。プログラムから簡単にアクセスできるため、ポケモン図鑑やポケモン情報を活用したアプリケーションの開発に適しています。

#### 1.2 Google Colab の準備

[Google Colab](https://colab.research.google.com/) にアクセスして、新しいノートブックを作成します。利用には Google アカウントが必要です。

### 2. 最初のポケモンデータの取得

ここからプログラムを実装し、ピカチュウの情報を取得します。

#### 2.1 API リクエストの送信

```python
# 必要なライブラリをインポート
import requests

# ピカチュウの情報をリクエスト
response = requests.get("https://pokeapi.co/api/v2/pokemon/pikachu")

# 結果を確認
print("ステータスコード:", response.status_code)
```

\<details> \<summary>実行結果\</summary>

```
ステータスコード: 200
```

\</details>

> 📝 **ポイント**: ステータスコード 200 が表示されれば成功です。これは API からデータが正しく取得できたことを意味します。

#### 2.2 データの抽出

```python
# JSONデータを取得
pokemon_data = response.json()

# 基本情報を表示
print("名前:", pokemon_data["name"])
print("図鑑番号:", pokemon_data["id"])
print("高さ:", pokemon_data["height"] / 10, "m")  # 単位をメートルに変換
print("重さ:", pokemon_data["weight"] / 10, "kg")  # 単位をキログラムに変換
```

\<details> \<summary>実行結果\</summary>

```
名前: pikachu
図鑑番号: 25
高さ: 0.4 m
重さ: 6.0 kg
```

\</details>

> 📝 **ポイント**: JSON データは辞書のように `["キー名"]` でアクセスできます。高さと重さは API では特殊な単位で返されるため、10 で割って一般的な単位に変換します。

#### 2.3 タイプ情報の取得

```python
# タイプ情報を表示
types = []
for type_info in pokemon_data["types"]:
    types.append(type_info["type"]["name"])

print("タイプ:", ", ".join(types))
```

\<details> \<summary>実行結果\</summary>

```
タイプ: electric
```

\</details>

> 📝 **ポイント**: タイプ情報はリストとして格納されているため、ループを使って取り出し、最後に `join` メソッドで文字列に結合します。

#### 2.4 画像の表示

```python
# 画像表示のためのライブラリをインポート
from IPython.display import Image, display

# 画像URLを取得
sprite_url = pokemon_data["sprites"]["front_default"]

# 画像を表示
display(Image(url=sprite_url, width=200))
```

\<details> \<summary>実行結果\</summary>

画像表示結果: ピカチュウのスプライト画像が表示されます。

\</details>

> 📝 **ポイント**: `IPython.display` モジュールを使用すると、Colab 上で画像を表示できます。ポケモンの画像 URL は `sprites` 辞書内に格納されています。

### 3. お気に入りのポケモンを検索しよう

#### 3.1 関数にまとめよう

```python
def get_pokemon_info(pokemon_name):
    """
    ポケモンの名前を受け取り、情報を表示する関数
    """
    # APIリクエストを送信
    url = f"https://pokeapi.co/api/v2/pokemon/{pokemon_name.lower()}"
    response = requests.get(url)
    
    # エラーチェック
    if response.status_code != 200:
        print(f"エラー: {pokemon_name} の情報を取得できませんでした")
        return
    
    # データを取得
    pokemon_data = response.json()
    
    # 基本情報の表示
    print(f"名前: {pokemon_data['name']}")
    print(f"図鑑番号: {pokemon_data['id']}")
    print(f"高さ: {pokemon_data['height'] / 10} m")
    print(f"重さ: {pokemon_data['weight'] / 10} kg")
    
    # タイプ情報の表示
    types = []
    for type_info in pokemon_data["types"]:
        types.append(type_info["type"]["name"])
    print(f"タイプ: {', '.join(types)}")
    
    # 画像の表示
    sprite_url = pokemon_data["sprites"]["front_default"]
    display(Image(url=sprite_url, width=200))
    
    return pokemon_data
```

#### 3.2 入力を受け取ろう

```python
# ユーザーからの入力を受け取る
pokemon_name = input("ポケモンの名前を英語で入力してください: ")

# 関数を呼び出す
get_pokemon_info(pokemon_name)
```

> 📝 **ポイント**: `input()` 関数を使うことで、ユーザーから入力を受け取れます。入力したポケモン名の情報が表示されます。英語の名前か図鑑番号を入力してください。

### 4. 日本語名表示機能の追加

#### 4.1 日本語名取得関数の実装

```python
def get_japanese_name(pokemon_data):
    """
    ポケモンデータから日本語名を取得する関数
    
    引数:
    pokemon_data -- PokeAPIから取得したポケモンデータ
    
    戻り値:
    日本語名（見つからない場合は英語名）
    """
    # species情報のURLを取得
    species_url = pokemon_data["species"]["url"]
    
    # speciesデータをリクエスト
    response = requests.get(species_url)
    species_data = response.json()
    
    # 日本語名を探す
    for name_info in species_data["names"]:
        if name_info["language"]["name"] == "ja":
            return name_info["name"]
    
    # 見つからなければ英語名を返す
    return pokemon_data["name"]
```

#### 4.2 表示関数の拡張

```python
def get_pokemon_info(pokemon_name):
    """
    ポケモンの情報を取得・表示する関数（日本語名表示機能追加版）
    
    引数:
    pokemon_name -- ポケモンの名前または図鑑番号
    
    戻り値:
    取得したポケモンデータ（辞書型）、エラー時はNone
    """
    # APIリクエストを送信
    url = f"https://pokeapi.co/api/v2/pokemon/{pokemon_name.lower()}"
    response = requests.get(url)
    
    # エラーチェック
    if response.status_code != 200:
        print(f"エラー: {pokemon_name} の情報を取得できませんでした")
        return None
    
    # データを取得
    pokemon_data = response.json()
    
    # 日本語名を取得
    japanese_name = get_japanese_name(pokemon_data)
    
    # 基本情報の表示
    print(f"日本語名: {japanese_name}")
    print(f"英語名: {pokemon_data['name']}")
    print(f"図鑑番号: {pokemon_data['id']}")
    print(f"高さ: {pokemon_data['height'] / 10} m")
    print(f"重さ: {pokemon_data['weight'] / 10} kg")
    
    # タイプ情報の表示
    types = []
    for type_info in pokemon_data["types"]:
        types.append(type_info["type"]["name"])
    print(f"タイプ: {', '.join(types)}")
    
    # 画像の表示
    sprite_url = pokemon_data["sprites"]["front_default"]
    display(Image(url=sprite_url, width=200))
    
    return pokemon_data

# 関数の実行
pokemon_name = input("ポケモンの名前を英語で入力してください: ")
get_pokemon_info(pokemon_name)
```

\<details> \<summary>実行結果（例: "pikachu" と入力した場合）\</summary>

```
ポケモンの名前を英語で入力してください: pikachu
日本語名: ピカチュウ
英語名: pikachu
図鑑番号: 25
高さ: 0.4 m
重さ: 6.0 kg
タイプ: electric
```

ピカチュウの画像が表示されます。

\</details>

> 📝 **ポイント**: `species` エンドポイントには、ポケモンの分類や説明文など、ゲーム内での情報が含まれています。その中から、日本語名（`language.name` が `ja` のもの）を探して取得しています。

### 5. ランダムポケモン表示機能の実装

#### 5.1 ランダム選択機能

```python
import random

def show_random_pokemon():
    """
    ランダムなポケモンを表示する関数
    
    戻り値:
    取得したポケモンデータ（辞書型）、エラー時はNone
    """
    # ランダムな図鑑番号を生成（1〜898）
    random_id = random.randint(1, 898)
    
    # 情報を取得して表示
    return get_pokemon_info(str(random_id))

# ランダムポケモンを表示
show_random_pokemon()
```

\<details> \<summary>実行結果\</summary>

```
日本語名: [ランダムなポケモンの日本語名]
英語名: [ランダムなポケモンの英語名]
図鑑番号: [ランダムな番号]
高さ: [値] m
重さ: [値] kg
タイプ: [ポケモンのタイプ]
```

ランダムに選ばれたポケモンの画像が表示されます。

\</details>

> 📝 **ポイント**: `random` モジュールを使用してランダムな数値を生成し、その図鑑番号に対応するポケモン情報を表示しています。図鑑番号は文字列に変換してAPIリクエストに使用します。

### 6. ポケモンクイズ機能の実装

#### 6.1 シンプルなクイズゲーム

```python
def pokemon_quiz():
    """
    ポケモンの画像を表示し、名前を当てるシンプルなクイズゲーム
    """
    # ランダムなポケモンのデータを取得
    random_id = random.randint(1, 898)
    
    # APIリクエストを送信
    url = f"https://pokeapi.co/api/v2/pokemon/{random_id}"
    response = requests.get(url)
    
    if response.status_code != 200:
        print("ポケモンデータの取得に失敗しました。もう一度実行してください。")
        return
    
    # データを取得
    pokemon_data = response.json()
    
    # 日本語名を取得
    correct_name = get_japanese_name(pokemon_data)
    
    # 画像だけを表示（名前は表示しない）
    sprite_url = pokemon_data["sprites"]["front_default"]
    display(Image(url=sprite_url, width=200))
    
    # ユーザーの回答を受け取る
    user_answer = input("このポケモンの名前は？ ")
    
    # 答え合わせ
    if user_answer == correct_name:
        print("正解です。おめでとうございます。")
    else:
        print(f"不正解です。正解は「{correct_name}」でした。")

# クイズを実行
pokemon_quiz()
```

\<details> \<summary>実行結果（例）\</summary>

```
このポケモンの名前は？ ピカチュウ
不正解です。正解は「[正解のポケモン名]」でした。
```

※ランダムに表示されるポケモンによって結果は異なります。

\</details>

> 📝 **ポイント**: ランダムなポケモンを取得して画像のみを表示し、ユーザーにポケモンの名前を答えてもらうシンプルなクイズゲームです。実際のポケモンゲームのような「だれだ？」クイズを再現しています。

### 7. ポケモンチーム表示機能の実装

#### 7.1 複数ポケモンの一括表示

```python
def show_pokemon_team(pokemon_names):
    """
    ポケモンチームの情報を表示する関数
    
    引数:
    pokemon_names -- ポケモン名または図鑑番号のリスト
    """
    print("===== ポケモンチーム =====")
    
    # 各ポケモンの情報を表示
    for pokemon_name in pokemon_names:
        print("\n-----------------------")
        get_pokemon_info(pokemon_name)
    
    print("=======================")

# ポケモンチームを表示
team = ["pikachu", "charizard", "bulbasaur", "squirtle", "eevee", "mewtwo"]
show_pokemon_team(team)
```

\<details> \<summary>実行結果\</summary>

```
===== ポケモンチーム =====

-----------------------
日本語名: ピカチュウ
英語名: pikachu
図鑑番号: 25
高さ: 0.4 m
重さ: 6.0 kg
タイプ: electric

-----------------------
日本語名: リザードン
英語名: charizard
図鑑番号: 6
高さ: 1.7 m
重さ: 90.5 kg
タイプ: fire, flying

...（以下、残りのポケモン情報）...
=======================
```

各ポケモンの画像も表示されます。

\</details>

> 📝 **ポイント**: リストの要素をループ処理することで、複数のポケモン情報を連続して表示しています。ポケモンのリストは自分の好みのポケモンに変更して実行することも可能です。

### 8. 発展的な実装アイデア

ここまでの基本機能を理解したら、以下のような機能を追加して機能を拡張することができます。

1. **タイプ別カラー表示**: ポケモンのタイプに応じて表示色を変更し、視覚的に情報を整理する。
2. **進化系列表示機能**: ポケモンの進化前と進化後を表示するプログラムの実装。
3. **タイプ検索機能**: 特定のタイプ（例: 「でんき」タイプ）のポケモンのみを抽出して表示する機能。
4. **ポケモン比較機能**: 複数のポケモンのステータスを比較表示する機能。
5. **高度なクイズ**: タイプ推測や進化系列に関するクイズなど、より難易度の高いクイズの実装。

> 📝 **ポイント**: Web API を活用することで、様々な情報を組み合わせた独自のアプリケーションを開発することが可能です。学んだ知識を応用して、オリジナルの機能を実装してみましょう。

### まとめ

本演習では、PokeAPI を活用して以下の内容を学習しました：

1. Web API からのデータ取得手法
2. JSON データからの情報抽出方法
3. 画像表示の実装
4. 関数によるコードのモジュール化
5. ランダム要素の導入方法

これらの技術は、他の Web API を使った開発にも応用可能な基礎的なスキルとなります。

### 補足情報

* PokeAPI は英語ベースのため、ポケモン名は英語表記（例: `pikachu`, `charizard`）または図鑑番号で指定する必要があります。
* 短時間に多数のリクエストを送信すると、一時的に利用制限が適用される場合があります。その場合は一定時間後に再試行してください。
* エラーが発生した場合は、レスポンスのステータスコードを確認し、URL の正確性を確認してください。
* `print(pokemon_data.keys())` を実行すると、取得可能な情報の種類を確認できます。
* データ構造の詳細を確認する場合は `print(pokemon_data)` で JSON データ全体を表示できますが、表示量が多いため注意が必要です。
