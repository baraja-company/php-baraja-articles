Conditions and branching
========================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	en: conditions-and-branching
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

> **Warning:** This article was written many years ago and some information may be outdated or incorrect. Please bear this in mind when reading.

No more linear programs! The most basic principle of any program is "what happens when....". A condition can be written as a logical statement, which may be valid (the condition is satisfied) or invalid (then it is not executed or its exact opposite is executed). Both are easy to define.

General notation
------------

In general, a condition can be written as a logical statement. The condition may or may not be satisfied. It is a good idea to count both options as possible. If there are multiple alternatives, it is called a **nested condition**.

Example:

```php
if (operation value value) {
	// This will execute if the condition is true
} else {
	// This will execute if the condition does not hold
}
```

We don't always have to define both options (sometimes it's completely unnecessary). In fact, we can define the situation if only the condition holds. This is done as follows:

```php
if (operation value value) {
	// This is executed if the condition is true
}
```

Logical operators
--------------------------

| Operator | Meaning
|----------|---------
| `==` | Equals
| `===` | Equals and has the same data type (*anything can be compared to anything, but the condition is only satisfied if it is a value of the same data type (e.g. number, text, ...)*)
| `!=` | Does not equal
| `<=` | Equal to or greater than
| `>=` | Equal to or less than
| `<` | Greater
| `>` | Less

Real Example
--------------------------

````php
$a = 5;
$b = 3;
if ($a === $b) {
	// block to be printed if $a equals $b
} else {
	// block that is printed if $a does NOT equal $b
}
```

Nested conditions
--------------------------

Unfortunately, the output is only `true` (valid) and `false` (invalid). So if we want to consider multiple possibilities, we have to nest multiple conditions inside each other. This is called a **nested condition**. It is nested because one of the solutions to the condition is just another condition.

```php
$a = 5; // left pocket
$b = 3; // right pocket
$kapsa = true; // do I have a pocket?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'There's more in the left pocket';
	} else {
		echo 'There is more in the right pocket';
	}

} else {
	echo 'You have no pocket';
}
```
