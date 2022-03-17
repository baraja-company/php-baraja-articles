PHP funkce syslog()
===================

> id: ba3cfc2b-2b9d-471d-ae94-edb470c3dbf7
> slug:
> 	cs: funkce-syslog
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Vytvoření zprávy systémového protokolu

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$priorita` | `int` | *není* | Priorita je kombinací zařízení a úrovně. Více info v tabulce níže. |
| `$message` | `string` | *není* | Zpráva, která se má odeslat, s tím rozdílem, že dva znaky %m budou nahrazeny řetězcem chybového hlášení (strerror) odpovídajícím aktuální hodnotě errno. |

Možné hodnoty argumentu `priority`
----------------------------------

Priority syslogu (v sestupném pořadí):

| Konstanta     | Popis |
|---------------|-------|
| `LOG_EMERG` | systém je nepoužitelný |
| `LOG_ALERT` | je třeba okamžitě přijmout opatření |
| `LOG_CRIT` | kritické stavy |
| `LOG_ERR` | chybové stavy |
| `LOG_WARNING` | varovné stavy |
| `LOG_NOTICE` | normální, ale významné podmínky |
| `LOG_INFO` | informační zpráva |
| `LOG_DEBUG` | zpráva na úrovni ladění |

Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.

Další zdroje
------------

[Oficiální dokumentace funkce syslog](https://www.php.net/manual/en/function.syslog.php)
