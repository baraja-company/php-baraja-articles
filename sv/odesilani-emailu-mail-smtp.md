Skicka e-post (mail() och SMTP-funktioner) i PHP
================================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	sv: skicka-e-post-mail-och-smtp-funktioner-i-php
> 
> perex:
> 	- 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> 	- 'E-postsändningsalternativ i PHP, mail(), SMTP, rubriker, konfiguration och Nette Mailer.'
> 
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

I PHP har vi i princip två sätt att skicka e-postmeddelanden:

- Den inhemska funktionen `mail()`, som har en del begränsningar,
- eller via en SMTP-server.

Funktionen `mail()` måste använda SMTP-servern, vilket är det enda sättet att skicka e-post via SMTP-servern.
---------------

Tanken med detta är enkel: du kallar funktionen:

```php
mail('jan@barasek.com', 'Ämne', 'Texten i meddelandet...');
```

Och PHP gör själva sändningen.

Internt fungerar sändningen genom att läsa konfigurationen från `php.ini` och leta efter standard-SMTP-servern för att leverera mailet genom. Detta kräver alltså en föregående konfiguration av webbservern.

Den största haken med funktionen `mail()` är att programmeraren måste räkna ut all logik själv. Detta innebär till exempel att man slänger rubriker om kryptering, kopplar certifikat för att kryptera meddelanden och så vidare.

Om sändningen misslyckas returneras ett "falskt" värde som vi måste fånga upp och bearbeta själva. Vi kan ta reda på det specifika felet på ett begränsat sätt genom att anropa `error_get_last()`, till exempel:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Kan inte skicka e-post:'
		. (@error_get_last()['meddelande'] ?? '')
	);
}
```

> **TIP:** Observera att vi inte har angett adressen från vilken vi vill skicka e-postmeddelandet och vilken kodning som ska användas.
>
> Alla dessa inställningar måste överföras via rubriker.

Om du fortfarande behöver använda funktionen `mail()` (t.ex. på grund av värdskap) rekommenderar jag att du använder paketet `nette/mail` och tjänsten `SendmailMailer`, som hanterar sändning av e-post på ett bra sätt.

SMTP-server
-----------

SMTP står för `Simple Mail Transfer Protocol`, vilket (som du snart kommer att se) är mycket riktigt.

SMTP är till skillnad från `mail()` ett mer avancerat protokoll med avancerade konfigurationsalternativ, inte bara från PHP-sidan utan även direkt på e-postservern.

SMTP-stödet på värdar är utmärkt 2018.

SMTP fungerar i princip så att PHP först upprättar en anslutning till SMTP-servern (det kräver tillägget `php_openssl.dll` i PHP, som du förmodligen redan har aktiverat), autentiserar (kontrollerar att inloggningsuppgifterna är korrekta) under anslutningen, och sedan kan vi kommunicera med servern på samma sätt som med en databas - dvs. skicka enskilda förfrågningar, men hålla en enda anslutning hela tiden. En stor fördel med SMTP är det direkta stödet för kryptering (känt som TLS).

Skicka e-post från localhost - en enkel lösning
--------------------------------------------------

Jag behöver ofta skicka e-post från localhost när jag testar en nyskriven applikation.

> **För tips:**
>
> På en Mac är situationen enkel eftersom MAMP-servern på något "magiskt" sätt hittar det Apple Mail-konto som är inloggat och meddelanden skickas alltid från det aktuella kontot.

Du kan dock inte alltid lita på detta beteende och det är en bra idé att skapa en egen lösning. Om du har en internetanslutning och ett Google-konto är det mycket enkelt att använda ett Gmail-konto som du kan ansluta till direkt från PHP och skicka e-post via det.

Om du använder paketet `nette/mail` är konfigurationen enkel:

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> Lösenordet är inte inloggningslösenordet för ditt konto (det skulle vara osäkert och du skulle till exempel inte kunna använda **tvåfaktorsautentisering**).
>
> Du måste använda ett så kallat "programlösenord", vilket innebär att du <a href="https://myaccount.google.com/apppasswords">registrerar din ansökan</a> direkt på ditt Google-konto, som tilldelas ett slumpmässigt genererat lösenord som du anger i PHP och som kan skickas via.
>
> Detaljerade instruktioner finns <a href="https://support.google.com/accounts/answer/185833?hl=cs">på Googles webbplats</a>.

Konfigurera e-post på Wedos
---------------------------

Du kan bara skicka 500 e-postmeddelanden per dag via Wedos hosting, och jag kämpade med SMTP-anslutningen ett tag.

Genom paketet `nette/mail` görs det så här (en fungerande lösning):

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

Parametern `host` är olika för varje webbhotell och finns i det e-postmeddelande som Wedos skickar när du registrerar webbhotellet.

Användarnamnet representerar den brevlåda från vilken e-postmeddelandena kommer att skickas. Brevlådan måste finnas. När vi skickar e-post i PHP måste vi också ställa in sändningen till samma adress (i Nette med metoden `->setFrom()`).

Om vi inte fyller i konfigurationen på ett korrekt sätt kommer olika felmeddelanden att visas och e-postmeddelandena kommer inte att kunna skickas.

När antalet skickade meddelanden överskrids kommer ett undantag att skickas ut som informerar om att gränsen har överskridits.
