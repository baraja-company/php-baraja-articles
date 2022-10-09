Cookies i PHP
=============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	da: cookies-i-php
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- 'En cookie er et kort stykke tekstinformation i browserens hukommelse, hvor vi kan skrive og markere brugeren i PHP.'
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **Varsel:** Denne artikel blev skrevet for mange år siden, og nogle oplysninger kan være forældede eller forkerte. Vær opmærksom på dette, når du læser.

Cookies er små stykker tekstinformation, der gemmes i en webstedsbesøgers browser. De overføres altid med hver side, der indlæses igen, og kan til enhver tid slettes, ændres og læses af brugeren, så de er ikke velegnede til lagring af personlige oplysninger.

*Advarsel: Hvis dit websted bruger cookies til at spore brugere eller tilføjelser fra tredjeparter (f.eks. Facebook like-knappen, Google Analytics trafikmåler, reklamebannere), skal du informere brugeren om dette.*

> "Endnu en bemærkning: dit websted bør ikke indeholde reklamer eller målekoder, før du har fået samtykke. Og det er noget lort."
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Læs en værdi fra en cookie
--------------------------

Alle cookies gemmes i den superglobale variabel `$_COOKIE`, som gemmer hver nøgle som et array.

Hvis vi f.eks. har gemt navnet på den aktuelt loggede bruger under nøglen `user` i cookien, kan vi nemt hente det:

```php
echo $_COOKIE['bruger'];
```

Bemærk: Cookies findes ikke altid (f.eks. hvis du er en ny bruger). Vi bør derfor altid kontrollere, om der findes cookies, før vi opfører en liste, og om nødvendigt tilbyde en alternativ fejlmeddelelse.

```php
if (isset($_COOKIE['bruger']) && $_COOKIE['bruger']) {
    echo 'Logget ind som bruger:' . $_COOKIE['bruger'];
} else {
    echo 'Der er ingen, der er logget ind.';
}
```

Få alle tilgængelige cookies
--------------------------------

Da alle cookies er gemt i den superglobale variabel `$_COOKIE`, kan de nemt opgøres:

```php
var_dump($_COOKIE);
```

Du kan også gå gennem cyklussen og få alle nøgler og værdier:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// skriv nøglen og værdien ud
    echo '<br>';				// ombryd linjen
}
```

Lagring af værdien i en cookie
--------------------------

Funktionen `setcookie()` bruges til at gemme data i cookies.

Den første parameter er cookienøglen, som bruges til at læse den fra feltet `$_COOKIE`, og den anden parameter er selve dataene som en streng.

Med den tredje parameter kan vi (valgfrit) indstille den gyldighedsperiode, som cookien vil være tilgængelig i. Tidspunktet for tilgængelighed angives som <a href="/date">timestamp</a>, så hvis vi vil indstille en cookie med en gyldighed på 1 time fra dette øjeblik, skal vi blot skrive `time() + 3600`.

```php
$data = 'Noget af det indhold, vi ønsker at gemme.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Lagring af større data
-------------------

Cookies er ikke egnede til lagring af større data (browsere tillader normalt kun 4 kB og højst 20 cookies at blive gemt, størrelsen omfatter også cookienavne, gyldighedsindstillinger osv.)

Det er bedre at gemme større data på serveren og blot sætte en identifikator i cookien, som vi kan se, hvilken bruger den tilhører. Denne metode kaldes `$_SESSION` og behandles i en separat artikel.

Hvis du ikke nødvendigvis har brug for at gemme data altid synkront på serveren, kan du bruge **<a href="https://jecas.cz/localstorage">localstorage</a>**-lagring, der er tilgængelig i javascript. Dens kapacitet er på omkring MB, og dataene kan ikke udløbe.
