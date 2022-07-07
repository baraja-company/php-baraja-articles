HTML表格--浏览器中的一部分
================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	zh: html-biao-ge-liu-lan-qi-zhong-de-yi-bu-fen
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- 在PHP中处理表单，特别是使用GET和POST方法将获得的数据发送到服务器的可能性。
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

在我们通过PHP在服务器端处理任何用户数据之前，我们需要先得到它。这是在浏览器中通过HTML表单完成的，这些表单定义了接收数据的基本元素。本文的目的不是要介绍所有形式的可能性，而只是介绍接受数据和理解原理的基本可能性。

基本的HTML表格来源
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

每个表格以HTML标签`<form>`开始，以标签`</form>`结束。放在这些标签之间的所有表格字段都将被提交。

接下来，你需要用`action`属性（脚本名称）设置表单的发送位置，用`method`属性设置发送方法（GET或POST）。 如果你没有指定方法和目的地，表单默认为用GET方法发送自己。

基本表单字段
-------------------------

最常用的字段是用来获取文本（字符串）的。每个字段都有自己的类型和名称，提交后可以通过它来识别。

常见的文本字段
------------------

最重要的是，我需要一个纯文本字段。

```html
<input type="text" name="food">
```

<input type="text" name="food" >

密码字段
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">。

复选框
--------

它用于检查布尔值（`TRUE`和`FALSE`）。

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="check"> 您是否同意条款？
</label>

单选按钮可选择多个选项
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

它允许你从几个选项中进行选择。所选的选项会发送其值。默认情况下，选择一个带有`checked="checked"`属性的字段是好的。

<label>
	<input type="radio" name="language" value="cz" checked="check" > 捷克语
</label><br>
<label>
	<input type="radio" name="language" value="en"> Slovak
</label><br>
<label>
	<input type="radio" name="language" value="en"> 英语
</label>

大型文本字段
------------------

为输入多行文本而创建。它也被用来进入。

- `cols` ~ 列的数量
- `rows` ~ 行数

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="文章" cols="40" rows="6">
嘿，伙计们!
</textarea>

选择框
---------

提出了一种从许多数据中进行选择的方便方法。

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

提交表格后，`value'中的值被发送。

<select name="gender">
	<option value="man">男性</option>。
	<option value="woman">Female</option>。
</选择> </选择>

提交按钮
---------------------

该表格可以有无限数量的提交按钮。他们很容易进入。

```html
<input type="submit" value="Odeslat">
```

当点击时，它从表单字段中获取所有数据，并将其发送到设定的脚本。

<input type="submit" value="submit">。

服务器上的数据处理
-------------------------

接下来，你需要将数据发送到服务器并在那里进行处理，这将在<a href="/processing-formula-in-php">下一篇文章</a>中介绍。
