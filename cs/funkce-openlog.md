PHP funkce openlog()
====================

> id: ba2edc19-0df8-4eef-b8df-b8a0bb8aa9f1
> slug:
> 	cs: funkce-openlog
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4`, `PHP 5`, `PHP 7`

Otevře spojení do systémového loggeru.

Parametry
---------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$ident` | `string` | *není* | Identifikátor (typu `string`), který bude přidán do zprávy. |
| `$option` | `int` | *není* | Argument `$option` se používá k označení toho, jaké možnosti protokolování budou použity při generování zprávy protokolu. Podrobnosti níže. |
| `$facility` | `int` | *není* | Argument `$facility` se používá k určení, jaký typ programu protokoluje zprávu. To vám umožní určit (v konfiguraci syslogu vašeho stroje), jak budou zpracovávány zprávy přicházející z různých zařízení. Podrobnosti níže. |

`$option`
---------

Argument `$option` se používá k označení toho, jaké možnosti protokolování budou použity při generování zprávy protokolu.

Možné hodnoty:

| Konstanta   | Popis |
|-------------|-------|
| LOG_CONS    | Pokud při zápisu do loggeru (nebo při odeslání dat) došlo k chybě, vypíše hlášku do systémové konzole.
| LOG_NDELAY  | Ihned otevře spojení k loggeru.
| LOG_ODELAY  | (default) Zpoždění mezi zápisem jednotlivých zpráv. Používá se hlavně při zápisu první zprávy do logu.
| LOG_PERROR  | Vypíše zalogovanou chybu také jako standardní PHP chybu.
| LOG_PID     | Do zprávy zahrne číslo `PID`.

Můžete zvolit jednu nebo více možností.

Pokud používáte více možnosti, použijte zápis logický `OR` mezi konstantami.

Například: `LOG_CONS | LOG_NDELAY | LOG_PID`.

`$facility`
-----------

Argument `$facility` se používá k určení, jaký typ programu protokoluje zprávu. To vám umožní určit (v konfiguraci syslogu vašeho stroje), jak budou zpracovávány zprávy přicházející z různých zařízení.

| Konstanta    | Popis |
|--------------|-------|
| LOG_AUTH     | Bezpečnostní nebo autorizační zprávy. Pokud ve vašem systému není tato konstanta definována, použijte `LOG_AUTHPRIV`.
| LOG_AUTHPRIV | Bezpečnostní nebo autorizační zprávy. **Soukromá zpráva**.
| LOG_CRON     | Hodinové opakování (cron).
| LOG_DAEMON   | Systémový deamon, který čeká na události.
| LOG_KERN     | Zprávy z jádra operačního systému (z Kernelu).
| LOG_LOCAL0 ... LOG_LOCAL7 | Rezervováno pro použití na konkrétním lokálním zařízení nebo serveru. Není dostupné ve Windows.
| LOG_LPR      | Subsystem pro výpis do řádků.
| LOG_MAIL     | Subsystem pro obsluhu e-mailů a poštovního serveru.
| LOG_NEWS     | Subsystem pro obsluhu novinek a zpráv (`USENET`).
| LOG_SYSLOG   | Zprávy, které generuje interně syslogd (může obsahovat systémové zprávy z operačního systému).
| LOG_USER     | Výchozí zprávy, které definoval uživatel nebo konkrétní rozšíření. Nejsou systémové.
| LOG_UUCP     | Subsystem `UUCP`.

Návratové hodnoty
-----------------

`bool` - vrátí `true` v případě úspěchu, `false` v případě chyby.

Další zdroje
------------

[Oficiální dokumentace PHP.net](https://www.php.net/manual/en/function.openlog.php)
