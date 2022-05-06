PHP function syslog()
=====================

> id: ba3cfc2b-2b9d-471d-ae94-edb470c3dbf7
> slug:
> 	cs: funkce-syslog
> 	en: php-function-syslog
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: be95b04a388294d3b57547aa11553bf3

Availability in `PHP 4.0`

Creating a system log message

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$priority` | `int` | *not* | Priority is a combination of device and level. See table below for more info. |
| `$message` | `string` | *not* | The message to be sent, except that the two %m characters will be replaced by the error message string (strerror) corresponding to the current errno value. |

Possible values for the `priority` argument
----------------------------------

Syslog priorities (in descending order):

| Constant | Description |
|---------------|-------|
| `LOG_EMERG` | system unusable |
| `LOG_ALERT` | immediate action is required |
| `LOG_CRIT` | critical conditions |
| `LOG_ERR` | error conditions |
| | `LOG_WARNING` | warning conditions |
| `LOG_NOTICE` | normal but significant conditions |
| `LOG_INFO` | information message |
| `LOG_DEBUG` | debug level message |

Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official syslog documentation](https://www.php.net/manual/en/function.syslog.php)
