Hashing af strenge og adgangskoder
==================================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	da: hashing-af-strenge-og-adgangskoder
> 
> perex:
> 	- 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> 	- 'Hash er ikke en ciffer! Metoder til hashing af data og adgangskoder. MD5, SHA1, Bcrypt. Bekræftelse af adgangskode.'
> 
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: f6ea0b06d6ace3c41684a49938f7ce8e

Hashing-processen (i modsætning til kryptering) producerer et output fra input, hvorfra den oprindelige streng ikke længere kan udledes.

Den er derfor velegnet til beskyttelse af følsomme strenge, adgangskoder og checksummene.

En anden god egenskab ved hashing-funktioner er, at de altid genererer output af samme længde, og at en lille ændring i input altid ændrer hele outputtet fuldstændigt.

Hashing-funktioner
----------------

Der er mange hash-funktioner i PHP, de vigtigste er:

- **Bcrypt: password_hash()** - Den mest sikre hashning af adgangskoder, langsom beregning, bruger internt salt og hasher iterativt.
- **md5()** - Meget hurtig funktion, der er velegnet til hashing af filer. Udgangen er altid på 32 tegn.
- **sha1()** - Hurtig hash-funktion til filhashing, bruges internt af Git til commit-hashing. Udgangen er altid på 40 tegn.

Hashing
-----------

```php
$password = 'secret-password';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Varsling:** Hverken `md5()` eller `sha1()` er egnet til hashing af adgangskoder, fordi det er beregningsmæssigt let at finde frem til den originale adgangskode eller i det mindste at forudberegne adgangskoderne. Det er meget bedre at bruge `bcrypt`, som blev udviklet til hashing af adgangskoder.
>
> <a href="https://www.md5cracker.com/">md5cracker.com</a> har en database med checksum (hashes), prøv at søge efter hash: `79c2b46ce252594ecbcbcb5b73e928345492`, som du kan se, så ren `md5()` er ikke så sikkert for almindelige ord og adgangskoder.

Den eneste korrekte løsning: `Bcrypt + salt`.
--------------------------------------

I foredraget <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Sådan laver du ikke rod i målplanet</a>, behandlede David Grudl måder at hashe og gemme adgangskoder på korrekt vis.

Den eneste korrekte løsning er: `Bcrypt + salt`.

Mere specifikt:

```php
$password = 'hash';

// Genererer en sikker hash
echo password_hash($password, PASSWORD_BCRYPT);

// Alternativt med højere kompleksitet (standard er 10):
echo password_hash($password, PASSWORD_BCRYPT, ['omkostninger' => 12]);
```

Fordelen ved Bcryp er primært dens hastighed og automatiske saltning.

Det faktum, at det tager **langt** at generere, f.eks. 100 ms, gør det meget dyrt for en angriber at teste mange adgangskoder.

Desuden behandles output-hashingen automatisk med **tilfældig salt**, hvilket betyder, at når det samme kodeord hashes gentagne gange, er output altid en anden hash. Derfor vil en angriber ikke kunne bruge en forudberegnet hashtabel.

Derfor vil vi ikke kunne verificere, at adgangskoden er korrekt ved gentagen hashing, men skal kalde en specialiseret funktion:

```php
if (password_verify($password, $hash)) {
    // Adgangskoden er korrekt
} else {
    // Adgangskoden er forkert
}
```

Saltning af kodeord
------------

For at gøre det vanskeligere at knække hash-koder er det en god idé at indsætte en ekstra streng i det oprindelige input. Helst en tilfældig. Denne proces kaldes **password salting**.

Sikkerheden er baseret på den idé, at en angriber ikke vil kunne bruge en på forhånd beregnet tabel med adgangskoder og hashes, fordi han ikke kender saltet og skal knække adgangskoderne enkeltvis.

For eksempel:

```php
$password = 'secret_passport';
$salt = 'fghjgtzjjhg';

$hash = md5($password . $salt);

echo $password; // udskriver den oprindelige adgangskode
echo $hash;     // udskriver hash af adgangskode inklusive salt
```

Sammensatte hash-funktioner
------------------------

Du tænker måske, at det ville være en god idé at udføre hash-funktionen flere gange og dermed gøre det mere kompliceret at knække den, da det oprindelige kodeord skal hashes flere gange.

For eksempel:

```php
$password = 'adgangskode';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

Paradoksalt nok bliver det mindre vanskeligt at bryde igennem eller forbliver næsten uændret.

Årsagen er, at funktionen `md5()` er ekstremt hurtig og kan beregne over en million hashes i sekundet på en almindelig computer, så det er ikke meget langsommere at prøve adgangskoderne en efter en.

Den anden grund er mere teoretisk, nemlig muligheden for at støde på en såkaldt kollision. Hvis vi hasher et kodeord gentagne gange, kan det med tiden ske, at vi finder en hash, som angriberen allerede kender, og det vil gøre det muligt for ham at hashe kodeordet ved hjælp af databasen.

Derfor er det bedre at bruge en langsom, sikker hashing-funktion og kun udføre hashing-funktionen én gang, mens det endelige output stadig behandles med saltning.

Ekstremt sikker sammenligning af to hashes/strenge
---------------------------------------------------

Vidste du, at ===-operatoren ikke er det mest sikre valg til hash-sammenligning i forbindelse med adgangskodebekræftelse?

Når du sammenligner strenge, gennemløbes de to strenge tegn for tegn, indtil du når slutningen (succes, de er ens) eller der er en forskel (strengene er forskellige).

Og dette kan udnyttes i et angreb. Hvis du måler tiden nøjagtigt nok, kan du vurdere, hvor mange tegn der skal tilføjes for at få et nøjagtigt match og nå til slutningen, eller du kan vurdere, hvor langt strengene er nået, når du sammenligner strengene.

Løsningen er at bruge funktionen hash_equals() overalt hvor strenge sammenlignes, og det ville gøre noget, hvis en angriber kunne finde ud af, hvor sammenligningen mislykkedes.

Og hvordan gør funktionen det? Den sørger for, at sammenligningen af to strenge altid tager lige lang tid, så du ikke kan se ved at måle tiden, hvor forskellen er opstået. Jeg finder nogle typer angreb virkelig meget usandsynlige og svære at gennemføre. Dette er en af dem.
