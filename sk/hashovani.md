Hashovanie reťazcov a hesiel
============================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	sk: hashovanie-retazcov-a-hesiel
> 
> perex: 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '5d1e289fd93e18ad73eb23ee1bbba8ee'

Proces hašovania (na rozdiel od šifrovania) vytvára zo vstupu výstup, z ktorého už nemožno odvodiť pôvodný reťazec.

Je preto vhodný na ochranu citlivých reťazcov, hesiel a kontrolných súčtov.

Ďalšou peknou vlastnosťou hašovacích funkcií je, že vždy generujú výstupy rovnakej dĺžky a malá zmena na vstupe vždy úplne zmení celý výstup.

Funkcie Hashing
----------------

V PHP existuje mnoho hash funkcií, dôležité sú:

- **Bcrypt: password_hash()** - Najbezpečnejšie hashovanie hesiel, výpočtovo pomalé, používa internú soľ a hashuje iteratívne.
- **md5()** - Veľmi rýchla funkcia vhodná na hashovanie súborov. Výstup je vždy 32 znakov.
- **sha1()** - Rýchla hash funkcia na hashovanie súborov, interne používaná systémom Git na hashovanie revízií. Výstup je vždy 40 znakov.

Hashing
-----------

```php
$password = 'secret-password';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Varning:** Ani `md5()`, ani `sha1()` nie sú vhodné na hashovanie hesiel, pretože je výpočtovo jednoduché zistiť pôvodné heslo alebo aspoň vopred vypočítať heslá. Oveľa lepšie je použiť `bcrypt`, ktorý bol vyvinutý na hashovanie hesiel.
>
> Webová stránka <a href="https://www.md5cracker.com/">md5cracker.com</a> obsahuje databázu kontrolných súčtov (hashov), skúste vyhľadať hash: `79c2b46ce2594ecbcb5b73e928345492`, ako môžete vidieť, takže čistý `md5()` nie je pre bežné slová a heslá až taký bezpečný.

Jediné správne riešenie: `Bcrypt + soľ`
--------------------------------------

V prednáške <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Ako si nepokaziť cieľovú rovinu</a> sa David Grudl venoval spôsobom správneho hashovania a ukladania hesiel.

Jediné správne riešenie je: `Bcrypt + soľ`.

Konkrétne:

```php
$password = 'hash';

// Generuje bezpečný hash
echo password_hash($password, PASSWORD_BCRYPT);

// Alternatívne s vyššou zložitosťou (predvolená hodnota je 10):
echo password_hash($password, PASSWORD_BCRYPT, ["náklady => 12]);
```

Výhodou systému Bcryp je najmä jeho rýchlosť a automatické solenie.

Skutočnosť, že generovanie trvá **dlho**, napríklad 100 ms, spôsobuje, že pre útočníka je testovanie mnohých hesiel veľmi nákladné.

Okrem toho sa výstupný hash automaticky spracúva pomocou **náhodnej soli**, čo znamená, že pri opakovanom hashovaní rovnakého hesla je výstupom vždy iný hash. Útočník preto nebude môcť použiť vopred vypočítanú hašovaciu tabuľku.

Preto nebudeme môcť overiť správnosť hesla opakovaným hashovaním, ale budeme musieť zavolať špecializovanú funkciu:

```php
if (password_verify($password, $hash)) {
    // Heslo je správne
} else {
    // Heslo je nesprávne
}
```

Solenie hesiel
------------

Na sťaženie prelomenia hash je dobré vložiť do pôvodného vstupu nejaký ďalší reťazec. Ideálne náhodný. Tento proces sa nazýva **solenie hesla**.

Zabezpečenie je založené na myšlienke, že útočník nebude môcť použiť vopred vypočítanú tabuľku hesiel a hashov, pretože nebude poznať soľ a bude musieť heslá prelomiť jednotlivo.

Napríklad:

```php
$password = 'secret_password';
$salt = 'fghjgtzjjhg';

$hash = md5($password . $salt);

echo $password; // vypíše pôvodné heslo
echo $hash;     // vypíše hash hesla vrátane soli
```

Zložené hash funkcie
------------------------

Možno si myslíte, že by bolo dobré vykonať hashovaciu funkciu opakovane, čím sa zvýši zložitosť jej prelomenia, pretože pôvodné heslo bude potrebné hashovať opakovane.

Napríklad:

```php
$password = 'heslo';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x zahashované cez md5()
```

Paradoxne, náročnosť prelomenia sa znižuje alebo zostáva takmer rovnaká.

Dôvodom je, že funkcia `md5()` je extrémne rýchla a na bežnom počítači dokáže vypočítať viac ako milión hashov za sekundu, takže postupné skúšanie hesiel sa veľmi nespomalí.

Druhý dôvod je skôr teoretický, a to možnosť, že dôjde k tzv. kolízii. Ak heslo hashujeme opakovane, môže sa stať, že časom narazíme na hash, ktorý útočník už pozná, a to mu umožní hashovať heslo pomocou databázy.

Preto je lepšie použiť pomalú bezpečnú hašovaciu funkciu a vykonať hašovanie iba raz, pričom konečný výstup sa stále ošetruje solením.
