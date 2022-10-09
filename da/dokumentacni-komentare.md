Dokumentariske kommentarer, tjekkisk eller engelsk?
===================================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	da: dokumentariske-kommentarer-tjekkisk-eller-engelsk
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

Det tager tid at skrive dokumentation, og ofte er der ingen, der læser den efter dig, så det er en god praksis at skrive kommentarer direkte i kildekoden. En masse tekst fylder dog unødigt meget i koden, som så måske ikke passer på skærmen samtidig, hvilket igen forringer læsbarheden.

Derfor bør enhver kode være **selvforklarende**, dvs. at det ved læsning skal være klart, hvad den gør, og at den kun skal gøre én ting.

Hvis en funktion f.eks. hedder `getUserProfileById($id)`, er det helt klart, hvad en sådan funktion gør, og hvilken returværdi vi kan forvente. Derfor bruges kommentarer ofte til kun at beskrive input- og outputparametre + en kort tekstforklaring af princippet (hvis der er tale om noget mere komplekst). Når koden er beregnet til flere personer, er det høfligt at angive forfatteren og oprettelsesdatoen.

```php
/**
 * Returnerer TRUE, hvis den overførte værdi er en gyldig IPv6-adresse
 * Alternativt regulært udtryk:
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @forfatter Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

Det fremgår umiddelbart af kommentaren, at funktionen forventer en IPv6-adresse som input og returnerer et boolsk `TRUE` eller `FALSE` afhængigt af, hvordan valideringen forløber.

Bootstrap-annotationen bruges til at angive værdier ved hjælp af standardiserede nøgler, som mange redaktører kan behandle yderligere og f.eks. angive parametre ved kald af funktionen eller videregive afhængigheder automatisk.

Kommentarer direkte inde i koden bruges kun i særlige tilfælde. Hvis koden har brug for en intern kommentar, er det normalt et tegn på, at den skal opdeles i flere mindre dele, som skal henvende sig til deres egen del af programmet.

Sprog
--------------

Det er almindeligt at navngive klasser, metoder, funktioner og variabler på engelsk (for at gøre koden læsbar for et stort antal programmører), nøgler er relativt standardiserede med en ensartet syntaks, så sproget er også ligegyldigt der, og for hjælpetekster afhænger det af projektets brug. Hvis der er tale om et lille projekt for et stabilt team af mennesker, er engelsk ikke nødvendigvis et problem, tværtimod forstår alle i det mindste beskrivelsen perfekt.

Motivation for at bruge annotationen
-------------------

Særlige tegn (call-tegn) kan bemærkes af andre værktøjer end editoren, så det er f.eks. nyttigt at sætte sine tests (på hvilke indgange jeg forventer hvilket output) direkte over funktionsdefinitionen, således at man aldrig glemmer det sted, hvor testene er skrevet, og samtidig har alle programmører en umiddelbar idé om, hvad funktionen gør.

Her er et lille eksempel på, hvordan en testdefinition kan se ud (det er blot et notationsbegreb, ikke et specifikt testværktøj):

```php
Dosadí hodnoty a vynutí jejich dump do prohlížeče / konzole:
@test Název I---> $limit => {1-{5-8}}, $city => {Praha|Kladno|Brno|Plzeň} O---> [DUMP]

Vygeneruje náhodný hash o délce 6 znaků a ověří, zda je hlavička odpovědi jakákoli, kromě 201:
@test Název I---> $hash => {a-z0-9|len:6} O---> [HEADER:***|!HEADER:201]

Vygeneruje dvojici čísel a na výstupu ověří jejich součet:
@test Sčítání čísel I---> $a => {1-5}, $b => {7-12} O---> ($return == $a+$b)

Načte náhodný článek s ID mezi 1 až 30, ověří jeho délku nebo neprázdnost:
@test Získání textu článku I---> $id => {1-30} O---> (strlen($return) > 64 || $return != NULL)

Vygeneruje náhodnou IP adresu a ověří, zda je z České republiky:
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['land'] == 'DA')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

Som normalt ville blive skrevet, f.eks. i henhold til reglen:

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

I testene brugte jeg en generator af tilfældige værdier ved hjælp af regulære udtryk.
Her er nogle eksempler:

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
