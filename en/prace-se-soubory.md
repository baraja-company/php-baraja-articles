Working with files
==================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	en: working-with-files
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

There are a number of functions in PHP for working with files.

- <a href="/fopen">fopen()</a>, low-level access to files on disk
- <a href="/file-get-contents">file_get_contents()</a>, retrieve the contents of a file or URL
- <a href="/file-put-contents">file_put_contents()</a>, saving a string to a local file.

Disk functions
--------------

- `unlink($path)`, delete file
- `copy($from, $to)`, copy a file from one location to another (can be from a URL to the local disk)
- `rename()`, rename or move a file on disk
- `chmod()`, change permissions to read, write or execute a file
