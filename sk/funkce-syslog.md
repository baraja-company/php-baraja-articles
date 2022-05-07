PHP funkcia syslog()
====================

> id: ba3cfc2b-2b9d-471d-ae94-edb470c3dbf7
> slug:
> 	cs: funkce-syslog
> 	sk: php-funkcia-syslog
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: be95b04a388294d3b57547aa11553bf3

Dostupnosť v `PHP 4.0`

Vytvorenie správy systémového denníka

Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$priority` | `int` | *not* | Priorita je kombináciou zariadenia a úrovne. Viac informácií nájdete v tabuľke nižšie.
| `$message` | `string` | *not* | Správa, ktorá sa má odoslať, s tým rozdielom, že dva znaky %m budú nahradené reťazcom chybovej správy (strerror) zodpovedajúcim aktuálnej hodnote errno. |

Možné hodnoty pre argument `priorita`
----------------------------------

Priority syslogu (v zostupnom poradí):

| Konštanta | Popis |
|---------------|-------|
| `LOG_EMERG` | systém nepoužiteľný |
| `LOG_ALERT` | je potrebné okamžite konať |
| `LOG_CRIT` | kritické podmienky |
| `LOG_ERR` | chybové podmienky |
| | `LOG_WARNING` | varovné podmienky |
| `LOG_NOTICE` | normálne, ale významné podmienky |
| `LOG_INFO` | informačná správa |
| `LOG_DEBUG` | správa úrovne ladenia |

Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k syslogu](https://www.php.net/manual/en/function.syslog.php)
