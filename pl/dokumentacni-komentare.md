Komentarze do dokumentów, czeskie czy angielskie?
=================================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	pl: komentarze-do-dokumentow-czeskie-czy-angielskie
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

Pisanie dokumentacji jest czasochłonne i często nikt nie czyta jej po tobie, dlatego dobrą praktyką jest pisanie komentarzy bezpośrednio w kodzie źródłowym. Jednak duża ilość tekstu niepotrzebnie zaśmieca kod, który może nie zmieścić się jednocześnie na monitorze, co ponownie zmniejsza jego czytelność.

Dlatego każdy kod powinien być **samoobjaśniający**, tzn. czytając go, powinno być od razu jasne, co on robi, i powinien robić tylko jedną rzecz.

Na przykład, jeśli funkcja nazywa się `getUserProfileById($id)`, jest jasne, co taka funkcja robi i jakiej wartości zwrotnej możemy się spodziewać. Dlatego często używa się komentarzy, które opisują tylko parametry wejściowe i wyjściowe oraz krótkie wyjaśnienie tekstowe zasady działania (jeśli chodzi o coś bardziej złożonego). Jeśli kod jest przeznaczony dla wielu osób, dobrze jest podać autora i datę utworzenia.

```php
/**
 * Zwraca TRUE, jeśli podana wartość jest prawidłowym adresem IPv6.
 * Alternatywne wyrażenie regularne:
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

Z komentarza wynika, że funkcja oczekuje na adres IPv6 jako dane wejściowe i zwraca wartość logiczną `TRUE` lub `FALSE` w zależności od tego, jak przebiega sprawdzanie poprawności.

Adnotacja bootstrap służy do wskazywania wartości za pomocą standardowych kluczy, które wiele edytorów może dalej przetwarzać i np. podpowiadać parametry podczas wywoływania funkcji lub automatycznie przekazywać zależności.

Komentarze bezpośrednio wewnątrz kodu są używane tylko w szczególnych przypadkach. Jeśli kod wymaga wewnętrznego komentarza, jest to zwykle wskazówka, że należy go podzielić na kilka mniejszych części, które będą zajmować się własnym fragmentem programu.

Języki
--------------

Zwyczajowo klasy, metody, funkcje i zmienne nazywa się zawsze po angielsku (aby kod był czytelny dla dużej liczby programistów), klucze są stosunkowo ustandaryzowane i mają jednolitą składnię, więc język nie ma tu znaczenia, a w przypadku tekstu pomocy zależy to od zastosowania w projekcie. Jeśli jest to mały projekt dla stałego zespołu ludzi, język angielski nie musi być problemem, wręcz przeciwnie - przynajmniej wszyscy doskonale rozumieją opis.

Motywacja do stosowania adnotacji
-------------------

Znaki specjalne (znaki wywoławcze) mogą być zauważone przez inne narzędzia poza edytorem, dlatego warto na przykład umieszczać jej testy (na jakich wejściach oczekuję jakiego wyjścia) bezpośrednio nad definicją funkcji, dzięki czemu nigdy nie zapomina się o miejscu, w którym są napisane testy, a jednocześnie każdy programista od razu wie, co robi dana funkcja.

Oto mały przykład tego, jak może wyglądać definicja testu (jest to tylko koncepcja notacji, a nie konkretne narzędzie do testowania):

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
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['kraj'] == 'PL')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

Które na ogół byłyby napisane, na przykład, zgodnie z regułą:

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

W testach zastosowałem generator wartości losowych wykorzystujący wyrażenia regularne.
Oto kilka przykładów:

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
