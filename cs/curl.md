cURL v PHP - stahování dat přes URL
===================================

> id: "0bd1aed6-460d-4b63-9afe-f5087d1c6046"
> slug:
> 	cs: curl
>
> perex: Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> publicationDate: "2020-02-15 22:14:32"
> mainCategoryId: "02d93aa8-ed83-460a-ab97-4de21118f019"

PHP knihovna `cURL` představuje dobrý způsob, jak je možné stahovat data z cizího serveru.

Na základě dotazu sestaví HTTP request, který odešle na cílový server a po stažení obsahuje API pro (relativně) snadnou práci s daty.

Na rozdíl od nativní funkce `file_get_contents` (přes kterou můžeme také pokládat HTTP dotazy) nabízí mnohem lepší možnost konfigurace a stránky/soubory stahuje jako reálný prohlížeč.

> Funkce `file_get_contents` interně knihovnu `cURL` používá, jenom nemá tak podrobné možnosti konfigurace.

Detekce cURL módu v requestu
----------------------------

Často se hodí detekovat, jestli byl aktuální požadavek položen prostřednictvím `cUrl`, nebo klasicky v prohlížeči.

V PHP pro to neexistuje přímá implementace, ale můžeme si napsat jednoduchou funkci sami:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'curl');
}
```

Pokud disponujete Linuxem a jeho Terminálem, popřípadě jste na Macu, vyzkoušejte tento příkaz:

```shell
curl https://php.baraja.cz/curl
```

Příkaz provede interní request na tento web a vrátí výsledek.

Kdyby aplikace neobsahovala detekce cURL requestu, bude vráceno HTML, jako kdyby šel request z prohlížeče. Protože se ale typy requestů detekují, nic nebrání tomu, abychom vrátili očištěný článek v Markdownu.

Výhodou jsou pak mnohem lépe očištěná data. Uživateli v prohlížeči zobrazíme naformátované HTML, zatímco robotovy jen základní obsah.

Podrobné použití API v PHP
--------------------------

Pokud hledáte podrobné použití cUrlu, doporučuji si přečíst <a href="https://www.php.net/manual/en/book.curl.php">oficiální dokumentaci</a>, která je vždy aktuální.

Pro běžné použití existuje knihovna <a href="https://guzzle.readthedocs.io/en/stable/">**Guzzle**</a>, která řeší
