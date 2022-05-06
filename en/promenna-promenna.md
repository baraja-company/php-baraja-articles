Variables Variables
===================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	en: variables-variables
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

> **Warning:** This article was written many years ago and some information may be outdated or incorrect. Please bear this in mind when reading.

**Variables** are not intended for common deployment (they solve problems that can be solved in other ways), they are mainly used to make writes more concise and memory accesses more complex.

Consider the following example:

```php
$x = 25; // contains 25
$nacitana_promenna = 'x'; // contains "x"
$y = $$nacitana_promenna; // contains 25
echo $y; // prints 25
```


Note the two dollars following each other. In this case, the value of the variable $y will be loaded into the variable that has the name contained in the $nacitana_variable.

A little confusing, huh? That's why you'd better not use variables.
> **Note:** Variable variables are a specialty of PHP because of the dollar sign. In other languages, the beginning of a variable name is not marked with any character, so you cannot use variable variables because it would be ambiguous when it is a classical variable and when it is not.
