Komentáre k dokumentom, česky alebo anglicky?
=============================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	sk: komentare-k-dokumentom-cesky-alebo-anglicky
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

Písanie dokumentácie zaberá čas a často ju po vás nikto nečíta, preto je dobrým zvykom písať komentáre priamo do zdrojového kódu. Množstvo textu však zbytočne zahlcuje kód, ktorý sa potom nemusí súčasne zmestiť na monitor, čo opäť znižuje jeho čitateľnosť.

Preto by mal byť každý kód **samovysvetľujúci**, t. j. pri jeho čítaní by malo byť okamžite jasné, čo robí, a mal by robiť len jednu vec.

Ak sa napríklad funkcia volá `getUserProfileById($id)`, je úplne jasné, čo takáto funkcia robí a akú návratovú hodnotu môžeme očakávať. Preto sa často používajú komentáre, ktoré popisujú len vstupné a výstupné parametre + krátke textové vysvetlenie princípu (ak ide o niečo zložitejšie). Ak je kód určený pre viacero osôb, je slušné uviesť autora a dátum vytvorenia.

```php
/**
 * Vracia TRUE, ak je odovzdaná hodnota platná adresa IPv6
 * Alternatívny regulárny výraz:
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @autor Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

Z komentára je hneď jasné, že funkcia očakáva ako vstup IPv6 adresu a vracia logickú hodnotu `TRUE` alebo `FALSE` podľa toho, ako prebieha validácia.

Anotácia bootstrap sa používa na označenie hodnôt pomocou štandardizovaných kľúčov, ktoré môžu mnohé editory ďalej spracovávať a napríklad naznačovať parametre pri volaní funkcie alebo automaticky odovzdávať závislosti.

Komentáre priamo v kóde sa používajú len v špeciálnych prípadoch. Ak kód potrebuje vnútorný komentár, zvyčajne to znamená, že by sa mal rozdeliť na viacero menších častí, ktoré budú riešiť vlastnú časť programu.

Jazyky
--------------

Je zvykom pomenovávať triedy, metódy, funkcie a premenné vždy v angličtine (aby bol kód čitateľný pre veľké množstvo programátorov), kľúče sú relatívne štandardizované s jednotnou syntaxou, takže ani tam na jazyku nezáleží, a pri texte nápovedy záleží na použití projektu. Ak ide o malý projekt pre stabilný tím ľudí, angličtina nemusí byť problém, naopak, aspoň každý dokonale rozumie opisu.

Motivácia na používanie anotácie
-------------------

Špeciálne znaky (call signály) si môžu všimnúť aj iné nástroje mimo editora, preto je užitočné napríklad jej testy (na akých vstupoch očakávam aký výstup) umiestniť priamo nad definíciu funkcie, čím sa nikdy nezabudne na miesto, kde sú testy napísané a zároveň má každý programátor okamžitú predstavu, čo funkcia robí.

Tu je malý príklad toho, ako môže vyzerať definícia testu (ide len o koncept zápisu, nie o konkrétny testovací nástroj):

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
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return["krajina] == "CS)

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

Ktoré by sa vo všeobecnosti písali napríklad podľa tohto pravidla:

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

Vnútri testov som použil generátor náhodných hodnôt pomocou regulárnych výrazov.
Tu je niekoľko príkladov:

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
