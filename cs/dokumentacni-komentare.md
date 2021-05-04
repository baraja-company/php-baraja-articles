Dokumentační komentáře, česky nebo anglicky?
============================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1"

Psaní dokumentace zdržuje a často ji už po vás nikdo nečte, proto je dobrým zvykem psát komentáře přímo do zdrojového kódu. Mnoho textu ovšem zbytečně zanáší kód, který se poté nemusí vejít najednou na monitor, což opět snižuje jeho čitelnost.

Proto by každý kód měl být **samovysvětlující**, tj. při jeho čtení by mělo být hned jasné, co dělá, a měl by dělat právě jednu věc.

Pokud se například funkce jmenuje `getUserProfileById($id)`, tak je křišťálově jasné, co taková funkce dělá a jakou můžeme očekávat návratovou hodnotu. Proto se za pomoci komentářů často popisují jen vstupní a výstupní parametry + krátké textové vysvětlení principu (pokud se děje něco složitějšího). Když je kód určen pro více lidí, tak je slušností uvést jeho autora a datum vytvoření.

```php
/**
 * Vrati TRUE, kdyz je predana hodnota validni IPv6 adresa
 * Alternativni regularni vyraz:
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @author Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```


Z komentáře je hned jasné, že funkce očekává jako vstupní parametr IPv6 adresu a vrátí booleovský kód `TRUE` nebo `FALSE` v závislosti na tom, jak dopadne validace.

Za pomoci zavináčové anotace se označují za pomoci standardizovaných klíčů hodnoty, které umí celá řada editorů dále zpracovat a na základě toho například provést napovídání (hintování) parametrů při volání funkce, nebo automaticky předávat závislosti.

Komentáře přímo uvnitř kódu se používají jen ve zvláštních případech. Pokud kód potřebuje vnitřní komentář, tak to je obvykle náznak toho, že by se měl rozložit na více menších částí, které budou řešit svůj kus programu.

Jazyky
--------------

Názvy tříd, metod, funkcí a proměnných je zvykem pojmenovávat vždy anglicky (aby byl kód čitelný pro velkou řadu programátorů), klíče jsou relativně standardizované s jednotnou syntaxí, takže tam na jazyku také nezáleží a u pomocných textů závisí na použití projektu. Pokud jde o menší projekt pro stabilní team lidí, tak čeština nemusí být nutně problém, ba naopak aspoň všichni perfektně popisku rozumí.

Motivace k používání zavináčové anotace
-------------------

Speciálních znaků (zavináčů) se mohou všímat další nástroje nad rámec editoru, proto je vhodné například přímo nad definici funkcí uvést její testy (na jaké vstupy očekávám jaký výstup), čímž nikdy nezapomenu místo, kam se testy píší a zároveň má každý programátor ihned představu, co funkce dělá.

Tady je menší příklad, jak by definice testů mohla vypadat (jde pouze o koncept zápisu, nejedná se o žádný konkrétní testovací nástroj):

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
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['country'] == 'CS')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```


Což by se obecně zapisovalo třeba podle pravidla:

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```


Uvnitř testů jsem použil generátor náhodných hodnot za pomoci regulárních výrazů.
Tady je pár příkladů:

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
