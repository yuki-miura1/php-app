# PHP App ① レビュー

## 全般

### 以下のaタグのリンクを押下した際にedit.phpの$_GETにどんな値が格納されるか説明してください。

```html
<a href="edit.php?todo_id=123&todo_content=焼肉">更新</a>
```
まず、指定先のURLに情報の続きがあるのでGETメソッドであると判断できます。そしてリンクを押すと"edit.php"にtodo_idは123、todo_contentに焼肉が格納されます。
### 以下のフォームの送信ボタンを押下した際にstore.phpの$_POSTにどんな値が格納されるか説明してください。

```html
<form action="store.php" method="post">
    <input type="text" name="id" value="123">
		<textarea　name="content">焼肉</textarea>
    <button type="submit">送信</button>
</form>
```
送信ボタンを押すとactionで指定したURL先"store.php"にPOSTメソッド方式で"123""焼肉"の値が分類されて送信されます。
"id"には"123""content"には"焼肉"が格納されていきます。
### `require_once()` は何のために記述しているか説明してください。
他ファイルで定義されている情報を読み込み使用できるようにするため。
()内にファイル先を指定します、指定先で定義されている情報が使用できるようになるため、再利用しやすくなる。重要な情報などもオープンにしないまま使用できるのでセキュリティ面も高い。onceで読み込みが重複していないか確認もしています、重複していた場合処理のバグが起きたりするらしいので未然ことができます。
### `savePostedData($post)`は何をしているか説明してください。
フォームデータを引数で受け取り、直前のパスを取得した変数を使って条件分岐をしています。
### `header('location: ./index.php')`は何をしているか説明してください。
locationでリダイレクトの指示をしています。今回はindex.phpに遷移するようにコードが書かれてます。
### `getRefererPath()`は何をしているか説明してください。
parse_urlで直前ページの情報を取得し$urlArrayに連想配列として格納してます。リターンで$urlArrayの['path']のバリューが返されてます。
### `connectPdo()` の返り値は何か、またこの記述は何をするための記述か説明してください。
まずtry文でPDOクラスのインスタンス化を行ってます。この際、config.phpで定義されているDSN,DB_USER,DB_PASSWORDの情報を使用してDBの接続を試みてます。接続に成功すればPDOクラスのオブジェクトが返ってきます。もし失敗すれば、例外処理が行われcatch文のメッセージが返ってきて処理を終了します。

### `try catch`とは何か説明してください。
例外処理をさせる際に使用する構文。
try文にはエラーが起こる可能性のある処理を書いていきます。問題なければそのまま実行されていきますが、万が一エラーが起きるとcatch文が実行されるようになってます。
### Pdoクラスをインスタンス化する際に`try catch`が必要な理由を説明してください。
起こりうるエラーが起きた時にエラーの理由が発見できるため。
どこのプログラムでエラーが起きたか解りやすくするためにcatch文毎に適したエラーメッセージを定義しておくことで早急な原因解消につながる。
## 新規作成

### `createTodoData($post)`は何をしているか説明してください。
new.phpからstore.phpに送信された値を$postの引数で受け取りcreateTodoData($post['content'])に渡します。
## 一覧

### `getTodoList()`の返り値について説明してください。
conection.phpで定義されているgetAllRecordsに、todosテーブルから削除されていないレコードがすべて呼び出されてます。その取得情報が返り値になってます。
### `<?= ?>`は何の省略形か説明してください。
echoの短縮形。
## 更新

### `getSelectedTodo($_GET['id'])`の返り値は何か、またなぜ`$_GET['id']` を引数に渡すのか説明してください。
返り値はGETメソッドに付いてたidのレコードのcontentカラムの値。
### `updateTodoData($post)`は何をしているか説明してください。
引数でフォームデータを受け取ってます。connectPdoが呼び出されデータベースに接続します。データベースのtodosテーブルのcontentカラムに対し$postのidと一致するレコードの値を更新してます。
## 削除

### `deleteTodoData($id)`は何をしているか説明してください。
todosテーブルの引数で受け取ったidのdeleted_atカラムに対し、現在の時刻を更新してます。
### `deleted_at`を現在時刻で更新すると一覧画面からToDoが非表示になる理由を説明してください。
遡ることgetAllRecordsではdeleted_atカラムの値がNULLのものすべてを取得するようになっているため。更新されているとでなくなります。
### 今回のように実際のデータを削除せずに非表示にすることで削除されたように扱うことを〇〇削除というか。
論理削除
### 実際にデータを削除することを〇〇削除というか。
物理削除
### 前問のそれぞれの削除のメリット・デメリットについて説明してください。
　倫理削除
・メリット
もとに戻す復元が可能
ログに残るので操作した情報が解る
・デメリット
データベースから削除はされないのでデータは蓄積し肥大化していく

　物理削除
・メリット
データベースからも完全に削除されるのでデータベースも軽くなる
・デメリット
復元ができない