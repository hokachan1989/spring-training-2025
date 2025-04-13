---
description: 入門
---

# 14. Gemini API 入門

### 1. Gemini API とは？ Google Colab での準備

#### 1.1 Gemini API の基本概念

Gemini API は、Google が開発した高性能な生成AIモデル「Gemini」を利用するためのインターフェースです。テキスト生成、翻訳、要約、質疑応答、コード生成など、様々なタスクを実行できます。

このセクションでは、Google Colab を使って Gemini API を利用するための準備を行います。

#### 1.2 準備の手順

**前提:** Google アカウントを持っていること。

**手順1: API キーを取得する**

Gemini API を利用するには API キーが必要です。以下の手順で取得します。

1. [Google AI Studio](https://aistudio.google.com/app/apikey) にアクセスします。
2. Google アカウントでログインします。
3. 「Create API key in new project」または既存のプロジェクトを選択して API キーを作成します。
4. 生成された API キーをコピーして、安全な場所に保管してください。 **このキーは他人に知られないように注意してください。**

**手順2: Google Colab を開く**

[Google Colab](https://colab.research.google.com/) にアクセスし、新しいノートブックを作成します。

**手順3: API キーを Colab Secrets に保存する**

API キーをコードに直接書き込むのはセキュリティ上好ましくありません。Colab の Secrets 機能を使って安全に管理しましょう。

1. Colab の左側メニューにある鍵アイコン（🔑 Secrets）をクリックします。
2. 「新しいシークレットを追加」をクリックします。
3. `名前` に `GEMINI_API_KEY` と入力します。
4. `値` に、手順1で取得したあなたの API キーを貼り付けます。
5. 「ノートブックへのアクセス」をオンにします。
6. これで、コードから安全に API キーを呼び出せるようになりました。

**手順4: 必要なライブラリをインストールする**

Gemini API を Python で利用するためのライブラリをインストールします。Colab のコードセルに以下のコマンドを入力して実行（Shift + Enter）します。

```
!pip install -q google-generativeai

```

> 📝 **ポイント**: `!` から始まるコマンドは、Colab 上で Linux のシェルコマンドを実行するためのものです。`-q` はインストール時のログ出力を抑制するオプションです。

**手順1.5: 料金について (重要)**

Gemini API の利用には、選択するモデルや使用量に応じて料金が発生する場合があります。

* **無料枠:** 多くの場合、実験や学習目的のための無料利用枠が提供されています（特に Google AI Studio 経由での利用開始時など）。ただし、条件は変更される可能性があります。
* **モデルによる違い:** `gemini-1.5-flash` や `gemini-pro` など、モデルによって料金体系が異なります。
* **最新情報の確認:** 必ず公式サイトで最新の料金情報を確認してください。
  * [Google AI for Developers Pricing](https://ai.google.dev/pricing) (Google AI Studio / `google-generativeai` ライブラリ向け)
  * [Google Cloud Vertex AI Pricing](https://cloud.google.com/vertex-ai/pricing) (Google Cloud経由での利用の場合)
* **利用状況の監視:** APIキーの使用状況や関連する請求を定期的に確認することをお勧めします。

> ⚠️ **注意**: 無料枠を超過した場合や、有料プランを利用する場合には費用が発生します。利用を開始する前に、料金体系をよく理解しておきましょう。

### 2. API キーの設定とクライアントの初期化

次に、インストールしたライブラリを使って、API キーを設定し、Gemini モデルを利用するためのクライアントを初期化します。

新しいコードセルに以下の Python コードを入力して実行します。

```
import google.generativeai as genai
from google.colab import userdata

# Colab Secrets から API キーを取得
# Secretsにキーがない場合はエラーになるので注意
try:
    api_key = userdata.get('GEMINI_API_KEY')
    if not api_key:
        raise ValueError("APIキーがSecretsに設定されていません。")
    # APIキーを設定
    genai.configure(api_key=api_key)
    print("APIキーの設定完了。")
except Exception as e:
    print(f"エラー: APIキーの取得または設定に失敗しました - {e}")
    api_key = None # エラー時はNoneにする

# APIキーが設定できた場合のみモデルを初期化
if api_key:
    try:
        # 使用するモデルを初期化 (ここでは gemini-1.5-flash を使用)
        # 他のモデル例: 'gemini-pro'
        model = genai.GenerativeModel('gemini-1.5-flash')
        print("Gemini API のモデル初期化が完了しました。")
    except Exception as e:
        print(f"エラー: モデルの初期化に失敗しました - {e}")
        model = None # エラー時はNoneにする
else:
    model = None
    print("APIキーが設定されていないため、モデルを初期化できません。")


```

> 📝 **ポイント**:
>
> * `google.generativeai` ライブラリを `genai` という名前でインポートするのが一般的です。
> * `userdata.get('GEMINI_API_KEY')` で、先ほど Secrets に保存した API キーを安全に取得します。エラー処理を追加して、キーがない場合に分かりやすくしています。
> * `genai.configure()` でライブラリに API キーを認識させます。
> * `genai.GenerativeModel()` で、利用したい Gemini モデルを指定してインスタンスを作成します。`'gemini-1.5-flash'` は高速でコスト効率の良いモデルです。より高機能な `'gemini-pro'` なども利用可能です。

### 3. 基本的なテキスト生成 (generate\_content)

モデルの準備ができたので、実際にテキストを生成させてみましょう。最も基本的な使い方は `generate_content()` メソッドにプロンプト（指示文）を渡すことです。

新しいコードセルに以下の Python コードを入力して実行します。

```
# モデルが正常に初期化されているか確認
if model:
    try:
        # プロンプト（AIへの指示）を定義
        prompt = "日本の首都はどこですか？"

        # AIにプロンプトを送信して回答を生成
        response = model.generate_content(prompt)

        # 生成されたテキストを表示
        print("\n--- AIからの回答 ---")
        print(response.text)
    except Exception as e:
        print(f"\nエラー: コンテンツの生成に失敗しました - {e}")
else:
    print("\nモデルが初期化されていないため、コンテンツを生成できません。")


```

> 📝 **ポイント**:
>
> * `model` が正しく初期化されているか確認してから実行するようにしています。
> * `prompt` 変数に、AI に実行してほしい指示や質問を文字列として格納します。
> * `model.generate_content(prompt)` を呼び出すと、AI がプロンプトに基づいてテキストを生成し、`response` オブジェクトとして返します。
> * 生成されたテキスト本体は `response.text` プロパティで取得できます。
> * API通信などでエラーが発生する可能性もあるため、`try...except` で囲むとより安全です。

### 4. プロンプトの工夫と応用

Gemini API の能力を引き出す鍵は**プロンプト**です。指示の仕方を変えることで、様々な応答を得ることができます。

#### 例1: 簡単な説明を求める

```
if model:
    try:
        prompt_explain = "プログラミングにおける「変数」とは何か、初心者にも分かりやすく説明してください。"
        response_explain = model.generate_content(prompt_explain)
        print("\n--- 変数の説明 ---")
        print(response_explain.text)
    except Exception as e:
        print(f"\nエラー: コンテンツの生成に失敗しました - {e}")
else:
    print("\nモデルが初期化されていないため、コンテンツを生成できません。")

```

#### 例2: アイデア出し

```
if model:
    try:
        prompt_idea = "小学生向けの夏休みの自由研究のテーマを3つ提案してください。"
        response_idea = model.generate_content(prompt_idea)
        print("\n--- 自由研究のアイデア ---")
        print(response_idea.text)
    except Exception as e:
        print(f"\nエラー: コンテンツの生成に失敗しました - {e}")
else:
    print("\nモデルが初期化されていないため、コンテンツを生成できません。")

```

#### 例3: 簡単な翻訳

```
if model:
    try:
        prompt_translate = "「こんにちは、世界」を英語に翻訳してください。"
        response_translate = model.generate_content(prompt_translate)
        print("\n--- 翻訳結果 ---")
        print(response_translate.text)
    except Exception as e:
        print(f"\nエラー: コンテンツの生成に失敗しました - {e}")
else:
    print("\nモデルが初期化されていないため、コンテンツを生成できません。")

```

> 💡 **ヒント**:
>
> * **具体的に:** AI に何をしてほしいのか、できるだけ具体的に指示しましょう。
> * **役割を与える:** 「あなたはプロの翻訳家です。」のように役割を与えると、応答の質が変わることがあります。
> * **フォーマットを指定する:** 「箇条書きで」「表形式で」のように出力形式を指定することも有効です。
> * **試行錯誤:** 色々なプロンプトを試して、望む結果が得られるように調整してみましょう！

### 5. 実行と結果の確認

作成したコードを実行し、Gemini API がどのように応答するかを確認しましょう。

#### Colab セルの実行方法

* 実行したいコードセルを選択します。
* 以下のいずれかの方法で実行します。
  * セルの左側にある再生ボタン（▶）をクリックする。
  * `Shift + Enter` キーを押す。

#### 結果の確認

* `print()` 関数で出力するように記述した内容は、コードセルのすぐ下に表示されます。
* `response.text` の内容を確認し、プロンプトに対して AI が適切に応答しているか見てみましょう。

#### うまくいかないときは？

* **API キーのエラー:**
  * Secrets に API キーが正しく設定されているか確認してください（手順1.2参照）。
  * `genai.configure(api_key=api_key)` がエラーなく実行されているか確認してください（手順2参照）。
  * API キーが有効か（Google AI Studio で確認）確認してください。
* **ライブラリのインストール:**
  * `!pip install -q google-generativeai` が正しく実行されたか確認してください（手順1.4参照）。エラーメッセージが出ていないか確認しましょう。
* **プロンプトの内容:**
  * 非常に複雑な指示や、不適切な内容のプロンプトは、うまく処理されない場合があります。簡単なプロンプトから試してみてください。
* **モデルの選択:**
  * `genai.GenerativeModel()` で指定したモデル名が正しいか確認してください。利用可能なモデルは変更されることがあります。
* **エラーメッセージを読む:**
  * コード実行時にエラーメッセージが表示された場合は、その内容をよく読んでみましょう。問題解決の手がかりが含まれていることが多いです。

これで、Google Colab を使って Gemini API の基本的な使い方を体験できました。色々なプロンプトを試して、生成AIの可能性を探ってみてください！
