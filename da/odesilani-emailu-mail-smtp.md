Afsendelse af e-mails (mail() og SMTP-funktioner) i PHP
=======================================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	da: afsendelse-af-e-mails-mail-og-smtp-funktioner-i-php
> 
> perex:
> 	- 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> 	- 'E-mailsendelsesmuligheder i PHP, mail(), SMTP, headers, konfiguration og Nette Mailer.'
> 
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

I PHP har vi grundlæggende 2 måder at sende mails på:

- Den oprindelige `mail()`-funktion, som har en del begrænsninger,
- eller via en SMTP-server.

Funktionen `mail()` skal bruge SMTP-serveren, hvilket er en meget enkel måde at sende post via SMTP-serveren på.
---------------

Idéen med at bruge dette er enkel: du kalder funktionen:

```php
mail('jan@barasek.com', 'Emneord', 'Teksten i meddelelsen...');
```

Og PHP sender selv forsendelsen.

Internt fungerer afsendelse ved at læse konfigurationen fra `php.ini` og finde den SMTP-server, som standard serveren skal levere mailen gennem. Så det kræver, at webserveren først konfigureres.

Den største faldgrube ved funktionen `mail()` er, at programmøren selv skal finde ud af al logikken. Dette indebærer f.eks. at man smider headere om kryptering væk, at man forbinder certifikater til at kryptere meddelelser osv.

I tilfælde af en fejl ved afsendelse returneres en "false"-værdi, som vi selv skal opfange og behandle. Vi kan finde ud af den specifikke fejl på en begrænset måde ved at kalde `error_get_last()`, så f.eks:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Kan ikke sende mail:'
		. (@error_get_last()['besked'] ?? '')
	);
}
```

> **TIP:** Bemærk, at vi ikke har angivet den adresse, hvorfra vi vil sende mailen, og den kodning, der skal bruges.
>
> Alle disse indstillinger skal overføres via headere.

Hvis du stadig har brug for at bruge funktionen `mail()` (f.eks. på grund af hosting), anbefaler jeg at bruge pakken `nette/mail` og tjenesten `SendmailMailer`, som håndterer forsendelse af post godt.

SMTP-server
-----------

SMTP står for `Simple Mail Transfer Protocol`, hvilket (som du snart vil se) er meget rigtigt.

SMTP er i modsætning til `mail()` en mere avanceret protokol med avancerede konfigurationsmuligheder, ikke kun fra PHP-siden, men også direkte på mailserveren.

SMTP-understøttelse på værter er fremragende i 2018.

SMTP fungerer grundlæggende ved, at PHP først opretter en forbindelse til SMTP-serveren (det kræver udvidelsen `php_openssl.dll` i PHP, som du sikkert allerede har aktiveret), autentificerer (kontrollerer, at loginoplysningerne er korrekte) under forbindelsen, og derefter kan vi kommunikere med serveren på samme måde som med en database - dvs. sende individuelle forespørgsler, men holde en enkelt forbindelse hele tiden. En stor fordel ved SMTP er den direkte understøttelse af kryptering (kendt som `TLS`).

Afsendelse af e-mails fra localhost - en enkel løsning
--------------------------------------------------

Jeg har ofte brug for at sende e-mails fra localhost, når jeg tester et nyligt skrevet program.

> **For tip:**
>
> På en Mac er situationen enkel, fordi MAMP-serveren på en eller anden måde "på magisk vis" finder den aktuelt loggede Apple Mail-konto, og beskederne sendes altid fra den aktuelle konto.

Du kan dog ikke altid stole på denne adfærd, og det er en god idé at opsætte din egen løsning. Hvis du har en internetforbindelse og en Google-konto, er det meget nemt at bruge en Gmail-konto, som du kan oprette forbindelse til direkte fra PHP og sende post via den.

Hvis du bruger pakken `nette/mail`, er konfigurationen enkel:

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> Adgangskoden er ikke loginadgangskoden til din konto (det ville være usikkert, og du kunne f.eks. ikke bruge **to-faktorgodkendelse**).
>
> Du skal bruge det, der kaldes et "programadgangskode", hvilket implementeret betyder, at du <a href="https://myaccount.google.com/apppasswords">registrerer dit program</a> direkte på din Google-konto, som tildeles en tilfældigt genereret adgangskode, som du indtaster i PHP og kan sendes igennem.
>
> Detaljerede instruktioner findes <a href="https://support.google.com/accounts/answer/185833?hl=cs">på Googles websted</a>.

Konfigurering af mail på Wedos
---------------------------

Du kan kun sende 500 e-mails om dagen via Wedos hosting, og jeg kæmpede med SMTP-forbindelsen i et stykke tid.

Gennem pakken `nette/mail` gøres det på følgende måde (en brugbar løsning):

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

Parameteren `host` er forskellig for hver hosting og kan findes i den e-mail, som Wedos sender, når du registrerer hosting.

`username` repræsenterer den postkasse, hvorfra mailsene vil blive sendt. Postkassen skal eksistere. Når vi sender en mail i PHP, skal vi også indstille send til den samme adresse (i Nette med metoden `->setFrom()`).

Hvis vi ikke udfylder konfigurationen præcist og korrekt, vil forskellige fejlmeddelelser blive sendt, og mails vil ikke kunne sendes.

Når antallet af sendte meddelelser overskrides, vil der blive sendt en undtagelse, der informerer om, at grænsen er overskredet.
