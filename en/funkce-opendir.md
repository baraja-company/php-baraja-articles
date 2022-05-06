PHP function opendir()
======================

> id: '9f9a1934-6b93-4105-ad55-59ab43dc74df'
> slug:
> 	cs: funkce-opendir
> 	en: php-function-opendir
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '445b5e899a056db4acbfb4840d674e94'

Availability in `PHP 4.0`

Open directory handle


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$path` | `string` | *not* | The directory path that is to be opened |
| `$context` | `resource` | null | For a description of the context parameter, refer to the streams section of the manual. |


Return values
----------------

`resource`

|bool a directory handle resource on success, or
false on failure.
</p>
<p>
If path is not a valid directory or the
directory can not be opened due to permission restrictions or
filesystem errors, opendir returns false and
generates a PHP error of level
E_WARNING. You can suppress the error output of
opendir by prepending
'@' to the
front of the function name.

Other resources
------------

[Official opendir documentation](https://www.php.net/manual/en/function.opendir.php)
