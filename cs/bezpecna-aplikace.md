Bezpečná aplikace
=================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
>
> publicationDate: "2019-10-01 14:19:04"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

PHP je jedním z nejpopulárnějších programovacích jazyků používaných pro tvorbu webových aplikací. Nicméně, stejně jako většina programovacích jazyků, má i PHP své bezpečnostní problémy. Tyto bezpečnostní problémy mohou být způsobeny chybami v kódu, špatnými návyky při vývoji aplikací nebo nedostatečnou ochranou proti útokům z internetu.

V tomto článku se zaměříme na několik bezpečnostních hrozeb, kterým jsou PHP aplikace vystaveny, a na způsoby, jak se jim bránit. Rozdělíme tento článek do několika kapitol.

Ochrana před SQL Injection útoky
--------------------------------

SQL Injection je útok, při kterém útočník vloží do formuláře nebo URL řetězce SQL kód, který je následně spuštěn na databázi. Tento útok může způsobit vážné bezpečnostní problémy, jako jsou odcizení dat, modifikace dat nebo dokonce úplné smazání dat.

Nejlepším způsobem, jak se bránit proti SQL Injection útokům, je použití tzv. "prepared statements". Tyto prepared statements používají speciální syntaxi, která zamezuje spuštění nebezpečného SQL kódu v databázi. Kód vypadá následovně:

```php
$stmt = $pdo->prepare('SELECT * FROM users WHERE username = :username');
$stmt->execute(['username' => $username]);
$user = $stmt->fetch();
```

Pokud by v proměnné $username byl nebezpečný kód, jako například `"username' OR 1=1 --"` (který by vrátil všechny uživatele v databázi), prepared statement by tento kód nespustil a vrátil by prázdný výsledek.

Ochrana před XSS útoky
----------------------

XSS (Cross-Site Scripting) útoky jsou dalším bezpečnostním rizikem PHP aplikací. XSS útoky umožňují útočníkovi vložit do stránky nebezpečný kód, který může například odcizit uživatelská data nebo spustit nebezpečný kód na straně uživatele.

Proti XSS útokům se lze bránit pomocí sanitizace uživatelského vstupu. Toto znamená, že se vstupní data převedou na bezpečný formát, který neobsahuje nebezpečné kódy. Například, pokud uživatel zadá do formuláře `<script>alert("Hello");</script>`, sanitizace by toto vstupní pole převedla na bezpečný řetězec `"&lt;script&gt;alert(&quot;Hello&quot;);&lt;/script&gt;"`.

Existují různé funkce v PHP, které lze použít k sanitizaci uživatelského vstupu. Například funkce htmlspecialchars() převede speciální znaky HTML na bezpečné řetězce. Kód může vypadat následovně:

```php
$unsafeText = '<script>alert("Hello");</script>';
$safeText = htmlspecialchars($unsafeText, ENT_QUOTES, 'UTF-8');
echo $safeText;
```

Ochrana proti CSRF útokům
-------------------------

CSRF (Cross-Site Request Forgery) útoky jsou dalším bezpečnostním rizikem pro PHP aplikace. Tyto útoky umožňují útočníkovi využít relaci uživatele, aby provedl neoprávněnou akci na jiném serveru.

Proti CSRF útokům se lze bránit pomocí tzv. "tokenů". Tyto tokeny jsou náhodně generované řetězce, které jsou připojeny k formulářům a odesílány spolu s daty. Pokud token neodpovídá tokenu v relaci uživatele, formulář není odeslán.

Kód pro generování a ověřování tokenů může vypadat následovně:

```php
// generování tokenu
$token = bin2hex(random_bytes(32));
$_SESSION['csrf_token'] = $token;

// ověřování tokenu
if ($_POST['csrf_token'] !== $_SESSION['csrf_token']) {
    die('Invalid CSRF token');
}
```

Ochrana proti neoprávněnému přístupu
------------------------------------

Dalším bezpečnostním rizikem PHP aplikací je neoprávněný přístup. Útočník může získat přístup k citlivým datům nebo dokonce kód aplikace.

Proti neoprávněnému přístupu lze použít různé zabezpečení, jako například:

- Omezení přístupu k souborům pomocí práv a oprávnění
- Použití silného hesla a jeho pravidelná změna
- Použití dvoufaktorové autentizace
- Ochrana vývojového serveru a databáze pomocí firewallu

Aktualizace PHP a použitých knihoven
------------------------------------

Posledním důležitým faktorem pro bezpečnost PHP aplikací je aktualizace PHP a použitých knihoven. Nové verze PHP a knihoven obsahují často opravy bezpečnostních chyb a zranitelností. Pokud tedy používáte zastaralé verze PHP nebo knihoven, může vaše aplikace být ohrožena.

Je důležité pravidelně aktualizovat PHP a knihovny na nejnovější verze a sledovat bezpečnostní upozornění od vývojářů. Pokud vaše aplikace používá knihovny, které již nejsou udržovány, je vhodné najít alternativu nebo vytvořit vlastní řešení.

Závěr
-----

Bezpečnost PHP aplikací je klíčová pro ochranu citlivých dat a prevenci útoků. V tomto článku jsme se zaměřili na několik bezpečnostních hrozeb, kterým jsou PHP aplikace vystaveny, a na způsoby, jak se jim bránit. Ochrana před SQL Injection útoky, XSS útoky, CSRF útoky a neoprávněným přístupem mohou být dosaženy pomocí správného kódu a správného vývojového postupu. Navíc je důležité pravidelně aktualizovat PHP a knihovny na nejnovější verze a sledovat bezpečnostní upozornění od vývojářů. S těmito opatřeními můžete snížit riziko úspěšného útoku na vaši PHP aplikaci.
