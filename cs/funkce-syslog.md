PHP funkce syslog()
===================

> id: ba3cfc2b-2b9d-471d-ae94-edb470c3dbf7
> slugCS: funkce-syslog
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Generate a system log message


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$priority` | `int` |  | priority is a combination of the facility and the level. Possible values are: <table> syslog Priorities (in descending order) <tr valign="top"> <td>Constant</td> <td>Description</td> </tr> <tr valign="top"> <td>LOG_EMERG</td> <td>system is unusable</td> </tr> <tr valign="top"> <td>LOG_ALERT</td> <td>action must be taken immediately</td> </tr> <tr valign="top"> <td>LOG_CRIT</td> <td>critical conditions</td> </tr> <tr valign="top"> <td>LOG_ERR</td> <td>error conditions</td> </tr> <tr valign="top"> <td>LOG_WARNING</td> <td>warning conditions</td> </tr> <tr valign="top"> <td>LOG_NOTICE</td> <td>normal, but significant, condition</td> </tr> <tr valign="top"> <td>LOG_INFO</td> <td>informational message</td> </tr> <tr valign="top"> <td>LOG_DEBUG</td> <td>debug-level message</td> </tr> </table> |
| `$message` | `string` |  | The message to send, except that the two characters %m will be replaced by the error message string (strerror) corresponding to the present value of errno. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://php.net/manual/en/function.syslog.php
