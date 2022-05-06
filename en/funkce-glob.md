PHP function glob()
===================

> id: '8c5ecbec-af3f-4ad5-8f3d-b8ff392f0d72'
> slug:
> 	cs: funkce-glob
> 	en: php-function-glob
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '5f4f1bd92ef2891b0bb427b292828fa2'

Availability in versions: `PHP 4.3.0`

Find pathnames matching a pattern


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$pattern` | `string` | *not* | The pattern. No tilde expansion or parameter substitution is done. |
| `$flags` | `int` | null | Valid flags: GLOB_MARK - Adds a slash to each directory returned GLOB_NOSORT - Return files as they appear in the directory (no sorting). When this flag is not used, the pathnames are sorted alphabetically GLOB_NOCHECK - Return the search pattern if no files matching it were found GLOB_NOESCAPE - Backslashes do not quote metacharacters GLOB_BRACE - Expands {a,b,c} to match 'a', 'b', or 'c' GLOB_ONLYDIR - Return only directory entries which match the pattern GLOB_ERR - Stop on read errors (like unreadable directories), by default errors are ignored. @return array an array containing the matched files/directories, an empty array if no file matched or false on error. |


Return values
----------------

`array`

an array containing the matched files/directories, an empty array
if no file matched or false on error.
</p>
<p>
On some systems it is impossible to distinguish between empty match and an
error.

Other resources
------------

[Official glob documentation](https://www.php.net/manual/en/function.glob.php)
