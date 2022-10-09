Sådan fungerer Captcha (beskrivende billede)
============================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	da: sadan-fungerer-captcha-beskrivende-billede
> 
> perex:
> 	- 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> 	- 'En captcha er en type billede, der bekræfter, at brugeren ikke er en robot, og som beskytter webapplikationen. Muligheder for implementering af PHP.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

Captcha er i øjeblikket en af de mest almindelige måder at beskytte gratis formater på. Det blev oprindeligt ikke oprettet for at beskytte datasikkerheden, men for at beskytte mod spam og for at erkende, at det er et menneske.

Det er dog maskinelt genereret, så det er ikke altid perfekt, men succesraten for en captcha-test ligger et sted omkring 99 %, og kun 1 % af billederne kan afkodes af en velkonstrueret spamrobot.

Sådan fungerer det
--------------------------

Der er flere muligheder, f.eks. bruger jeg denne løsning:

- Serveren modtager oplysninger om, at brugeren har anmodet om en formular, og begynder at generere den.
- I det fremtidige captcha-testfelt placeres et billede med en hemmelig adresse, der ikke siger noget (f.eks. et tilfældigt tal, en tegnstreng, ... hvad som helst).
- Billedet genereres og lagres på denne adresse. Samtidig skrives den korrekte udskrift et sted (måske til en session eller en database).
- Når brugeren indsender formularen, kontrolleres det, om udskriftet stemmer overens med det, han/hun har indtastet. Hvis det er tilfældet, behandles formularen. Hvis ikke, anmodes der om en ny beskrivelse af et andet billede.
- Efter både vellykkede og mislykkede forsøg slettes det midlertidige captcha-billede (det skal aldrig bruges igen). Hvis formularen ikke indsendes inden for en vis tid, bliver den også slettet (udløber).

Korrekt transskription
--------------------------

Hvis captcha-testen kan løses, er brugeren sandsynligvis et menneske. Men det er godt at tænke på brugere, der ikke kan løse opgaven, især blinde brugere. Det er en god løsning til at kombinere flere mulige tests (især voice prefetching). Stemmegenkendelse af en maskine er dog i øjeblikket betydeligt mere effektiv end oplæsning af et billede. Derfor er denne løsning ikke altid ideel.

Brugerdefineret simpelt captcha-billede
--------------------------

Ofte er det nok at generere et tomt billede med bestemte dimensioner og indtaste et par tegn i det på en læselig måde uden yderligere redigering. Helt ærligt! De fleste spam-bots er dumme og kan ikke angribe generiske formularer med denne type beskyttelse, selv om der er tale om perfekt læsbar tekst, som bedre OCR kan transskribere perfekt.

Det resulterende billede kan se således ud:

```txt
<img src="captcha.php" alt="ukázková captcha">
```

Prøv at opdatere siden et par gange, og du vil se, at koden ændres tilfældigt hver gang. Til demonstrationsformål genereres den blot uden at blive gemt, så den fjernes straks efter at du har indlæst den.

Jeg har løst kildekoden ved hjælp af PHPGD-biblioteket, som er tilgængeligt på stort set alle PHP-installationer og -hosting:

```php
Header("Indholdstype: image/png");
$obr = ImageCreate(100, 35);
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //definition af baggrundsfarve
$bila = ImageColorAllocate ($obr, 255, 255, 255); //definition af hvid farve for tekst
$styl = array ($pozadi);
ImageSetStyle ($obr, $styl);
  $nahodne_cislo = rand(11111,99999); //trækning af et tilfældigt tal på 5 tegn
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); //funktion til at tegne tekst (i dette tilfælde et tal)
ImagePNG($obr); //generering af billedet i hukommelsen og rendering
ImageDestroy($obr); //slette billedet fra hukommelsen (der er ikke længere brug for det, fordi det er genereret én gang)
```

Det er så bare et spørgsmål om HTML at gengive billedet:

```html
<img src="captcha.php">
```

Bemærk, at dette script ikke fungerer i sig selv uden yderligere ændringer. Det tjener kun som en demonstration til at generere et simpelt billede.

Resumé
--------------------------

Captcha-tests er ret pålidelige, men irriterende og tidskrævende. Nogle gange er de ikke læselige, så det er rart at give brugeren mulighed for at indlæse et andet billede ved hjælp af javascript, så hele siden ikke skal genindlæses, og alt skal udfyldes igen.

Husk: Det, der er en triviel opgave for et menneske, kan aldrig være en mulig løsning for en maskine. Derfor kan selv denne primitive captcha med perfekt læsbar tekst beskytte mod mere end halvdelen af spam.
