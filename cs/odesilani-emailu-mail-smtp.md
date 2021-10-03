Odesílání e-mailů (funkce mail() a SMTP) v PHP
==============================================

> id: "3051b5ea-9408-4aaa-8209-e3c527ef8ad2"
> slug:
> 	cs: odesilani-emailu-mail-smtp
>
> perex: "Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer."
> publicationDate: "2019-11-26 12:00:02"
> mainCategoryId: "02d93aa8-ed83-460a-ab97-4de21118f019"

V PHP máme v podstatě 2 možnosti, jak odesílat maily:

- Nativní funkce `mail()`, která má relativně dost omezení,
- nebo prostřednictvím SMTP serveru.

Funkce `mail()`
---------------

Myšlenka použití je jednoduchá: Zavoláte funkci:

```php
mail('jan@barasek.com', 'Předmět', 'Text zprávy...');
```

A PHP samo zajistí odeslání.

Interně odeslání funguje tak, že se načte konfigurace z `php.ini` a hledá se výchozí SMTP server, přes který bude mail doručen. Vyžaduje to tedy předchozí konfiguraci webového serveru.

Hlavní úskalí funkce `mail()` je zejména v tom, že si programátor musí řešit celou logiku sám. Jde například o vyhození hlaviček ohledně kódování, napojení certifikátů pro šifrování zpráv a podobně.

V případě selhání odesílání se vrací hodnota `false`, kterou si musíme sami odchytit a zpracovat. Zjistit konkrétní chybu můžeme omezeně prostřednictvím volání `error_get_last()`, takže například:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Can not send mail: '
		. (@error_get_last()['message'] ?? '')
	);
}
```

> **TIP:** Všimněte si, že jsme funkci nikde nepředali adresu, z které chceme mail odesílat a jaké se má použít kódování.
>
> Veškeré toto nastavení je potřeba předávat prostřednictvím hlaviček.

Pokud funkci `mail()` musíte i přesto použít (například kvůli hostingu), doporučuji k odesílání použít balíček `nette/mail` a službu `SendmailMailer`, která problematiku odesílání mailů řeší dobře.

SMTP server
-----------

SMTP znamená `Simple Mail Transfer Protocol`, což (jak brzy uvidíte) je velká pravda.

SMTP je na rozdíl od funkce `mail()` vyspělejší protokol s pokročilou možností konfigurace nejen ze strany PHP, ale i přímo na poštovním serveru.

Podpora SMTP na hostinzích je v roce 2018 výborná.

SMTP v principu funguje tak, že se PHP nejprve naváže spojení se SMTP serverem (vyžaduje v PHP aktivní rozšíření `php_openssl.dll`, která pravděpodobně už aktivní máte), během připojování dojde k autentizaci (ověření správnosti přihlašovací údajů) a poté můžeme se serverem komunikovat podobně, jako s databází - tedy odesílat jednotlivé požadavky, ale držet si stále jedno spojení. Velká výhoda SMTP je přímá podpora šifrování (známé jako `TLS`).

Odesílání e-mailů z localhostu - jednoduché řešení
--------------------------------------------------

Často potřebuji z localhostu posílat e-maily, když testuji nově napsanou aplikaci.

> **Pro tip:**
>
> Na Macu je situace jednoduchá, protože MAMP server nějak "zázračně" najde vždy aktuálně přihlášený účet do Apple Mailu a zprávy jsou posílány z aktuálního účtu.

Na toto chování se ale vždy nelze spolehnout a je dobré si zřídit vlastní řešení. Pokud máte k dispozici připojení k internetu a účet na Googlu, tak lze velice jednoduše použít účet na Gmailu, na který se lze přímo z PHP připojit a maily posílat přes něj.

Pokud používáte balík `nette/mail`, tak je konfigurace jednoduchá:

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> Heslo není přihlašovací heslo k vašemu účtu (to by bylo nebezpečné a nešlo by použít například **dvoufázové ověření**).
>
> Je potřeba použít tzv. "heslo aplikace", což implementačně znamená, že si přímo v Google účtu <a href="https://myaccount.google.com/apppasswords">zaregistrujete vaši aplikaci</a>, které se přidělí nějaké náhodně vygenerované heslo, které zadáte do PHP a bude přes něj možno odesílat.
>
> Podrobný návod je <a href="https://support.google.com/accounts/answer/185833?hl=cs">na stránkách Googlu</a>.

Konfigurace mailů na Wedosu
---------------------------

Prostřednictvím hostingu Wedos můžete denně odeslat jen 500 mailů a s připojením na SMTP jsem chvíli zápasil.

Prostřednictvím balíku `nette/mail` se to dělá takto (funkční řešení):

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

Parametr `host` má každý hosting jiný a dá se dohledat v mailu, který Wedos posílá při registraci hostingu.

`username` představuje e-mailovou schránku, z které se budou maily odesílat. Schránka musí existovat. Při odeslání mailu v PHP musíme také nastavit odesilatete na tu samou adresu (v Nette metodou `->setFrom()`).

Pokud konfiguraci nevyplníme přesně a správně, budou vyhazovány různé chybové hlášky a maily nebude možné odeslat.

Po překročení počtu odeslaných zpráv se začne vyhazovat výjimka informující o překročení limitu.
