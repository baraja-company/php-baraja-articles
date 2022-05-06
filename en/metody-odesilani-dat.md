Data sending methods (GET and POST)
===================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	en: data-sending-methods-get-and-post
> 
> perex: 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '2282538597ebfe95877ae0e005ddd352'

In addition to regular variables, we also have so-called **superglobal variables** in PHP, which carry information about the currently called page and the data we pass.

Typically, we have a form on a page that the user fills in and we want to transfer that data to the web server where we process it in PHP.

There are 3 methods most commonly used to do this:

- `GET` ~ the data is passed in the URL as parameters
- `POST` ~ the data is passed covertly along with the page request
- <a href="/ajax-post">Ajax POST</a> ~ asynchronous javascript processing

GET method - `$_GET`
--------------------

The data sent by the GET method is visible in the URL (as parameters after the question mark), the maximum length is 1024 characters in Internet Explorer (other browsers *almost* do not restrict it, but larger texts should not be passed this way). The advantage of this method is mostly simplicity (you can see what you are sending) and the possibility to provide a link to the result of the processing. The data is sent to a variable.

The address of the receiving page might look like this:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

In PHP, we can then, for example, write the value of the `variable` parameter as follows:

```php
echo $_GET['variable']; // prints the "contents"
```

> **Strong warning:** This method of outputting data directly to an HTML page is not safe, because we can pass, for example, HTML code in the URL that would be written to the page and then executed.
>
> We must **always** treat the data before any output to the page, the `htmlspecialchars()` function is used for this.
>
> For example: `echo htmlspecialchars($_GET['variable']);`

POST method - `$_POST`
----------------------

The data sent by the POST method is not visible in the URL, which solves the problem of maximum length of the data sent. The POST method should always be used to send form fields, as this will ensure that, for example, passwords are not visible and that a link cannot be provided to the page processing the result of a particular input.

The data is available in the `$_POST` variable and the usage is the same as for the GET method.

Verifying the existence of the data sent
--------------------------------

Before processing any data, we should first verify that the data has actually been sent, otherwise we would be accessing
 to a non-existent variable, which would throw an error message.

The `isset()` function is used to verify the existence of a variable.

```php
if (isset($_GET['name'])) {
    echo 'Your name: ' . htmlspecialchars($_GET['name']);
} else {
    echo 'No name was entered.';
}
```

Data entry form
------------------------

The form is made in HTML, not in PHP. It can be on a plain HTML page. All the "magic" is handled by the PHP script that accepts the data.

For an example, we can use a form to receive 2 numbers, sent by the GET method:

```html
<form action="script.php" method="get">
    First number: <input type="text" name="x">
    Second number: <input type="text" name="y">

    <input type="submit" value="Add numbers">
</form>
```

In the first line you can see where the data will be sent, and by what method.

The next 2 lines are simple form elements, note the **name=""** attribute, this is where you write the name of the variable that will hold what is now in the form.

Next comes the button to submit the data (mandatory) and the form's closing HTML tag (mandatory so the browser knows what else to submit and what not).

> We can have any number of forms on one page, and they cannot be nested. If nesting occurs, the most nested form is always sent and the rest are ignored.

Form processing on the server
-------------------------------

Now we have a finished HTML form and we send it to `script.php`, which receives the data using the GET method. The address of the page request might look like this:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x']; // 5
$y = $_GET['y']; // 3

echo $x + $y; // prints 8
```

Correctly, we should first verify that both form fields have been filled, this is done with the `isset()` function:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x']; // 5
    $y = $_GET['y']; // 3

    echo $x + $y; // print 8
} else {
    echo 'The form was not filled in correctly.';
}
```

> **TIP:** You can pass multiple parameters to the `isset()` construct to verify that they all exist.
>
> Therefore, instead of `isset($_GET['x']) && isset($_GET['y'])`, you can just specify:
>
> `isset($_GET['x'], $_GET['y'])`.

Processing data received by the POST method
--------------------------------------

If data is received by POST method, the URL of the script to be processed will always look like this:

`https://________.com/script.php`

And never otherwise. Just no. The data is hidden in the HTTP request and we can't see it.

> The hidden POST method is required to send usernames and passwords for security reasons.
>
> **Security:** If you are working with passwords on your site, the login and registration form should be hosted on HTTPS and you must hash the passwords appropriately (for example, with BCrypt).

Handling ajax requests
------------------------------

In some cases, when processing ajax requests, it may not be easy to retrieve the data. The reason is that ajax libraries usually send data as `json payload`, while the superglobal variable `$_POST` contains only form data.

The data can still be accessed, I described the details in the article <a href="/ajax-post">Handling ajax POST requests</a>.
