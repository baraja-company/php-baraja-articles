Sessioner - servercookies i PHP
===============================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	da: sessioner-servercookies-i-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Ofte har vi brug for at gemme flere oplysninger i <a href="/cookies">cookies</a>, men den maksimale grænse for cookies er 4 kB, hvilket ikke er meget. Sessions løser dette problem ved at lagre dataene på webserveren og kun gemme en kort identifikator i klientens browser for at fortælle, hvilke data der tilhører hvilken klient.

Start af en session
---------------------

Før vi kan arbejde med sessioner, skal vi først starte dem. Dette gøres ved at kalde funktionen `session_start()` lige i starten af scriptet:

```php
session_start();
```

> Stærk advarsel: der må ikke udføres output til HTML-kode før kald af funktionen `session_start()`!

Sikkerhed i forbindelse med sessioner
-------------------

Indholdet af sessionen gemmes på serveren, og kun identifikatoren sendes til klientens browser, så brugeren kan ikke vide, hvad der er gemt i sessionen. Den eneste måde, scriptet kan påvirke brugeren på, er ved at slette identifikatoren (hvorefter scriptet genererer en ny).

Hentning af data fra en session
----------------------

Alle sessioner gemmes i den superglobale variabel `$_SESSION` og kan gennemløbes som et array.

F.eks. kan navnet på den aktuelt loggede bruger hentes ved at skrive:

```php
echo $_SESSION['bruger'];
```

Bemærk: Session eksisterer ikke altid (f.eks. hvis du er en ny bruger). Derfor bør vi altid kontrollere, om de findes, før vi opfører dem på listen, og om nødvendigt tilbyde en alternativ fejlmeddelelse.

```php
if (isset($_SESSION['bruger']) && $_SESSION['bruger']) {
    echo 'Logget ind som bruger:' . $_SESSION['bruger'];
} else {
    echo 'Der er ingen, der er logget ind.';
}
```

Lagring af data i session
----------------------

Gemning sker som en simpel lagring af data i en variabel:

```php
$_SESSION['bruger'] = 'Honzik';
```

Webserveren sørger for den tekniske sikring af korrekt lagring på serveren og for at sende identifikatoren til brugeren.

Sletning af sessioner
----------------

Vi kan slette de enkelte værdier separat i henhold til nøglen:

```php
unset($_SESSION['bruger']);
```

Eller alternativt alle tilgængelige sessioner:

```php
unset($_SESSION);
```

> Bemærk: Hvis du sletter en bestemt session, tømmes nøgleværdien ikke, men nøglen slettes helt. Derfor vil der blive givet en fejladvarsel, når der forsøges at læse en nøgle, der ikke eksisterer. Vi kan altid nemt verificere eksistensen af en nøgle med funktionen `isset()`.

Maksimal gyldighed af sessionen
---------------------------------

Hver gemte session har en grænse for, hvor længe den vil blive gemt på serveren. PHP indeholder direkte et cron-script, der med jævne mellemrum sletter gamle sessioner.

Standardværdien er normalt `1440 sekunder`, hvilket svarer til `24 minutter`.

Forøgelsen af værdien skal ske 2 steder:

- I <a href="/info">`php.ini` er den maksimale gyldighedslængde, som serveren vil opretholde, angivet</a>. Værdien er fastsat af direktivet `session.gc_maxlifetime`,
- På PHP-scriptets side skal du angive den aktuelle ønskede gyldighed.

Anvendelse i PHP:

```php
// serveren vil nu holde sessionen i op til 3600 sekunder = 1 time
ini_set('session.gc_maxlifetime', '3600');

// alle klienter (browsere) vil være
// session sendt med en gyldighed på præcis 3600 sekunder
session_set_cookie_params(3600);

session_start(); // vi kan starte sessionen!
```
