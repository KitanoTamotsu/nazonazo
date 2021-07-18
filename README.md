## 　　Lesson17.　Alfred環境変数を使ってカスタマイズ設定を可能とする  
[サンプル動画]

https://user-images.githubusercontent.com/40127279/126052719-60a720fb-afe0-4483-baf4-0c880d366a23.mp4


#### 開発メモ
### 1.Alfredの環境変数を設定値をとして利用する
　なぞなぞの出題はAlfredの出力フォーマットなので1度に9個出題できます
<br>　HTMLのパースからJSONフォーマットへの編集は今までのやり方と同じ
<br>　ただ、実際に遊んでみると、回答が見れるのは、出題後に選択した1問だけ
<br>　ちょっともどかしい
<br>
<br>　そこで、下記のカスタマイズを可能するようにしました
<br>　・出力フォーマットに表示する出題数
<br>　・出題した問題を記録するかどうか
<br>
<br>　このコントロールをするために、Alfredの環境変数を利用します
<br>　プロパティリスト（.plist）のイメージで利用するわけです
<br>　
<br>　具体的にはWorkflow Environment VariablesにQmaxとoutputを記載します
<br>　いったんQmaxは3を設定しています。1から9まで、お好きな数字を指定できます
<br>　またoutputにはtextfileを設定しています。clipboardにすると出題をクリップします。
<br>　因みに、Workflow Environment Variablesには、
<br>　出題を保存するテキストファイルのパスも書いていますが、
<br>　ホームディレクトリの"~"はスクリプト内でうまく動きませんでした
### 2.出題をクリップするためのロジック
　JSONフォーマットを作成すると同時に、$clipに出力用の文字列を作成しています
<br>　整形するために改行の"\n"や、表題、日時のヘッダーも込みで編集しています
<br>　
<br>　実際の出力は、個別のechoで実施しています
<br>　ScriptFilterでJSONフォーマット以外をechoするとエラーになるようなのですが、
<br>　下記のクリップボードコピーは問題ありませんでした
```
echo -e $clip|pbcopy
```
<br>　もちろん、標準出力でないファイルへの出力も問題ありません
```
　echo -e $clip >> ~/documents/alfred/nazo/nazonazo.txt
```
<br>　あと、Workflow Environment Variablesで指定したoutputを
<br>　スクリプト内で$outputとして参照して、出力をコントロールしています
### 3.初回にテキストファイルを作成する
　AlfredのWrite Text Fileでは、存在しないフォルダも作成してくれるオプションがあるのですが
<br>　今回はScript Filterから直でテキストファイルを出力するので、初回は存在しないフォルダを作成する必要があります
<br>　documentsフォルダー直下にテキストファイルを置くことでも解決できますが、
<br>　documents/alfred/xxxx/という風にワークフロー毎に保存フォルダーを作成するポリシーにしました
<br>　
<br>　そのため、ファイルの存在を確認してない場合は、フォルダーを作成します
<br>　フォルダーさえあればechoの追加">>"で、テキストファイルがない場合は作成、ある場合は追加という振る舞いになります
### 4.ランダムに9件を選んでAlfredフォーマットに出力する
　shufコマンドを利用します。ない方はインストールしてください
<br>　またインストール先をScript Filterの先頭部分のPATHに定義してください
<br>　（私はhomebrewでインストールしています）
<br>　
<br>　shufコマンドはシャッフルという意味です
<br>　下記は抽出したリンクの個数をシャッフルして、設定した出題数（Qmax）だけ取り出しています
```
　`seq 1 ${#link[@]}| shuf| head -n $Qmax`
```
#### 背景
　トリヴィアの泉のワークフローを作った時に、クイズの出力ができないかと思って作りました
<br>　あとは、1ページに多数の問題があるものを探して実装しました
#### 取扱説明
### 機能:
　ランダムになぞなぞを出題する
<br>　
### インストール:
　1.[Alfredworkflow](https://github.com/KitanoTamotsu/nazonazo/releases/download/1.1/nazonazo.alfredworkflow)をダウンロード 
<br>　2.ファイルをダブルクリックしてワークフローに登録
### 使い方:
　下記のキーワードで起動
<br>　　nazo1〜5（数字は難易度）
<br>　　nazoq （過去に出題したなぞなぞを表示）
#### 修正履歴
### ver1.1(2021-04-04)：
　シェルスクリプトをbashからzshに変更しました
<br>
<br>
[トップページに戻る](https://kitanotamotsu.github.io/)

