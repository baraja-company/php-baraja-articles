Cookies v PHP
================================

> id: "392dd88b-d2f5-4943-a993-01aaad7ccd32"
> slugCS: cookies
> perex: "Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele."
> publicationDate: "2019-09-11 10:18:29"
> mainCategoryId: "2a1ef8bc-14aa-438a-87e7-5b3f9643f325"

> **Upozornění:** Tento článek byl napsán před mnoha lety a některé informace mohou být zastaralé, nebo nepravdivé. Při čtení na to, prosím, berte ohled.

Cookies jsou malé textové informace, uložené v prohlížeči návštěvníka webu. Přenáší se vždy s každou další načtenou stránkou, uživatel je může kdykoli odstranit, změnit a přečíst, proto se příliš nehodí k odkládání osobních údajů.

*Pozor: Pokud váš web používá cookies k sledování uživatelů, případně doplňky třetích stran (například Facebook like button, měřič návštěvnosti Google analytics, reklamní bannery), tak musíte o tomto uživatele informovat.*

> "Ještě poznámka: dokud souhlas nezískáte, neměla by vaše stránka obsahovat ani reklamu, ani měřící kódy. A to jako nasrat."
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Přečtení hodnoty z cookie
--------------------------

Všechny cookies jsou uloženy v superglobální proměnné `$_COOKIE`, která jednotlivé klíče ukládá jako pole.

Pokud jsme do cookies uložili například jméno aktuálně přihlášeného uživatele pod klíč `user`, tak jej získáme snadno:

```php
echo $_COOKIE['user'];
```

Pozor: Cookies nemusí vždy existovat (například pokud jde o nově přicházejícího uživatele). Před jakýmkoli výpisem bychom tedy měli vždy zkontrolovat existenci a případně nabídnout alternativní chybové hlášení.

```php
if (isset($_COOKIE['user']) && $_COOKIE['user']) {
	echo 'Přihlášený uživatel: ' . $_COOKIE['user'];
} else {
	echo 'Nikdo není přihlášený.';
}
```

Získání všech dostupných cookies
--------------------------------

Protože jsou všechny cookies uloženy v superglobální proměnné `$_COOKIE`, tak je možné je snadno vypsat:

```php
var_dump($_COOKIE);
```

Nebo případně projít cyklem a získat všechny klíče a hodnoty:

```php
foreach($_COOKIE as $key => $value) {
	echo $key . ': ' . $value;	// vypíšeme klíč a hodnotu
	echo '<br>';				// zalomíme řádek
}
```

Uložení hodnoty do cookie
--------------------------

Pro uložení dat do cookies se používá funkce `setcookie()`.

Prvním parametrem nastavíme klíč cookie, podle kterého ji přečteme z pole `$_COOKIE` a jako druhý parametr samotná data jako string.

Třetím parametrem můžeme (nepovinně) nastavit platnost, po které bude cookie dostupná. Čas dostupnosti se uvádí jako <a href="/date">timestamp</a>, pokud chceme tedy nastavit cookie s platností 1 hodina od tohoto okamžiku, tak stačí zapsat jen `time() + 3600`.

```php
$data = 'Nějaký obsah, který chceme uložit.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Uložení větších dat
-------------------

Cookies se pro ukládání rozsáhlejších dat nehodí (prohlížeče dovolují uložit obvykle jen 4 kB a maximálně 20 cookie, do velikosti se započítává také názvy cookies, nastavení platnosti a podobně).

Větší data je lepší uložit na server a do cookies jen identifikátor, podle kterého poznáme, k jakému uživateli patří. Této metodě se říká `$_SESSION` a pojednává o nich samostatný článek.

Pokud data nepotřebujete nutně skladovat vždy synchronní na serveru, tak lze použít uložiště **<a href="https://jecas.cz/localstorage">localstorage</a>**, které je k dispozici v javascriptu. Jeho kapacita je v řádu MB a data nepodléhají expiraci.
