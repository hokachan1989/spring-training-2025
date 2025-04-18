# Draw.io 入門

### 1. Draw.io (diagrams.net) とは？

#### 1.1 Draw.io の基本概念

Draw.io (現在は [diagrams.net](https://app.diagrams.net/) という名称でも利用可能) は、Web ブラウザ上で動作する**無料**の作図ツールです。フローチャート、UML図、ネットワーク図、組織図など、様々な種類の図を直感的な操作で作成できます。

* **インストール不要:** Web ブラウザがあればすぐに利用開始できます。
* **無料:** 基本的な機能はすべて無料で利用できます。
* **多機能:** フローチャート以外にも豊富な図形やテンプレートが用意されています。
* **データ保存:** 作成した図は、自分のデバイス、Google Drive、OneDrive、GitHub など、様々な場所に保存できます。

このドキュメントでは、Draw.io の基本的な使い方、特にアルゴリズム学習に役立つ**フローチャート**の作成方法を中心に学びます。

#### 1.2 Draw.io へのアクセスと開始

1. Web ブラウザで [https://app.diagrams.net/](https://app.diagrams.net/) にアクセスします。
2. 最初に図の保存場所を選択する画面が表示されます。
   * **Device:** 自分の PC に直接保存します。
   * **Google Drive / OneDrive / Dropbox / GitHub / GitLab:** 各クラウドストレージに保存します（初回は連携認証が必要です）。
   * **後で決定する:** 保存場所を後で決めることもできます。
   * ここでは、まず「**Device**」または「**後で決定する**」を選ぶと簡単です。
3. 次に、「新しいダイアグラムを作成」または「既存のダイアグラムを開く」を選択します。ここでは「**新しいダイアグラムを作成**」をクリックします。
4. 「空白のダイアグラム」を選択し、「作成」をクリックします。（ファイル名を求められたら、例えば `my_flowchart.drawio` のように入力します。）

### 2. 基本的な操作画面

Draw.io の編集画面は、主に以下の要素で構成されています。

`[Draw.ioのインターフェース画像]`

* **メニューバー:** ファイル操作、編集、表示、配置などのメニューがあります。
* **ツールバー:** よく使う機能（元に戻す、拡大/縮小、図形の追加など）のボタンが並んでいます。
* **キャンバス:** 中央の白いエリア。ここに図形を配置して図を作成します。
* **図形ライブラリ (左パネル):** 様々なカテゴリの図形が表示されます。ここから図形をドラッグ＆ドロップでキャンバスに追加します。「フローチャート」カテゴリなどがよく使われます。
* **フォーマットパネル (右パネル):** 選択した図形や線の色、スタイル、テキストの書式などを設定するパネルです。何も選択していないときは、ページ全体の設定ができます。

### 3. 図形の追加と接続 (フローチャート作成例)

簡単なフローチャートを作成しながら、基本的な操作を学びましょう。「朝の準備」を例にします。

#### 3.1 図形の追加

1. **開始/終了:** 左パネルの「フローチャート」カテゴリから、「Terminator」（端子、角丸長方形）を探し、キャンバスにドラッグ＆ドロップします。これが「開始」の記号になります。
2. **処理:** 同じく「フローチャート」カテゴリから、「Process」（処理、長方形）を探し、開始記号の下にドラッグ＆ドロップします。
3. **終了:** もう一度「Terminator」をドラッグ＆ドロップし、処理記号の下に配置します。これが「終了」の記号です。

`[図形ライブラリとキャンバスに配置された図形の例]`

> 📝 **ポイント**: 左パネルの検索ボックスに「flowchart」や図形の名前（例: 「rectangle」）を入力して探すこともできます。

#### 3.2 図形の接続

図形同士を線（コネクタ）で繋ぎ、処理の流れを示します。

1. 「開始」の図形の上にマウスポインタを置くと、図形の周囲に青い「×」印や小さな矢印（接続ポイント）が表示されます。
2. 接続ポイントの一つ（例えば下の「×」印）をクリックしたまま、下の「処理」の図形までドラッグします。
3. 「処理」の図形の上にマウスポインタを移動させると、接続ポイントが表示されるので、目的の接続ポイント（例えば上の「×」印）でマウスボタンを離します。
4. これで、「開始」から「処理」への矢印が引かれます。
5. 同様に、「処理」から「終了」へも矢印を引きます。

`[接続ポイントから矢印を引いている様子の画像]`

> 💡 **ヒント**: 図形を選択した状態で表示される青い矢印をクリックすることでも、簡単に同じ図形をコピーして接続できます。

### 4. テキストの追加と書式設定

図形や線に説明を追加しましょう。

#### 4.1 テキストの追加

* **図形にテキストを追加:** テキストを追加したい図形（例: 「開始」の楕円）を**ダブルクリック**します。カーソルが表示されるので、テキスト（例: `開始`）を入力します。
* **線にテキストを追加:** 同様に、テキストを追加したい線（矢印）を**ダブルクリック**し、テキスト（例: 条件分岐の `Yes`/`No` など）を入力します。
* テキストの編集が終わったら、キャンバスの何もないところをクリックします。

#### 4.2 書式設定

図形やテキストの見た目を変更します。

1. 書式を変更したい図形、線、またはテキストを選択します。
2.  画面右側のフォーマットパネルを使用します。

    \[フォーマットパネルの例]

    * **スタイル タブ:** 図形の塗りつぶし色、線の色、線の種類（実線、点線など）、角の丸みなどを変更できます。
    * **テキスト タブ:** フォントの種類、サイズ、色、太字、斜体、配置などを変更できます。
    * **配置 タブ:** 図形のサイズ、位置、角度、重なり順などを調整できます。

**例:** 「開始」と「終了」の図形を選択し、スタイルタブで背景色を緑に、「処理」の図形を選択して背景色を青に変更してみましょう。

### 5. 保存とエクスポート

作成した図を保存したり、他の形式で出力したりします。

#### 5.1 保存

* メニューバーの「ファイル」>「保存」を選択します。（最初に保存場所として「Device」を選んだ場合は、ファイルがダウンロードされます。クラウドストレージを選んだ場合は、そこに保存されます。）
* 「ファイル」>「名前を付けて保存」で、ファイル名や保存場所を変更して保存できます。
* Draw.io の標準ファイル形式は `.drawio` (または `.xml`) です。この形式で保存しておけば、後で再編集できます。

#### 5.2 エクスポート

作成した図を画像ファイルや PDF として出力します。

1. メニューバーの「ファイル」>「形式を指定してエクスポート」を選択します。
2. 出力したい形式（PNG, JPG, SVG, PDF など）を選びます。
3. 必要に応じて、エクスポート設定（背景を透明にするか、図全体か選択部分かなど）を調整します。
4. 「エクスポート」ボタンをクリックし、保存場所（またはダウンロード）を指定します。

> 📝 **ポイント**: PNG や JPG はレポートやプレゼンテーションに貼り付けるのに便利です。SVG は拡大しても劣化しないベクター形式、PDF は文書としての配布に適しています。

これで、Draw.io (diagrams.net) の基本的な使い方の説明は終わりです。まずは簡単なフローチャートを作成してみて、図形の追加、接続、テキスト入力、書式設定、保存・エクスポートの一連の流れに慣れてみてください。
