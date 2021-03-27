Sessions - serverové cookies v PHP
================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slugCS: sessions
> publicationDate: 2019-11-06 17:06:18
> mainCategoryId: 2a1ef8bc-14aa-438a-87e7-5b3f9643f325

Často potřebujeme do <a href="/cookies">cookies</a> uložit větší objem informací, nicméně maximální limit pro cookies je 4 kB, což není moc. Sessions tento problém řeší ukládáním dat na webový server a do prohlížeče klienta uloží jen krátký identifikátor, podle kterého pozná, jaká data patří jakému klientovi.

Nastartování sessions
---------------------

Před jakoukoli práci se sessions, je musíme nejprve nastartovat. To se dělá zavoláním funkce `session_start()` hned na začátku scriptu:

```php
session_start();
```

> Důrazné varování: Před zavoláním funkce `session_start()` nesmí proběhnout žádný výstup do HTML kódu!

Bezpečnost sessions
-------------------

Obsah sessions je uložen na serveru a do prohlížeče klienta se odesílá jen identifikátor, proto uživatel nemůže žádným způsobem zjistit, co je v sessions uloženo. Jediný způsob, kterým může script ovlivnit, je smazání identifikátoru (načež mu script vygeneruje nový).

Získání dat ze session
----------------------

Všechny session jsou uloženy v superglobální proměnné `$_SESSION` a lze je procházet jako pole.

Například jméno aktuálně přihlášeného uživatele můžeme získat zápisem:

```php
echo $_SESSION['user'];
```

Pozor: Session nemusí vždy existovat (například pokud jde o nově přicházejícího uživatele). Před jakýmkoli výpisem bychom tedy měli vždy zkontrolovat existenci a případně nabídnout alternativní chybové hlášení.

```php
if (isset($_SESSION['user']) && $_SESSION['user']) {
	echo 'Přihlášený uživatel: ' . $_SESSION['user'];
} else {
	echo 'Nikdo není přihlášený.';
}
```

Uložení dat do session
----------------------

Ukládání se provádí jako prosté uložení dat do proměnné:

```php
$_SESSION['user'] = 'Honzík';
```

O technické zajištění korektního uložení na server a odeslání identifikátoru uživateli se již postará webový server.

Smazání sessions
----------------

Jednotlivé hodnoty můžeme mazat samostatně podle klíče:

```php
unset($_SESSION['user']);
```

Nebo případně všechny dostupné sessions:

```php
unset($_SESSION);
```

> Pozor: Smazání konkrétní session nezpůsobí vyprázdnění hodnoty klíče, ale klíč kompletně smaže. Při pokusu o čtení neexistujícího klíče bude tedy vyhozeno chybové varování. Existenci klíče můžeme vždy snadno ověřit funkcí `isset()`.

Maximální délka platnosti session
---------------------------------

Každá uložená session má omezenou platnost, po jakou dobu bude uložena na serveru. PHP přímo v sobě obsahuje cron script, který staré sessions periodicky promazává.

Výchozí hodnota je obvykle `1440 sekund`, což je `24 minut`.

Navýšení hodnoty je potřeba provést na 2 místech:

- V <a href="/info">`php.ini` se nastavuje</a> maximální délka platnosti, kterou server udrží. Hodnotu nastavuje direktiva `session.gc_maxlifetime`,
- Na straně PHP scriptu je potřeba uvést aktuální požadovanou platnost.

Použití v PHP:

```php
// server nyní bude držet session s platností až 3600 sekund = 1 hodina
ini_set('session.gc_maxlifetime', '3600');

// všem klientům (prohlížečům) bude
// odeslána session s platností přesně 3600 sekund
session_set_cookie_params(3600);

session_start(); // můžeme nastartovat session!
```
