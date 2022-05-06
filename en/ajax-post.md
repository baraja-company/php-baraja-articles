Processing ajax POST requests in PHP
====================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	en: processing-ajax-post-requests-in-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

While developing ajax Vue.js applications, after years I finally found out how to use ajax in PHP and how to receive data by POST method.

The superglobal variable `$_POST` is only available for forms
-------------------------------------------------------------

In PHP, the <a href="/superglobal-variable">superglobal variable `$_POST`</a> is commonly available to hold the submitted data from a form.

It is **relatively** easy to use.

On the HTML side, you need to create a form:

```html
<form action="process.php" method="post">
    Name: <input type="text" name="username">
    <input type="submit" value="Submit">
</form>
```

And then in the `process.php` file, the values can be accessed as array elements:

```php
<?php

echo htmlspecialchars($_POST['username'] ?? '');
```

> **Warning:**
>
> With this simple approach, everyone might feel that the data sent by POST is automatically defined as the indexes of the array in the `$_POST` variable. But this is not true!

In fact, the reason why data sent from a form using the POST method is written to the `$_POST` variable is because the browser automatically sends the HTTP header `'Content-Type': 'application/x-www-form-urlencoded'` when the HTML form is submitted.

Without a properly set header, the values simply cannot be accessed and we have to use a trick solution.

Sending data by ajax
-------------------

When trying to send data using ajax, we need to change the approach a bit on the PHP side. You can <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">read the discussion on Facebook</a> for details.

In javascript, for example, you can use the <a href="https://github.com/axios/axios">axios</a> library to send data using ajax. To use it easily, just link javascript from the CDN server and use it straight away:

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    axios.post('/api/form-process', {
        username: 'user name'
    })
    .then(response => {
        // Processing the response from the API
        alert(response.data.message); // Throws a message
    });
</script>
```

In this simple case, the URL `'/api/form-process'` is called using ajax and the POST method passes the object `{ username: 'User name' }`. The library itself already handles the logistics of passing the data automatically, so it is sent serialized as Json. In frontend developer parlance, this is called **json payload**.

On the PHP side, I would expect to use it as a form (after all, it was a POST method):

```php
<?php

echo htmlspecialchars($_POST['username'] ?? '');
```

But in fact, in this case the `$_POST` field will be empty and no data will be passed. The `$_POST` variable is only used for data retrieved from forms (you can tell by the HTTP header we didn't throw away).

So in this case we need to get the data directly from the HTTP request, the **trick solution** is used for that. It is then easy to use:

```php
<?php

$data = json_decode(file_get_contents('php://input'), true);

echo htmlspecialchars($data['username'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    'message' => 'Server bug',
]);
die;
```

As part of the example, I also provide a simple javascript response. The important thing is to properly throw the HTTP header `'Content-Type: application/json'` and exit the script after all data has been sent.

Enforcing the use of `$_POST`
-------------------------

If you still want to treat the submitted data directly as a form, there is a way to transfer it. In this case, you need to modify the creation of the ajax query itself and pass the HTTP headers correctly:

```js
axios.post(
    '/api/form-process',
    {
        username: 'User name'
    },
    {
        headers: { 'content-type': 'application/x-www-form-urlencoded'}
    }
).then(response => {
    // Some processing
});
```

In this case, however, the processing on the PHP side will not be pleasant, because we still have to correct the data and convert it to an array.

I managed to come up with this horror for that:

```php
<?php

if (\count($_POST) === 1
    && preg_match('/^{.*}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['username'] ?? '');
```

However, it is much better to stay with the first access and get the data using the `'php://input'` method.
