HTMLフォーム - ブラウザ上の部分
===================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	ja: htmlfomu-burauza-shangno-bu-fen
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- PHPによるフォームの処理、特にGETおよびPOSTメソッドを使用して取得したデータをサーバーに送信することが可能です。
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

サーバーサイドでPHPを使ってユーザーデータを処理する前に、まずデータを取得する必要があります。これは、データを受け取るための基本的な要素を定義したHTMLフォームを介して、ブラウザ上で行われます。この記事の目的は、フォームのすべての可能性を提示することではなく、データを受け入れ、原理を理解するための基本的な可能性だけを提示することである。

基本的なHTMLフォームのソース
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

各フォームは、HTMLタグ `<form>` で始まり、 `</form>` で終わります。これらのタグの間に配置されたすべてのフォームフィールドが送信されます。

次に、`action`属性でフォームの送信先（スクリプト名）を、`method`属性で送信方法（GETまたはPOST）を設定します。 送信方法と送信先を指定しない場合、フォームはデフォルトでGETメソッドで自身を送信します。

基本的なフォームフィールド
-------------------------

最も使用されるフィールドは、テキスト（文字列）を取得するために使用されます。各フィールドには、投稿後に認識できるような型と名前がついています。

一般的なテキストフィールド
------------------

最も重要なことは、プレーンテキストフィールドを要求することです。

```html
<input type="text" name="food">
```

<input type="text" name="food">のように入力します。

パスワード欄
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="パスワード">。

チェックボックス
--------

ブール値 (`TRUE` と `FALSE`) をチェックするために使用されます。

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> 規約に同意しますか？
</label>

ラジオボタンで複数選択可能
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

いくつかのオプションから選択できるようになっています。選択されたオプションは、その値を送信します。デフォルトでは、`checked="checked"`属性のフィールドを一つ選択するのがよいでしょう。

<label>
	<input type="radio" name="language" value="cz" checked="checked">チェコ語
<label><br>ラベル
<label>
	<input type="radio" name="language" value="en"> スロバキア語
<label><br>ラベル
<label>
	<input type="radio" name="language" value="en"> 英語
</label>

大きなテキストフィールド
------------------

複数行のテキストを入力するために作成されました。入力にも使用されます。

- cols` ~ カラムの数
- rows` ~ 行の数

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="article" cols="40" rows="6">。
やあ、みんな！
</textarea>。

セレクトボックス
---------

多くのデータから選択する便利な方法を提示します。

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

フォームを送信した後、`value`にある値が送信されます。

<select name="gender">
	<option value="man">男性</option>。
	<option value="woman">女性</option>。
</select

送信ボタン
---------------------

フォームには、送信ボタンを無制限に配置することができます。入力が簡単なのです。

```html
<input type="submit" value="Odeslat">
```

クリックされると、フォームフィールドからすべてのデータが取り込まれ、セットスクリプトに送信されます。

<input type="submit" value="Submit">のように入力します。

サーバーでのデータ処理
-------------------------

次に、データをサーバーに送ってそこで処理する必要がありますが、これについては<a href="/processing-formula-in-php">次回の記事</a>で説明します。
