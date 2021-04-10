PHP funkce imageaffinematrixget()
=================================

> id: f6c5dc12-4323-4f8d-a787-80ada3bdd13c
> slug:
> 	cs: funkce-imageaffinematrixget
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.5.0
/
function imageaffinematrixconcat(array $m1, array $m2) {}

/**
Return an image containing the affine tramsformed src image, using an optional clipping area`, `PHP 5.5.0`

Return an image containing the affine tramsformed src image, using an optional clipping area


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$type` | `int` |  |  |
| `$options` | `mixed` | null |  |


Návratové hodnoty
----------------


- resource|bool Return affined image resource on success or FALSE on failure.
/
function imageaffine($image, $affine, $clip = null) {}

/**
Concat two matrices (as in doing many ops in one go)
- array|bool Array with keys 0 to 5 and float values or <b>FALSE</b> on failure.
- array|bool Array with keys 0 to 5 and float values or <b>FALSE</b> on failure.

Další zdroje
------------


- https://www.php.net/manual/en/function.imageaffine.php
- https://www.php.net/manual/en/function.imagecreatetruecolor.php imagecreatetruecolor()}.</p>
- https://www.php.net/manual/en/function.imageaffinematrixconcat.php
- https://www.php.net/manual/en/function.imageaffinematrixget.php
