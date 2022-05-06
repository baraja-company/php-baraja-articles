Paginator and pagination of results in PHP
==========================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	en: paginator-and-pagination-of-results-in-php
> 
> perex: Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

When we have a lot of data to dump, it's polite to split it into multiple pages. This article does not address the practical implementation of passing page numbers and listing results, only the theoretical extraction of values and calculation of the optimal codebook to make browsing large numbers of pages as user-friendly as possible.

How many results do we have
----------------------

To start with, we need to find out how many results we have at all. If the data comes from a database, it can be very efficiently counted with the following SQL statement:

```sql
SELECT COUNT(*) FROM table
```


This calculation is very fast because the database keeps statistics in a helper file, so it doesn't touch the data at all.

If the data comes from elsewhere (and we have it in an array, for example), it can be counted with the count() function:

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'The array contains ' . count($cisla) . ' numbers;
```


Limiting the number of results
----------------------

Another problem is the limitation of the number of results. If the data is in the database, just put the `LIMIT` parameter in the SQL statement:

```sql
SELECT * FROM table WHERE (anything) LIMIT 10
```


This command will always get a maximum of 10 results, and it will also make the query faster because the database won't have to go through entire data files.

If we have data from another source (again an array), we can also limit the results at the PHP level using the `$iterator` helper variable:

```php
$field = [...];

$iterator = 0;
$limit = 10;
foreach ($field as $prvek) {
	// this is where the data is dumped

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Stops the loop when it has run 10 times
	}
}
```


Skipping the first X results
----------------------

When we are on the first page, it's pretty simple, you just need to limit the number of results using `LIMIT`. But what if I'm on the third page? Then we have to skip the first `X` results.

In SQL we have an elegant notation for this again:

```sql
SELECT * FROM table WHERE (anything) LIMIT 10 OFFSET 20
```


Skips the first 20 results and limits the next output to 10 results, so it outputs the interval `<21 - 30>`.

In pure PHP, this is handled in two ways.

If we know the indexes of the array, we can start reading it from a certain point (which is very fast):

```php
$field = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($field[$i])); $i++) {
	// this is where the data is dumped
}
```


However, for an unknown field, we have to use the iterator again and skip the items:

```php
$field = [...];

$iterator = 0;
$start = 20;
$limit = 10;
foreach ($field as $prvek) {
	if ($iterator < $start) {
		$iterator++;
		continue;
	}

	// somehow the data is being dumped here

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```


Displaying the optimal paginator/stepper
----------------------

Suppose we know the total number of items, the number of items on the page, and the current page number. Now we want to render a bar that will allow fast browsing of all pages with search results. However, since there are many pages (on the order of thousands), we can't list them all at once, so we have to intelligently choose some representative ones that best represent the range between pages.

It may look like this:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```


Assignment:

I am on page 36 of 72, how to optimally place the page numbers?
Well, through the sequence.

> **Tip:** By practical observation I found out that the left part of the Paginator should be calculated through an arithmetic sequence (so I can move linearly by the same number of steps) and the right part through a **geometric sequence**, which in turn makes it easy to make a big step. So if I want to get to a particular page, I first skip a large number of unnecessary items and then refine the selection by going back to the left.

Arithmetic sequence theory (we keep adding the same number):

```php
$d = 10; // step size
$a[1] = 1; // first element
$a[2] = $a[1] + $d; // second element
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // nth element

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```


Geometric sequence theory (we keep multiplying by the same number):

```php
$q = 10; // step size
$a[1] = 1; // first element
$a[2] = $a[1] * $q; // second element
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // nth element

function getGeometricItem(int $start, int $step, int $q): int
{
	return $start * pow($q, $step - 1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
