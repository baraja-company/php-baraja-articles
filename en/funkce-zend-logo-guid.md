PHP function zend_logo_guid()
=============================

> id: '180db505-820c-43dc-bc26-005d2226ea17'
> slug:
> 	cs: funkce-zend-logo-guid
> 	en: php-function-zend-logo-guid
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '11a44dd0cdfe61f2ca3752dfd3a49562'

Availability in `PHP 4.0`

> **Warning:**
>
> This feature is deprecated and no longer available as of PHP 5.

This function returns an ID that can be used to display the Zend logo using the built-in image.

For example:

```php
echo '<img src="' . $_SERVER['PHP_SELF']
   . '?=' . zend_logo_guid() . '" alt="Zend Logo!">';
```

Alternatively, you can use the `php_logo_guid()` function.

Parameters
--------------

The function has no input parameters.

Return values
----------------

`string`

Returns the value `PHPE9568F35-D428-11d2-A769-00AA001ACF42`.
