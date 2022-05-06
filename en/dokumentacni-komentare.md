Documentary comments, Czech or English?
=======================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	en: documentary-comments-czech-or-english
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

Writing documentation takes time and often no one reads it after you, so it is a good practice to write comments directly in the source code. However, a lot of text unnecessarily clutters up the code, which then may not fit on the monitor at the same time, again reducing its readability.

Therefore, any code should be **self-explanatory**, i.e. when reading it, it should be immediately clear what it does, and it should do just one thing.

For example, if a function is named `getUserProfileById($id)`, it is crystal clear what such a function does and what return value we can expect. That's why comments are often used to describe only the input and output parameters + a short textual explanation of the principle (if something more complex is going on). When the code is intended for multiple people, it is polite to include the author and the date of creation.

```php
/**
 * Returns TRUE if the passed value is a valid IPv6 address
 * Alternative regular expression:
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @author Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```


It is immediately clear from the comment that the function expects an IPv6 address as input and returns a boolean `TRUE` or `FALSE` depending on how the validation goes.

The caller annotation is used to indicate values using standardized keys, which many editors can process further and, for example, hint parameters when calling the function or pass dependencies automatically.

Comments directly inside the code are used only in special cases. If the code needs an internal comment, this is usually an indication that it should be broken down into multiple smaller parts that will address their own piece of the program.

Languages
--------------

It's customary to always name classes, methods, functions and variables in English (to make the code readable for a large number of programmers), keys are relatively standardized with a uniform syntax, so language doesn't matter there either, and for auxiliary text it depends on the project usage. If it's a small project for a stable team of people, English is not necessarily a problem, on the contrary, at least everyone understands the description perfectly.

Motivation to use the annotation
-------------------

Special characters (call signs) can be noticed by other tools beyond the editor, so it is useful, for example, to put its tests (on what inputs I expect what output) directly above the function definition, thus never forgetting the place where the tests are written and at the same time every programmer has an immediate idea what the function does.

Here's a small example of what a test definition might look like (it's just a concept of notation, it's not a specific testing tool):

```php
Gets the values and forces a dump to the browser/console:
@test Name I---> $limit => {1-{5-8}}, $city => {Prague|Kladno|Brno|Plzeň} O---> [DUMP]

Generates a random hash of length 6 characters and checks if the response header is any except 201:
@test Name I---> $hash => {a-z0-9|len:6} O---> [HEADER:***|!HEADER:201]

Generates a pair of numbers and checks their sum on the output:
@test Number addition I---> $a => {1-5}, $b => {7-12} O---> ($return == $a+$b)

Retrieves a random article with ID between 1 and 30, verifies its length or non-emptyness:
@test Retrieving article text I---> $id => {1-30} O---> (strlen($return) > 64 || $return != NULL)

Generates a random IP address and checks if it is from the Czech Republic:
@test Geolocation I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['country'] == 'CS')

Attempts to retrieve the high ID article and winks at the modifiers (filters) while testing:
@test Non-existent article I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Returns the number of unicorn horns:
@test Unicorn I---> $maRohy => TRUE O---> 1

How many fingers am I pointing?
@test Fingers I---> $showing => {0-10|NAME:fingers} O---> ($return == {*|NAME:fingers})
```


Which would generally be written, for example, according to the rule:

```php
@test <test_name> I---> $<variable> => <value> O---> [<modifier>:<value>] (<value_expression>)
```


Inside the tests, I used a random value generator using regular expressions.
Here are some examples:

```php
{<value><diversity_direction>} {500+} {250-} {-200-(-50)}
{<start>-<end>} {0-5} {a-z} {a-f0-9}
{<Value1>|<Value2>|<Value3>} {Prague|Kladno|Brno}
{<expression>|<modifier>:<value>} {a-z0-9|en:6} {*|en:6}
{*|<modifier>:<value>} {*|randomWord:5}
```
