Sådan bryder du md5-funktionen
==============================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	da: sadan-bryder-du-md5-funktionen
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 er en meget almindeligt anvendt funktion til beregning af hashes.

Nybegyndere bruger den ofte til <a href="/hashovani">password hashing</a>, hvilket ikke er en god idé, da der er mange måder at hente den oprindelige adgangskode på.

Denne artikel beskriver specifikke metoder til at gøre det.

Tidskompleksitet
----------------

Al sikkerhed er baseret på det faktum, at det tager uforholdsmæssigt lang tid at prøve alle adgangskoder. Det burde den også. Problemet med `md5()`-algoritmen er især, at det er en meget hurtig funktion. På en normal computer er det ikke noget problem at beregne over en million hashes i sekundet.

Hvis vi bryder adgangskoden ved at prøve kombinationer én efter én, er det et **brute force-angreb**.

Metoder til krakning
----------------

Der er flere strategier:

- Successiv trial-and-error-testning (brute force-angreb)
- Test af ordbogsadgangskoder
- Regnbuetabeller (forudberegnet hash-database)
- Søgninger på Google
- Kollisioner i algoritmen

Der findes mange flere metoder, men denne artikel beskriver kun de mest almindelige.

Brute force-brudstrategier
-----------------------------

Alle kombinationer af bogstaver, tal og andre tegn afprøves én ad gangen.

De genererede forsøg hashes en efter en og sammenlignes med den oprindelige hash.

Så for eksempel:

```php
aaaaaaabaaacaaadaaaeaaaf...
```

Problemet med dette angreb ligger i selve `md5()`-algoritmen: Hvis vi kun skulle prøve de små bogstaver i det engelske alfabet og tal, ville det højst tage ti minutter at prøve alle kombinationer på en almindelig tilgængelig computer.

Det er derfor vigtigt at vælge lange passwords, helst tilfældige passwords med specialtegn.

Strategi for angreb med ordbøger
----------------------------

Folk vælger normalt svage adgangskoder, der findes i ordbogen.

Hvis vi udnytter dette faktum, kan vi hurtigt udelukke usandsynlige varianter som `6w1SCq5cs` og i stedet gætte på eksisterende ord.

Desuden ved vi fra tidligere password-lækager fra store virksomheder, at brugerne vælger et stort bogstav i begyndelsen af passwordet og et tal i slutningen. Lad os se - har din adgangskode også det? :)

Regnbuetabeller - forudberegnet database
--------------------------------------

Da en adgangskode altid svarer til den samme hash, er det let at genberegne en stor database, hvor adgangskoderne søges først.

Faktisk er det altid meget hurtigere at søge end at søge i hashes igen og igen.

Ved større datalækager kan adgangskoder desuden hashes parallelt på denne måde, og f.eks. kan 10 % af alle brugernes adgangskoder hurtigt hentes frem.

En god passworddatabase er f.eks. <a href="https://crackstation.net/">Crack Station</a>.

Google-søgning
-------------------

Mange enkle adgangskoder er kendt direkte af Google, fordi Google indekserer sider, der indeholder hashes.

Jeg bruger altid Google som min første mulighed. :)

Findning af kollisioner i algoritmen
--------------------------

<a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Dirichlet-princippet</a> beskriver, at hvis vi har et sæt hash-koder, der altid er 32 tegn lange, så er der mindst to forskellige adgangskoder på 33 tegn (en længere), der genererer den samme hash.

I praksis giver det ikke mening at lede efter kollisioner, men nogle gange gør programforfatteren selv gætteriet lettere ved at genberegne kollisionerne.

For eksempel:

```php
$password = 'adgangskode';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

I dette tilfælde giver det mening at gætte på kollisionen i stedet for den oprindelige hash.

Skål for at smøre!
