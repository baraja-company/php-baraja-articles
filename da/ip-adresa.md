Hent brugerens IP-adresse i PHP
===============================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	da: hent-brugerens-ip-adresse-i-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Hent brugerens IP-adresse i PHP, gemmer IP-adressen og forbyder brugeren. Undersøgelse af VPN og brugere bag proxy eller NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '54a77b5e4cc431b354903e42a27682a6'

I PHP er det meget nemt at registrere en IP-adresse på et grundlæggende niveau:

```php
echo 'Du ved, din IP-adresse er' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Varsling:** Det er kun muligt at få IP-adressen som nøgle i feltet `$_SERVER['REMOTE_ADDR']`, hvis PHP blev kaldt fra browseren. I CLI-tilstand (f.eks. ved at køre fra Terminal med cron) er IP-adressen ikke tilgængelig (det giver mening, da der ikke foretages nogen netværksanmodning).

Pålidelig IP adresseopsporing
-----------------------------

Efter mange års udvikling har jeg endelig holdt fast ved denne implementering:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Cloudflare-understøttelse
        $ip = $_SERVER['HTTP_CF_CONNECTING_IP'];
    } elseif (isset($_SERVER['REMOTE_ADDR']) === true) {
        $ip = $_SERVER['REMOTE_ADDR'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP'])) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
                $ip = $_SERVER['HTTP_X_FORWARDED_FOR'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', 'localhost'], true)) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

Så er det meget bedre:

```php
echo 'Du ved, din IP-adresse er' . getIp() . '?';
```

Hvis IP'en kan registreres direkte, eller kun er IPv6, eller hvis den er i CLI-tilstand (f.eks. cron), returneres `127.0.0.0.1` (localhost).

Implementeringer, der tager hensyn til `X-Forwarded-For` og `X-Real-IP` headere, er ekstremt farlige direkte i PHP, fordi data let kan ændres, og en angriber kan forfalske en falsk IP-adresse for f.eks. at se administrationen eller aktivere Debug-tilstanden på webstedet (Nette Tracy). På den anden side er vi nødt til at acceptere nogle proxyforespørgsler (f.eks. når vi sender trafik via Cloudflare, eller når Apache og Ngnix kører på samme maskine, når de kaldes lokalt lige efter hinanden).

I tilfælde af direkte brugeradgang til serveren er der kun én korrekt løsning, og det er at sikre på Apache (via udvidelsen `RemoteIP`) og på Nginx via udvidelsen `remote_ip`, at `X-Forwarded-For` sættes fra den faktiske IP-adresse på den besøgende, og at IP-adressen ikke kan sættes med en HTTP-header.

Feltet `$_SERVER['REMOTE_ADDR']` får automatisk den korrekte IP-adresse (dvs. den IP-adresse, hvorfra anmodningen kom direkte til PHP), og vi behøver ikke at tage os af det.

Brugeradgang via proxy
----------------------------

Det sker ofte, at en bruger får adgang via en proxy. Derefter gemmes den faktiske IP-adresse i variablen `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Dette tilfælde kan f.eks. opstå, når routing på serveren løses ved hjælp af metoden `Ngnix -> Apache -> PHP`, hvor `Ngnix` fungerer som reverse proxy før `Apache`. I dette tilfælde ser PHP kun IP-adressen inden for det interne netværk (normalt af formen `127.0.0.0.*`).

F.eks. kan **Cloudflare**-tjenesten opføre sig på denne måde, og man skal være opmærksom på, om vi arbejder med den faktiske brugers IP-adresse eller proxyens IP-adresse. For mig er den bedste måde at bruge funktionen `getIp()`, som blev nævnt i begyndelsen af denne artikel. Vi kan sikre Cloudflare-detektion ved at kontrollere eksistensen af nøglen `$_SERVER['HTTP_CF_CONNECTING_IP']`, som automatisk overføres i hver proxyforespørgsel.

Lagring af IP-adressen
------------------

Det afhænger af, hvilken IP-adresse du har til rådighed.

- IPv4 IP-adresse kan gemmes i 4 bytes, funktionen `ip2long` bruges til dette,
- For en IPv6-IP-adresse skal vi imidlertid bruge 16 bytes, og der er ingen konverteringsfunktion.

Hvis din databaseserver ikke direkte understøtter en datatype for IP-adressen, anbefaler jeg at gemme IP-adressen som `varchar(39)`, hvor begge versioner passer som en streng og kan læses af mennesker.

> Når du gemmer IP-adressen, skal du overveje, om det giver mening også at gemme domænenavnet, der er fundet med funktionen `gethostbyaddr`. Når du laver en liste og søger, kan du ikke finde ud af navnene, fordi det tager meget lang tid, og de kan ændre sig med tiden.

Blokering af den besøgendes IP-adresse
-----------------------------

Den ideelle løsning er at oprette en liste over blokerede IP-adresser og sammenligne denne liste med den aktuelle IP-adresse ved hver forespørgsel. Hvis adresserne stemmer overens, stoppes anmodningen straks.

```php
$blackList = [
    'første-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Desværre er din IP-adresse blokeret :-(';
    die; // Afslut anmodning
}
```

Eksemplet forudsætter en implementering af funktionen `getIp()` som i eksemplet ovenfor.

En mere effektiv løsning er at kontrollere, om indekset forekommer i arrayet:

```php
$blackList = [
    'første-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Desværre er din IP-adresse blokeret :-(';
    die; // Afslut anmodning
}
```

Serverens IP-adresse og servernavn
---------------------------------

Serverens IP-adresse er normalt gemt i feltet `$_SERVER['SERVER_ADDR']`, og dens navn kan fås ved hjælp af konstruktionen `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Men hvis konceptet `Ngnix -> Apache -> PHP` anvendes, og `Ngnix` spiller rollen som reverse proxy, vises serverens rigtige IP-adresse ikke.

I dette tilfælde kan servernavnet findes i feltet `$_SERVER['SERVER_NAME']` eller ved at bruge funktionen `php_uname('n')`. [Officiel dokumentation for uname-funktionen] (https://www.php.net/manual/en/function.php-uname.php).

Vi kan derefter bruge dette trick til at finde serverens offentlige IP-adresse: `gethostbyname(php_uname('n'))`.
