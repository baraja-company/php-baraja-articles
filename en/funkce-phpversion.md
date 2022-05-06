PHP function phpversion()
=========================

> id: '9f5d1018-f604-453c-91f3-062e8d9c32b3'
> slug:
> 	cs: funkce-phpversion
> 	en: php-function-phpversion
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: e7bab90ebc59b3f41c781ff4f1f6aa78

Availability in `PHP 4.0`

Gets the current PHP version


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$extension` | `string` | null | An optional extension name. |


Return values
----------------

`string`

If the optional extension parameter is
specified, phpversion returns the version of that
extension, or false if there is no version information associated or
the extension isn't enabled.

Other resources
------------

[Official phpversion documentation](https://www.php.net/manual/en/function.phpversion.php)
