PHP funkce mail()
=================

> id: "70ed8e12-5546-4d17-9f56-1fd79574c8dd"
> slugCS: funkce-mail
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Send mail


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$to` | `string` |  | Receiver, or receivers of the mail. |
| `$subject` | `string` |  | Subject of the email to be sent. |
| `$message` | `string` |  | Message to be sent. |
| `$additional_headers` | `string` | null, | String to be inserted at the end of the email header. |
| `$additional_parameters` | `string` | null | The additional_parameters parameter can be used to pass additional flags as command line options to the program configured to be used when sending mail, as defined by the sendmail_path configuration setting. For example, this can be used to set the envelope sender address when using sendmail with the -f sendmail option. |


Návratové hodnoty
----------------

`bool`

true if the mail was successfully accepted for delivery, false otherwise.
</p>
<p>
It is important to note that just because the mail was accepted for delivery,
it does NOT mean the mail will actually reach the intended destination.

Další zdroje
------------

https://php.net/manual/en/function.mail.php
