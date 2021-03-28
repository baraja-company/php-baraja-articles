PHP funkce stat()
================================

> id: "1239c03d-1724-4b24-9be8-57186c019217"
> slugCS: funkce-stat
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Gives information about a file


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | Path to the file. |


Návratové hodnoty
----------------

`array`

<table>
stat and fstat result
format
<tr valign="top">
<td>Numeric</td>
<td>Associative (since PHP 4.0.6)</td>
<td>Description</td>
</tr>
<tr valign="top">
<td>0</td>
<td>dev</td>
<td>device number</td>
</tr>
<tr valign="top">
<td>1</td>
<td>ino</td>
<td>inode number *</td>
</tr>
<tr valign="top">
<td>2</td>
<td>mode</td>
<td>inode protection mode</td>
</tr>
<tr valign="top">
<td>3</td>
<td>nlink</td>
<td>number of links</td>
</tr>
<tr valign="top">
<td>4</td>
<td>uid</td>
<td>userid of owner *</td>
</tr>
<tr valign="top">
<td>5</td>
<td>gid</td>
<td>groupid of owner *</td>
</tr>
<tr valign="top">
<td>6</td>
<td>rdev</td>
<td>device type, if inode device</td>
</tr>
<tr valign="top">
<td>7</td>
<td>size</td>
<td>size in bytes</td>
</tr>
<tr valign="top">
<td>8</td>
<td>atime</td>
<td>time of last access (Unix timestamp)</td>
</tr>
<tr valign="top">
<td>9</td>
<td>mtime</td>
<td>time of last modification (Unix timestamp)</td>
</tr>
<tr valign="top">
<td>10</td>
<td>ctime</td>
<td>time of last inode change (Unix timestamp)</td>
</tr>
<tr valign="top">
<td>11</td>
<td>blksize</td>
<td>blocksize of filesystem IO **</td>
</tr>
<tr valign="top">
<td>12</td>
<td>blocks</td>
<td>number of 512-byte blocks allocated **</td>
</tr>
</table>
* On Windows this will always be 0.
</p>
<p>
** Only valid on systems supporting the st_blksize type - other
systems (e.g. Windows) return -1.
</p>
<p>
In case of error, stat returns false.

Další zdroje
------------

https://php.net/manual/en/function.stat.php
