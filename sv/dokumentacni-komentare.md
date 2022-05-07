Kommentarer till dokumentärfilmer, tjeckiska eller engelska?
============================================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	sv: kommentarer-till-dokumentaerfilmer-tjeckiska-eller-engelska
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

Att skriva dokumentation tar tid och ofta är det ingen som läser den efter dig, så det är bra att skriva kommentarer direkt i källkoden. En massa text gör dock koden onödig och den får inte plats på skärmen samtidigt, vilket återigen försämrar läsbarheten.

Därför bör all kod vara **självförklarande**, dvs. när man läser den bör man omedelbart förstå vad den gör och den bör bara göra en sak.

Om en funktion till exempel heter `getUserProfileById($id)` är det kristallklart vad en sådan funktion gör och vilket returvärde vi kan förvänta oss. Det är därför som kommentarer ofta används för att beskriva endast in- och utgångsparametrarna + en kort textförklaring av principen (om något mer komplext pågår). Om koden är avsedd för flera personer är det artigt att ange författare och datum för skapandet.

```php
/**
 * Återger TRUE om det överlämnade värdet är en giltig IPv6-adress.
 * Alternativt reguljärt uttryck:
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @författare Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

Det framgår direkt av kommentaren att funktionen förväntar sig en IPv6-adress som indata och returnerar ett boolska `TRUE` eller `FALSE` beroende på hur valideringen går.

Bootstrap-annotationen används för att ange värden med hjälp av standardiserade nycklar, som många redaktörer kan bearbeta vidare och till exempel ange parametrar när funktionen anropas eller överföra beroenden automatiskt.

Kommentarer direkt i koden används endast i speciella fall. Om koden behöver en intern kommentar är det vanligtvis ett tecken på att den bör delas upp i flera mindre delar som behandlar sin egen del av programmet.

Språk
--------------

Det är vanligt att alltid namnge klasser, metoder, funktioner och variabler på engelska (för att göra koden läsbar för ett stort antal programmerare), nycklar är relativt standardiserade med en enhetlig syntax, så språket spelar ingen roll där heller, och när det gäller hjälptext beror det på hur projektet används. Om det är ett litet projekt för ett stabilt team av personer är engelska inte nödvändigtvis ett problem, tvärtom, alla förstår åtminstone beskrivningen perfekt.

Motivation för att använda kommentaren
-------------------

Specialtecken (call signs) kan uppmärksammas av andra verktyg än editorn, så det är till exempel användbart att lägga sina tester (på vilka ingångar jag förväntar mig vilken utgång) direkt ovanför funktionsdefinitionen, vilket gör att man aldrig glömmer bort platsen där testerna är skrivna och samtidigt har varje programmerare en omedelbar uppfattning om vad funktionen gör.

Här är ett litet exempel på hur en testdefinition kan se ut (det är bara ett notationsbegrepp, inte ett specifikt testverktyg):

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
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['land'] == 'SV')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

Vilket i allmänhet skulle skrivas, till exempel, enligt följande regel:

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

I testerna använde jag en slumpgenerator med hjälp av reguljära uttryck.
Här är några exempel:

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
