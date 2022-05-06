Forms, form processing in PHP
=============================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	en: forms-form-processing-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

I suppose we have created an HTML form, which we send and now we want to process the data. There is a <a href="/formulare">separate article</a> about creating an HTML form.

Receiving data - different ways
----------------------------

The way the form is sent is set directly in the HTML

There are 2 options:

- **GET** - It is visible in the address bar after the question mark
 For example: `php.baraja.cz/search.php?query=formulare`
- **POST** - Hidden (not visible), most forms are sent via post

We then have to use the same method to read them in PHP.

Getting the data from the user and transferring it to the script
------------------------------------------------------

The basis is an HTML form, how to make it you can read <a href="/formulare">in a separate article</a>.

For starters, let's assume a simple form to enter the user's name:

```html
<form action="welcome.php" method="GET">
   Enter a name: <input type="text" name="username">
   <input type="submit" value="submit">
</form>
```


A text box for entering a name and a submit button will appear. When the button is clicked, the contents of the field are sent to the script `welcome.php`.

Now for the actual processing in the `welcome.php` file:

```php
$username = $_GET['username'];

echo 'The username entered is: ' . $username;
```


Note the special variable `$_GET`. This is a **superglobal variable** that contains data from the form and can be accessed as an array.

The problem with this solution, however, is that the received data is **not secure** and a similar form can be easily attacked. For example, a potential attacker can enter javascript code in the field instead of a name, which will be written to the page and executed.

Therefore, we must always sanitize any user data before outputting it into HTML code:

```php
$username = $_GET['username'] ?? 'Unknown';

echo 'The specified name is: ' . htmlspecialchars($username);
```

Further processing
----------------

We can do anything with the received data and treat it like any ordinary variable.

For example, add the value in two fields:

```php
echo $_GET['x'] + $_GET['y'];
```

Or save to file, database, email, ...

The following functions are useful for this:

- <a href="/file-put-contents">file_put_contents</a> - function to save data to a file
- <a href="/hashovani">MD5</a> - checksum calculation, for example for passwords
- <a href="/cookies">Cookies</a> - save data to **cookies** (small files inside the web browser)
