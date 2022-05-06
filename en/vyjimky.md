Exceptions and catching them in PHP
===================================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	en: exceptions-and-catching-them-in-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Exceptions are the tools of Object Oriented Programming, which provides an elegant way to throw and handle (treat) application errors.

An exception is first thrown (`thrown`), treated (`try`), and caught (`catch`). Only throwing is mandatory.

Philosophy of exception generation
-------------------------

Before the emergence of exceptions, error handling in programming was very complicated because we had to rely on the return values of functions and catch them in our own way and behave accordingly.

In fact, functions themselves do not enforce error handling, which can lead to fatal problems, but David has written about this in <a href="https://phpfashion.com/programatori-chyby-neignoruji">Programmers don't ignore errors</a>.

An example of forgotten error handling:

```php
// moving from disk to disk
copy('c:/oldfile', 'd:/newfile');
unlink('c:/oldfile');
// if the first operation fails, the file is irretrievably deleted
```

This is because the correct way to handle the output from `copy()` is to not continue and throw an error if an error occurs. In the case of good old functions, it might look like this:

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      return unlink('c:/oldfile');
   }

   return false;
}
```

Our `backup()` function will return `true` only if the `copy()` function did not fail and the `unlink()` function returned `true`. Otherwise, it returns `false`.

But is this now safe for the application? It is not! Because now we have to treat the output of the `backup()` function at the point where we call it, and if it fails, we won't even know why. In short, it will return `false` and we have to detect the error ourselves somehow. I guess in this case it's good to see that programmers often give up on error handling, or simply forget to handle something and the application throws hard-to-detect errors because of it.

The solution to this problem is to use exceptions that force the treatment, and if not treated, the application crashes completely and we always find out why.

Basic exception definition
--------------------------

In PHP, an exception is a special kind of interface implemented by the native `Exception` class that we will be using.

If the processing of some part of the program fails, we simply throw an exception describing the problem:

```php
if (copy('c:/oldfile', 'd:/newfile') === false) {
   throw new \Exception('Can not copy file "oldfile".');
}
```

Throwing an exception is done with the `throw` keyword, followed by creating an instance of the class with the exception. We can get an instance in other ways (for example, passing it from a variable), and creating an instance of an exception does not cause it to be thrown.

The first argument of the `\Exception` class constructor accepts the text of the exception, which should explain in a concise way what just happened. It is good practice to also include information about the operation being performed and a reference to the data. For example, if a file copy failed, it is good practice to pass the file name. If the SQL query execution fails, we again pass the query being executed. This will help us a lot later when handling errors, because we can see exactly what the problem is.

Exception handling
-----------------

For example, let's have a function `backup()` that backs up data and may throw a pair of errors:

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception('Can nto remove old file.');
      }
   }

   throw new \Exception('Can not copy backup files.');
}
```

Note that the function does not return any output, and we specify the `void` type in the definition. The function doesn't need to return anything, because a success is considered a state where no error is thrown and we don't need to treat a positive scenario.

If we were to use the function in an application without treatment, for example, as follows:

```php
echo 'Backing up files...';
backup();
echo 'Backup complete.';
```

This will work in the normal way. However, if an error occurs, the script will automatically terminate and output the exception text. The important thing is that it will not continue to execute the code and we know that no data corruption will occur.

If we want to continue execution, we need to **clean** the error, which we do by using the `try` and `catch` constructs:

```php
echo 'Backing up files...';
try {
   backup();
} catch (\Exception $e) {
   echo 'Backup failed: ' . $e->getMessage();
}
echo 'Backup complete.';
```

If an exception is thrown, the code in the `catch()` area (which accepts the exception by matching its data type) is called and the internal code is executed.

We always get an instance of the exception class, which can be used, for example, to display an error message, which is handled by the `getMessage()` method. It is also useful to know the `getFile()` method, which returns the disk path to the file containing the error, `getCode()`, which returns the error status code, and `getLine()`, which returns the line number where the exception was thrown.

Prepared exceptions
------------------------

In addition to the basic `\Exception` exception, PHP includes other predefined exception types that are suitable for different use cases.

| Data Type | Explanation |
|------------|-----------|
| `LogicException` | Logical error, predictable at program design |
| `BadFunctionCallException` | Function call error; function not found; call not allowed |
| `BadMethodCallException` | Method call error |
| `InvalidArgumentException` | Bad (invalid) argument passed to function or method |
| `OutOfRangeException` | Value out of range of array or collection |
| `LengthException` | Value exceeds allowed length |
| `DomainException` | Value does not fall within the required domain or range |
| `RuntimeException` | Error only detectable at runtime (for example, failure to write to a file) |
| `OverflowException` | Buffer or arithmetic operation overflow, often caused by processing more data than expected |
| `UnderflowException` | Underflow of a buffer or arithmetic operation, less data was passed than expected |
| `OutOfBoundsException` | Index out of range of array or collection |
| `RangeException` | Value not within the requested range |
| `UnexpectedValueException` | Unexpected value (e.g. return value of a function) |

The `LogicException` and `RuntimeException` exceptions should be prevented by proper program design. Personally, I use them only for exceptional situations, such as failure to write to a file and communication with an external service.

I recommend not catching `RuntimeException` at all and letting the application fail. This is usually a serious problem that should be reported as soon as possible.
