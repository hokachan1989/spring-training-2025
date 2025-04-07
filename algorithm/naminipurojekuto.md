# 実践的なミニプロジェクト

以下は、ここまでに学んだ知識を活用して作成できる小規模なプロジェクトのアイデアです。自分のペースで挑戦してみましょう。

### 7.1 数当てゲーム

コンピュータがランダムに選んだ1〜100の数字を、ユーザーが当てるゲームを作成します。

```python
import random

def number_guessing_game():
    """数当てゲーム
    コンピュータが1-100の数字をランダムに選び、プレイヤーがそれを当てる
    """
    # タイトル表示
    print("===== 数当てゲーム =====")
    print("1から100までの数字を当ててください")
    
    # 正解の数字を生成
    secret_number = random.randint(1, 100)
    attempts = 0
    max_attempts = 10
    
    # ゲームループ
    while attempts < max_attempts:
        try:
            # ユーザーからの入力
            guess = int(input(f"予想を入力（残り{max_attempts - attempts}回）: "))
            attempts += 1
            
            # 入力値のチェック
            if guess < 1 or guess > 100:
                print("1から100までの数字を入力してください。")
                continue
            
            # 結果判定
            if guess < secret_number:
                print("もっと大きい数字です。")
            elif guess > secret_number:
                print("もっと小さい数字です。")
            else:
                print(f"正解です！{attempts}回で当てました！")
                break
                
        except ValueError:
            print("有効な数字を入力してください。")
    
    # ゲーム終了処理
    if attempts >= max_attempts and guess != secret_number:
        print(f"ゲームオーバー。正解は{secret_number}でした。")
    
    # もう一度プレイするか確認
    play_again = input("もう一度プレイしますか？ (y/n): ").lower()
    if play_again == "y":
        number_guessing_game()  # 再帰的に関数を呼び出して再プレイ
    else:
        print("プレイしていただきありがとうございました！")

# ゲームの実行
number_guessing_game()
```

#### 7.6.2 簡易家計簿アプリ

収入と支出を記録し、残高や統計情報を表示する簡易的な家計簿アプリを作成します。

```python
def simple_accounting_app():
    """簡易家計簿アプリ
    収入と支出を記録し、残高や統計情報を表示する
    """
    # 初期化
    transactions = []
    balance = 0
    
    # メインループ
    while True:
        # メニュー表示
        print("\n===== 簡易家計簿 =====")
        print("1. 取引の記録")
        print("2. 取引履歴の表示")
        print("3. 残高の確認")
        print("4. 統計情報の表示")
        print("5. 終了")
        
        # メニュー選択
        choice = input("選択してください（1-5）: ")
        
        # 取引の記録
        if choice == "1":
            try:
                transaction_type = input("収入 or 支出（i/e）: ").lower()
                if transaction_type not in ["i", "e"]:
                    print("iまたはeを入力してください。")
                    continue
                
                amount = float(input("金額を入力してください: "))
                if amount <= 0:
                    print("正の数を入力してください。")
                    continue
                
                description = input("説明を入力してください: ")
                
                # 取引情報の保存
                if transaction_type == "i":  # 収入
                    transactions.append({"type": "収入", "amount": amount, "description": description})
                    balance += amount
                    print(f"{amount}円の収入を記録しました。")
                else:  # 支出
                    transactions.append({"type": "支出", "amount": amount, "description": description})
                    balance -= amount
                    print(f"{amount}円の支出を記録しました。")
            
            except ValueError:
                print("有効な数値を入力してください。")
        
        # 取引履歴の表示
        elif choice == "2":
            if not transactions:
                print("取引履歴がありません。")
                continue
                
            print("\n===== 取引履歴 =====")
            for i, tx in enumerate(transactions, 1):
                print(f"{i}. {tx['type']} - {tx['amount']}円 - {tx['description']}")
        
        # 残高の確認
        elif choice == "3":
            print(f"\n現在の残高: {balance}円")
            
            if balance < 0:
                print("警告: 残高がマイナスです！")
            elif balance < 10000:
                print("注意: 残高が10,000円を下回っています。")
        
        # 統計情報の表示
        elif choice == "4":
            if not transactions:
                print("取引履歴がありません。")
                continue
                
            income = sum(tx["amount"] for tx in transactions if tx["type"] == "収入")
            expense = sum(tx["amount"] for tx in transactions if tx["type"] == "支出")
            
            print("\n===== 統計情報 =====")
            print(f"総収入: {income}円")
            print(f"総支出: {expense}円")
            print(f"収支差額: {income - expense}円")
            
            if income > 0:
                savings_rate = ((income - expense) / income) * 100
                print(f"貯蓄率: {savings_rate:.1f}%")
                
                if savings_rate < 0:
                    print("警告: 支出が収入を上回っています！")
                elif savings_rate < 20:
                    print("アドバイス: 貯蓄率が20%を下回っています。支出の見直しを検討してください。")
        
        # 終了
        elif choice == "5":
            print("家計簿アプリを終了します。お疲れ様でした！")
            break
            
        else:
            print("無効な選択です。1から5の数字を入力してください。")

# アプリの実行
simple_accounting_app()
```

### 単語学習アプリ

単語とその意味を登録し、ランダムに出題するクイズ形式の学習アプリを作成します。

```python
import random

def vocabulary_trainer():
    """単語学習アプリ
    単語とその意味を登録し、ランダムに出題するクイズ形式の学習アプリ
    """
    # 単語帳の初期化
    vocab = {}
    
    # メインループ
    while True:
        # メニュー表示
        print("\n===== 単語学習アプリ =====")
        print("1. 単語の登録")
        print("2. 単語一覧の表示")
        print("3. 学習モード")
        print("4. 終了")
        
        # メニュー選択
        choice = input("選択してください（1-4）: ")
        
        # 単語の登録
        if choice == "1":
            word = input("登録する単語を入力してください: ").strip()
            if not word:
                print("単語を入力してください。")
                continue
                
            # 既存の単語かチェック
            if word in vocab:
                overwrite = input(f"「{word}」は既に登録されています。上書きしますか？ (y/n): ").lower()
                if overwrite != "y":
                    continue
            
            meaning = input(f"「{word}」の意味/定義を入力してください: ").strip()
            if not meaning:
                print("意味を入力してください。")
                continue
                
            # 単語の保存
            vocab[word] = meaning
            print(f"「{word}」を登録しました。")
        
        # 単語一覧の表示
        elif choice == "2":
            if not vocab:
                print("単語が登録されていません。")
                continue
                
            print("\n===== 登録済み単語一覧 =====")
            for i, (word, meaning) in enumerate(vocab.items(), 1):
                print(f"{i}. {word} - {meaning}")
        
        # 学習モード
        elif choice == "3":
            if len(vocab) < 3:
                print("学習モードを開始するには、最低3つの単語を登録してください。")
                continue
                
            print("\n===== 学習モード =====")
            print("単語の意味を答えてください。終了するにはqを入力してください。")
            
            # 学習用の単語リストを作成
            words = list(vocab.keys())
            score = 0
            total = 0
            
            # クイズループ
            while words:
                # ランダムに単語を選択
                word = random.choice(words)
                
                # 問題の出題
                print(f"\n問題: 「{word}」の意味は？")
                answer = input("答え: ").strip()
                
                # 終了チェック
                if answer.lower() == "q":
                    break
                    
                # 正誤判定（部分一致で正解とする）
                if answer.lower() in vocab[word].lower():
                    print("正解です！")
                    score += 1
                    words.remove(word)  # 正解した単語は出題リストから除外
                else:
                    print(f"不正解です。正しい意味は「{vocab[word]}」です。")
                
                total += 1
                
                # 全単語を出題したら終了
                if not words:
                    print("\nすべての単語を学習しました！")
                    break
            
            # 結果表示
            if total > 0:
                print(f"\n===== 学習結果 =====")
                print(f"問題数: {total}")
                print(f"正解数: {score}")
                print(f"正答率: {(score / total) * 100:.1f}%")
        
        # 終了
        elif choice == "4":
            print("単語学習アプリを終了します。お疲れ様でした！")
            break
            
        else:
            print("無効な選択です。1から4の数字を入力してください。")

# アプリの実行
vocabulary_trainer()
```

> 📝 **ポイント**: プロジェクトは小さく始めて、徐々に機能を追加していくのがおすすめです。例えば、最初は基本機能だけを実装し、後からファイルへの保存機能やグラフィカルなインターフェース、より高度なアルゴリズムなどを追加できます。自分の興味や目的に合わせてプロジェクトをカスタマイズしましょう。



***

次のセクションでは、「Python制御構造とデータ構造（午前）」について学びます。そこでは、ループ構造とリストを中心に学習を深めていきます。
