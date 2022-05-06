How to name variables, functions, methods and classes
=====================================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	en: how-to-name-variables-functions-methods-and-classes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

To keep the code in order, it is important to choose clear rules for how we derive names. This page serves as an overview of the relatively popular approaches used by a large number of programmers, including myself and people I work with.

If you work on a development team, by all means use their rules, but for general development it's just as beneficial to establish a few good habits.

Because the concept of the entire PHP syntax is really broad, I've divided the guide here into many categories that are highly specialized.

If you're looking for a quick solution, I recommend studying <a href="https://www.php-fig.org/psr/psr-4/">the PSR-4 standard</a>.

Inserting scripts, linking to HTML, loading files
---------------------------------------------------

Each script must start with the `<?php` tag.

If there is **no HTML** at the end of the file, it should not be terminated (to prevent a white character at the end of the page).

Loading other files should follow the following rules:

- If the file is not critical and can be dispensed with when rendering the page (for example, additional information in the footer), it should be loaded via include: `include 'file.php';`
- Critical files only via the require construct: `require 'file.php';`
- If we are working with objects, we use the require construct to load the autoloader, which will take care of loading the other classes itself (most frameworks implement this behavior).


If it is at least a little bit possible, we should separate the data retrieval logic from the render to HTML, i.e. use the MVC model (model - view - controller).

So we first prepare the data in, say, Presenter:

`Presenter.php`
```php
$cisla = [1, 2, 3];

$data = [];
$data['cisla'] = $cisla; // pass data to template
include 'renderCisel.php'; // load the template
```


And then render in the template:

`renderCisel.php`
```html
<table>
<?php
    foreach ($data['cisla'] as $cislo) {
        echo '<tr><td>' . $cislo . '</td></tr>';
    }
?>
</table>
```


This approach is used by most frameworks and is useful for increasing code clarity. In the later part of development, any experienced programmer who wants to develop clearly structured applications should think this way (for large applications, this approach is a must).

Variable names
----------------

If a variable contains an array of values or other objects, it should be named in the plural:

```php
$numbers = [1, 2, 3];
```


Because then we can simply iterate the values by a single number:

```php
foreach ($numbers as $number) {
    // processing numbers
}
```


A name composed of multiple words is concatenated into one long word with cameCase syntax, i.e. the first word starts with a lower case letter, each subsequent word with a capital letter:

```php
$variable = 'Hello!';
$username = [
   'Jan Barasek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // The PHP abbreviation has been reduced in characters
```


Function and method names
--------------------

Functions and methods should always make it clear in their name what they do. Often the expected input parameters and return value can also be included in the name.

Try to guess what the following functions do and what their return value is:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

The whole trick is in the first word in the name, which clearly tells you what method the function will use. Usually the following convention is followed:

- `get` - retrieve data as an array or object, input parameters specify the entity being searched for
- `save` - save to a file or database
- `create` - create an entity (for example, create an instance of an object)
- `set` - saving data to a predefined variable (inside a function)

Class names
----------

A class is a large entity that contains a large number of properties and methods, so it should also start with a capital letter. A class should also carry only one entity (and describe its properties), so it should be named with a singular name. If we need to work with multiple entities, we just store the instances in an array.

Example:

```php
class User
{
    public string $username;

    public string $password;

    public string $role;
}

class Users
{
    /** @var User[] */
    public array $users;

    public function addUser(User $user): void
    {
        $this->users = array_push($this->users, $user);
    }
}
```

The **User** class specializes only information about one particular user. If we want to work with multiple users, we create another class (envelope) that carries an array of instances of a specific entity.

Factories can also often be useful for this, as they allow us to easily create similar objects and recycle the original instances, resulting in clearer code while saving system resources.

Namespace - Namespaces
---------------------------

While Namespace is independent of the physical directory where the script is available, it is a good practice to at least partially respect the layout of the project (which leads to a better system for creating new names that is more unambiguous this way).

I personally name namespaces according to the common subdirectory for classes of that type.

Examples:

```php
App\Presenters; // This is how all presenters are named
App\Model; // This is the name of the generic model
App\Model\Math; // This is the name of the model that works with mathematics
```

For proper autoloading of classes, it is a good idea to follow the <a href="https://jakpsatphp.cz/PSR4/">PSR-4 standard</a>.

Custom conventions (for example, in a corporate team)
-----------------------------------------

And what do you name yours? I'd appreciate tips on how to improve this article.

In general, though, custom conventions within a team don't make much sense, because it makes the code harder to port to other frameworks and when you hire a new colleague, they have to learn the current conventions. It is therefore best to follow the `PSR-4` standard.
