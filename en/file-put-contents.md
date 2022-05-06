File_put_contents
=================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	en: file-put-contents
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

The **file_put_contents** function is suitable for automatic writing to a file. Alternatively, you can also use <a href="/fopen">fopen()</a>, which I don't recommend for beginners.

Sample
--------------------------

```php
$file = 'file.txt';
$content = 'Content to be saved to file.';

file_put_contents($file, $content);
```

file_put_contents takes 2 parameters:

- `filename` where to write,
- the `content of the file` to write.

> Note: `file_put_contents()` overwrites the file with the latest contents.

Watch out for overwriting
--------------------------

If you save via file_put_contents, beware of overwriting the data. The function will delete all the current content and replace it with the new content. So if you just want to append the text, you can either add it to the beginning or to the end using your own script:

```php
$file = 'file.txt';
$content = 'New content.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

So first the file is opened, then the new content is written, and the original content is written after it...

If we want to add the old content before the new, we just need to modify the script slightly:

```php
$file = 'file.txt';
$content = New content.';

$oldContent = file_get_contents($file);
file_put_contents($file, $oldContent . $content);
```
