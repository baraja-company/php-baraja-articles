Hur man bryter md5-funktionen
=============================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	sv: hur-man-bryter-md5-funktionen
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 är en mycket vanligt förekommande funktion för att beräkna hash-koder.

Nybörjare använder den ofta för <a href="/hashovani">hashning av lösenord</a>, vilket inte är någon bra idé eftersom det finns många sätt att få fram det ursprungliga lösenordet.

I den här artikeln beskrivs specifika metoder för att göra det.

Tidskomplexitet
----------------

All säkerhet bygger på det faktum att det tar oproportionerligt lång tid att prova alla lösenord. Ja, det borde det göra. Problemet med algoritmen `md5()` är att den är en mycket snabb funktion. På en normal dator är det inget problem att beräkna över en miljon hash-koder per sekund.

Om vi bryter lösenordet genom att prova kombinationer en efter en är det en **brute force-attack**.

Metoder för sprickbildning
----------------

Det finns flera olika strategier:

- Successiv trial-and-error-testning (brute force-attack).
- Testning av ordbokslösenord
- Regnbågstabeller (förberäknad hash-databas)
- Google-sökningar
- Kollisioner i algoritmen

Det finns många fler metoder, men i den här artikeln beskrivs bara de vanligaste.

Strategier för brytning med hjälp av brute force
-----------------------------

Alla kombinationer av bokstäver, siffror och andra tecken prövas en i taget.

De genererade försöken hashasheras ett efter ett och jämförs med den ursprungliga hashen.

Till exempel:

```php
aaaaaaabaaacaaadaaaeaaaf...
```

Problemet med denna attack ligger i själva algoritmen `md5()`. Om vi bara skulle prova de små bokstäverna i det engelska alfabetet och siffrorna skulle det ta högst tiotals minuter att prova alla kombinationer på en vanlig dator.

Det är därför viktigt att välja långa lösenord, helst slumpmässiga lösenord med specialtecken.

Strategi för en attack med ordboken
----------------------------

Folk väljer vanligtvis svaga lösenord som finns i ordlistan.

Om vi utnyttjar detta faktum kan vi snabbt förkasta osannolika varianter som `6w1SCq5cs` och i stället gissa befintliga ord.

Dessutom vet vi från tidigare lösenordsläckor från stora företag att användarna väljer en stor bokstav i början av lösenordet och en siffra i slutet. Få se - har ditt lösenord det också? :)

Regnbågstabeller - förberäknad databas
--------------------------------------

Eftersom ett lösenord alltid motsvarar samma hash är det lätt att räkna om en stor databas med lösenord som söks först.

Faktum är att sökning alltid är flera storleksordningar snabbare än att söka hash om och om igen.

Vid större dataläckor kan lösenord dessutom hashasas parallellt på detta sätt och till exempel 10 % av alla användarlösenord kan snabbt hittas.

En bra lösenordsdatabas är till exempel <a href="https://crackstation.net/">Crack Station</a>.

Google-sökning
-------------------

Många enkla lösenord är kända direkt av Google eftersom Google indexerar sidor som innehåller hashkoder.

Jag använder alltid Google som första alternativ. :)

Hitta kollisioner i algoritmen
--------------------------

<a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Dirichletprincipen</a> beskriver att om vi har en uppsättning hashkoder som alltid är 32 tecken långa finns det minst två olika lösenord med 33 tecken (ett längre) som genererar samma hashkoder.

I praktiken är det inte meningsfullt att leta efter kollisioner, men ibland gör programförfattaren själv gissningen lättare genom att räkna om kollisionerna.

Till exempel:

```php
$password = 'lösenord';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

I det här fallet är det vettigt att gissa kollisionen i stället för den ursprungliga hashen.

Skål för att basta!
