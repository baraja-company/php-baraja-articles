HTML forms - part in the browser
================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	en: html-forms---part-in-the-browser
> 
> perex: 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Before we can process any user data on the server side via PHP, we need to get it first. This is done in the browser via HTML forms that define the basic elements to receive the data. The purpose of this article is not to present all the possibilities of forms, but just the basic possibilities of accepting data and understanding the principle.

Basic HTML form source
-----------------------------

```html
<form action="script.php" method="get">

<!-- Here will be the entire content of the form -->

</form>
```


Each form starts with the HTML tag `<form>` and ends with the tag `</form>`. All form fields placed between these tags will be submitted.

Next, you need to set where to send the form with the `action` attribute (script name), and what method to use with the `method` attribute (GET or POST). If the method and destination are not specified, the form defaults to sending itself by the GET method.

Basic form fields
-------------------------

The most used field is used to get the text (string). Each field has its own type and name by which it can be recognized after submission.

Common text fields
------------------

Most importantly, I require a plain text field:

```html
<input type="text" name="food">
```

<input type="text" name="food">

Password field
---------------------

```html
<input type="password" name="password">
```

<input type="password" name="password">

Checkbox
--------

It is used to check the boolean (`TRUE` and `FALSE`):

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> Do you agree to the terms?
</label>

Radio button to select multiple options
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Czech
<input type="radio" name="language" value="sk"> Slovak
<input type="radio" name="language" value="en"> English
```


Allows you to select from several options. The selected option sends its value. By default it is good to select one field with the `checked="checked"` attribute:

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Czech
</label><br>
<label>
	<input type="radio" name="language" value="en"> Slovak
</label><br>
<label>
	<input type="radio" name="language" value="en"> English
</label>

Large text field
------------------

Created for entering multi-line text. It is also used to enter:

- `cols` ~ number of columns
- `rows` ~ number of rows

```html
<textarea name="article" cols="40" rows="6">
Hey guys!
</textarea>
```

<textarea name="article" cols="40" rows="6">
Hey guys!
</textarea>

Selectbox
---------

Presents a convenient way to select from many data.

```html
<select name="gender">
	<option value="man">Male</option>
	<option value="woman">Female</option>
</select>
```

When the form is submitted, the value in `value` is sent.

<select name="gender">
	<option value="man">Male</option>
	<option value="woman">Female</option>
</select>

Submit button
---------------------

The form can have an unlimited number of submit buttons. They are easy to enter:

```html
<input type="submit" value="Submit">
```

When clicked, it takes all the data from the form fields and sends it to the set script:

<input type="submit" value="Submit">

Data processing on the server
-------------------------

Next, you need to send the data to the server and process it there, this is covered in <a href="/processing-formula-in-php">the next article</a>.
