Odosielanie e-mailov (funkcie mail() a SMTP) v PHP
==================================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	sk: odosielanie-e-mailov-funkcie-mail-a-smtp-v-php
> 
> perex: 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

V PHP máme v podstate 2 spôsoby odosielania e-mailov:

- Natívna funkcia `mail()`, ktorá má pomerne veľa obmedzení,
- alebo prostredníctvom servera SMTP.

Funkcia `mail()` musí používať server SMTP, čo je veľmi jednoduchý spôsob odosielania pošty cez server SMTP.
---------------

Myšlienka použitia tejto funkcie je jednoduchá: zavoláte funkciu:

```php
mail("jan@barasek.com, "Predmet, "Text správy...);
```

A PHP vykoná odoslanie samo.

Vnútorne odosielanie funguje tak, že sa načíta konfigurácia zo súboru `php.ini` a vyhľadá sa predvolený server SMTP, cez ktorý sa má pošta doručiť. Preto je potrebné najprv nakonfigurovať webový server.

Hlavným úskalím funkcie `mail()` je, že programátor musí sám vymyslieť celú logiku. To zahŕňa napríklad vyhadzovanie hlavičiek o šifrovaní, prepojenie certifikátov na šifrovanie správ a podobne.

V prípade zlyhania odoslania je návratová hodnota `false`, ktorú musíme zachytiť a spracovať sami. Konkrétnu chybu môžeme v obmedzenom rozsahu zistiť volaním funkcie `error_get_last()`, takže napríklad:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Nemožno odoslať poštu: '
		. (@error_get_last()['message'] ?? '')
	);
}
```

> **TIP:** Všimnite si, že sme nezadali adresu, z ktorej chceme odoslať poštu, a kódovanie, ktoré sa má použiť.
>
> Všetky tieto nastavenia sa musia odovzdať prostredníctvom hlavičiek.

Ak aj napriek tomu potrebujete používať funkciu `mail()` (napríklad kvôli hostingu), odporúčam použiť balík `nette/mail` a službu `SendmailMailer`, ktorá dobre zvláda odosielanie pošty.

Server SMTP
-----------

SMTP je skratka pre `Simple Mail Transfer Protocol`, čo (ako čoskoro uvidíte) je veľmi pravdivé.

SMTP je na rozdiel od `mail()` pokročilejší protokol s pokročilými možnosťami konfigurácie nielen na strane PHP, ale aj priamo na poštovom serveri.

Podpora SMTP na hostiteľoch je v roku 2018 vynikajúca.

SMTP v podstate funguje tak, že PHP najprv vytvorí spojenie so serverom SMTP (vyžaduje rozšírenie `php_openssl.dll` v PHP, ktoré už pravdepodobne máte aktívne), počas spojenia sa autentifikuje (overí správnosť prihlasovacích údajov) a potom môžeme so serverom komunikovať podobne ako s databázou - t. j. posielať jednotlivé požiadavky, ale stále udržiavať jedno spojenie. Veľkou výhodou protokolu SMTP je priama podpora šifrovania (známeho ako TLS).

Odosielanie e-mailov z localhostu - jednoduché riešenie
--------------------------------------------------

Často potrebujem odosielať e-maily z localhostu, keď testujem novo napísanú aplikáciu.

> **Na tip:**
>
> V počítači Mac je situácia jednoduchá, pretože server MAMP nejakým "zázračným" spôsobom nájde aktuálne prihlásené konto Apple Mail a správy sa vždy odosielajú z aktuálneho konta.

Na toto správanie sa však nemôžete vždy spoliehať a je dobré nastaviť si vlastné riešenie. Ak máte pripojenie na internet a účet Google, je veľmi jednoduché používať účet Gmail, ku ktorému sa môžete pripojiť priamo z PHP a posielať cez neho poštu.

Ak používate balík `nette/mail`, konfigurácia je jednoduchá:

mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl

> Heslo nie je prihlasovacie heslo do vášho účtu (to by bolo nezabezpečené a nemohli by ste napríklad použiť **dvofaktorové overovanie**).
>
> Musíte použiť takzvané "heslo aplikácie", čo implementačne znamená, že <a href="https://myaccount.google.com/apppasswords">registrujete svoju aplikáciu</a> priamo vo svojom účte Google, ktorému je priradené nejaké náhodne vygenerované heslo, ktoré zadáte do PHP a cez ktoré sa dá odoslať.
>
> Podrobné pokyny nájdete <a href="https://support.google.com/accounts/answer/185833?hl=cs">na webovej stránke spoločnosti Google</a>.

Konfigurácia pošty v systéme Wedos
---------------------------

Prostredníctvom hostingu Wedos môžete odoslať iba 500 e-mailov denne a chvíľu som bojoval s pripojením SMTP.

Prostredníctvom balíka `nette/mail` sa to robí takto (funkčné riešenie):

mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587

Parameter `host` je pre každý hosting iný a nájdete ho v e-maile, ktorý vám spoločnosť Wedos pošle pri registrácii hostingu.

Meno používateľa predstavuje poštovú schránku, z ktorej sa budú odosielať e-maily. Poštová schránka musí existovať. Pri odosielaní pošty v PHP musíme tiež nastaviť odoslanie na rovnakú adresu (v Nette pomocou metódy `->setFrom()`).

Ak nevyplníme konfiguráciu presne a správne, vyhodia sa rôzne chybové hlásenia a e-maily nebude možné odoslať.

Po prekročení počtu odoslaných správ sa vyhodí výnimka informujúca o prekročení limitu.
