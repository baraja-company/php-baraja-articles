PHP function fopen()
====================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	en: php-function-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

The `fopen()` function represents low-level access to files on disk.

The programmer has to do everything himself (opening the file, reading the data, writing new data, closing the file).

If you just need to read and write files quickly, there are simpler options:

- <a href="/file-get-contents">file_get_contents()</a> - reading from a file
- <a href="/file-put-contents">file_put_contents()</a> - writing to a file

Basic usage
----------------

```php
$text = 'Any text to be saved...';

$file = fopen('file.html', 'a+'); // Opens file and mode

fwrite($file, $text); // Saves to file
fclose($file); // Closes the file
```

> If we open a file for reading and it is not closed, no other process can access it!

Types of file handling modes
----------------------------

We can work with files in different modes, which tell information about access rights.

For example, if we want to open a file for read-only, then the `r` mode is sufficient.

If we open the file for writing, then it will be marked as `open` on disk and another process (script) will not be able to write to it until we close it again. This ensures that the file will not be corrupted during writing.

| Mode | Meaning |
|-------|--------|
| `and` | Opens the file, if it does not exist it will be created |
| `a+` | Opens a file to add data and or read data, if it does not exist it will be created |
| `r` | Open read-only |
| | `r+` | Open for read and write |
| `w` | Open for writing, the original data will be deleted and replaced with new data, if it does not exist it will be created |
| `w+` | Open for write and read, original data will be deleted and replaced with new data, if it does not exist it will be created |
