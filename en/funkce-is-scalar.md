PHP function is_scalar()
========================

> id: b791a763-52ae-429f-bedb-a3c8aec73bb1
> slug:
> 	cs: funkce-is-scalar
> 	en: php-function-is-scalar
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '5bf11342-01a0-47e1-a6a8-c8c22bf92af9'
> sourceContentHash: d90130ec435901cd714af5928965cb8f

Availability in versions: `PHP 4.0.5`

Checks if the input is a scalar value.

Scalar variables are variables containing `integer` (integer), `float`, `string` or `boolean`. The `array`, `object` and `resource` types are not scalar.

Note: The value `null` is not scalar!

> **Note:**
>
> is_scalar() does not consider values of type `resource` to be scalar because it is an abstract data type (which may change over time). This implementation detail should not be relied upon as it may change.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$var` | `mixed` | *not* | Variable to be checked |


Return values
----------------

`bool`

Returns `true` if the input is scalar. Otherwise `false`.

Other resources
------------

[Official is-scalar documentation]([Official documentation](https://www.php.net/manual/en/function.is-scalar.php))
