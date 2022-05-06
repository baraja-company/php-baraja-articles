PHP function date(), date and time
==================================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	en: php-function-date-date-and-time
> 
> perex: 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

The `date()` function is a tool for working with date and time. It is used in two cases:

- **Finding the current state**, i.e. the current date, time, ... and outputting it in a specific format,
- **Converting** a specific date into another format (for example, month number to name, year notation format, 12 and 24 hour system, ...).

Sample
------

```html
I am a special page. I know that right now is
<?php
    echo date('H:i'); // Hour:minute
?>
```

> WARNING: PHP does not print your time, but the time on the server. Therefore, you may get a different time than the one set on your computer.

Input syntax
--------------------------

The function is called in the normal way and the individual requests are entered as function arguments.

```php
echo date('formatting tags', specific time attribute);
```

The formatting tags indicate the format in which the date is printed. Marks can include spaces, periods, colons, hyphens, and other characters that are not themselves formatting marks (if you want to use a formatting mark, you must <a href="/carriage-notes">escape it</a>). An overview of each tag is below.

The second (optional) attribute indicates the manually entered date or time, which will be converted and output in the format according to the first parameter. It must be specified as a *timestamp* (can be obtained via the formatting tag "U").

Example:

```php
echo date('d. m. Y', 1405856605); // prints 07/20/2014
```

Table of allowed formatting tags
--------------------------

| Character | Description
|------|---------------------
| `Y` | Year as four digits (e.g. 1998)
| `y` | Year as a double digit (e.g. 98)
| `M` | English abbreviation of the name of the month (e.g. Jan)
| `m` | Month number (01-12)
| `F` | English month name (e.g. January)
| `D` | English abbreviation of day of the week (e.g. Fri)
| `l` | English name of the day of the week (e.g. Friday)
| `N` | Number of the day of the week (1 - Monday, 7 - Sunday)
| `w` | Number of the day of the week (0 - Sunday, 1 - Monday, 6 - Saturday)
| `d` | Day of the month (01-31)
| `j` | Number of day of the month (1-31)
| `z` | Day of the year (001-365)
| `H` | Hour (00-23)
| `h` | Hour (01-12)
| `i` | Minute (00-59)
| `s` | Second (00-59)
| `U` | *Timestamp:* Number of seconds since the beginning of time (since 1 January 1970)
| `S` | English ending of the ordinal number of the day of the month
| `A` | AM/PM indicator
| `a` | Morning/afternoon indicator (am/pm)
| `P` | Difference from Greenwich time (GMT) with separator between hours and minutes (added in PHP 5.1.3), for example: `+02:00`
| `g` | Hour in 12-hour format (1-12)
| `G` | Hour in 24-hour format (0-23)

Time formatting for sitemap
---------------------------------

Very often you need to format the time for the `sitemap.xml` file, which contains the date and time of the last change in the `<lastmod>` tag.

As of PHP 5.1.3, the following syntax can be used to do this:

```php
date('Y-m-d\TH:i:sP');
```

The date and time will be displayed to the nearest second and will also include information about the time zone where the server is located (the time zone is determined by the server's operating system).

Getting the Czech names of days and months
----------------------------------

It is not possible to get the Czech names of days and months in PHP in the normal way, so we have to write such values ourselves. The best way is to store the entries in an array and retrieve them using an index call.

```php
$months = [
    1 => 'January', 'February', 'March', 'April', 'May',
    'June', 'July', 'August', 'September', 'October',
    'November', 'December'
];

$days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
```

This example is a simplified example from <a href="https://php.vrana.cz">**Jakub Vrana**</a> from the article <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">Czech names for months and days of the week</a>.

Translating dates from English to English
--------------------------------------

Often we have a date in the format `Friday, 13 September` and we want to translate it into English, but how to do it? The best way is to call some function, for example `datumcesky()`, to which we pass the English date and it translates it.

```php
function datumCesky(string $date): string
{
    $men = [
        'January', 'February', 'March', 'April', 'May',
        'June', 'July', 'August', 'September', 'October',
        'November', 'December'
    ];

    $mcz = [
        'January', 'February', 'March', 'April', 'May',
        'June', 'July', 'August', 'September', 'October',
        'November', 'December'
    ];

    $date = str_replace($men, $mcz, $date);

    $day = [
        'Monday', 'Tuesday', 'Wednesday', 'Thursday',
        'Friday', 'Saturday', 'Sunday'
    ];

    $dcz = [
        'Monday', 'Tuesday', 'Wednesday', 'Thursday',
        'Friday', 'Saturday', 'Sunday'
    ];

    return str_replace($day, $dcz, $date);
}
```

Example usage:

```php
echo datumCesky('Friday, 13 September'); // Friday, 13 December
```

This function is definitely not ideal for translation, as it only replaces English words with Czech ones, but it may be sufficient for many deployments. For more advanced translations, you should always guarantee the exact syntax to translate into a uniform style, for example, `Friday, 13 December`.

Finding the first day of the month
-----------------------------

The first day of April 2018 was a Sunday, but how to find out easily?

As of PHP 5.1.0, there is a simple solution:

```php
echo date('N', strtotime('2018-04-01')); // 1 (Monday), 7 (Sunday)
```

In older versions, we have to implement the function ourselves:

```php
/**
 * @author Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Since the `w` modifier returns the output in US format, I still modify the day number by a simple calculation. The function returns an integer between 1 (Monday) and 7 (Sunday).

Time offsets / formatting conversion and date validation
--------------------------------------------------

Often we need to jump by a relative time (say +5 days), or pull a date from a user's text input to make it valid. To do this, the <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a> function allows the following syntax:

```php
echo strtotime('now');
echo strtotime('10 September 2000');
echo strtotime('+1 day');
echo strtotime('+1 week');
echo strtotime('+1 week 2 days 4 hours 2 seconds');
echo strtotime('next Thursday');
echo strtotime('last Monday');
```

The output of the function is **timestamp** *(universal time)* of the specified time as an integer.

> In general, I recommend to deploy this function on all forms where we work with time from the user in some way. It's certainly not a good idea to force the user to write the date in any particular format, but always create such a format automatically so the user can write whatever they want. Because often the text, for example, is copied from somewhere and it is too much work to reformat it manually when it can be done automatically.

Number of seconds from the start of time
--------------------------

Since January 1, 1970, every second is added to one to give a huge integer that indicates the time that has elapsed since then. This is particularly useful for simply calculating time differences. It is a fairly reliable solution, but it has its risks (the 2038 problem).

**General properties of this notation:**
- It's an integer, so it's easy to work with,
- It's in the decimal system, so it's easy for humans to read (except that you can't quickly tell what the exact time is),
- It can't be used to represent the period before January 1, 1970, because it can't be negative *(some new versions of the algorithm implement negative times, but you can't always rely on that)*,
- In 2038 it will stop working on 32 bit computers and will cause the program to crash (due to stack overflow and maximum integer size).

The 2038 problem
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">I'm not reading...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>

> The current time value is displayed above this text, which is incremented by +1 every second. The current value is loaded by javascript, so it may not always be accurate (it indicates your computer's system time).

Computers store integers in binary form, and if it is an integer, it often has 32 bits (32 ones and zeros). The highest possible 32-bit number is: `011 111 111 111 111 111 111 111 111 11`.

However, if we add +1 to this constant every second, we will one day get such a number *(it will be January 19, 2038 at 03:14:07)*. However, when we try to add another 1, the bit range will no longer be enough (the number would be larger than can be stored in 32 bits), so there will be a **stack overflow**, i.e. a 1 will be added to the beginning of the number, which in binary form means a **negative number**, (so this will probably cause the program to crash), getting us somewhere in the year 1901 (ugh!).

There are two solutions to this problem:

- Extending the integer range from 32 bits to 64 bits, giving us a span of several thousand years
- Use the `\DateTime` data type (commonly available in PHP), which provides an object-oriented approach to date management, is well compatible with the database, and offers a range of years from `0001` to `9999`, which is sufficient for the needs of most real-world applications.

Personally, I advocate using the `\DateTime` data type and not using integer storage at all.

Different time zones
-----------------

PHP is intelligent, so it always tries to use the current best time zone where the server is located. However, sometimes it may happen that the time zone is incorrectly specified, or we are programming a global application where users from all over the world access it and therefore we need to manually change the time zone.

This can be done globally for the entire PHP script by calling a function:

```php
date_default_timezone_set('UTC');
```

Occasionally, you may encounter this solution (detecting that the server has a time zone other than UTC):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Change date and time
--------------------------

It is not PHP itself that is responsible for getting the current date and time, but the operating system it is running on. So if you need to manually change the time, change it there.
