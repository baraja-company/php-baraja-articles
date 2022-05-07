Získavanie parametrov z adresy URL metódou GET
==============================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	sk: ziskavanie-parametrov-z-adresy-url-metodou-get
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

Viete, že máte otvorenú stránku, sledujete adresu URL a vidíte otáznik s niektorými parametrami. Neskúsený programátor by si myslel, že ide o samostatné súbory, ale hľa. Skúste vytvoriť súbor, ktorý má v názve otáznik (nefunguje to). **To je dôvod, prečo bol tento článok napísaný**.

Čo to je?
--------------------------

V skutočnosti ide o to, že je to jeden súbor, ktorému odovzdávate premenné prostredníctvom adresy URL, takže mám napríklad súbor **index.php** a odovzdávam mu názov článku: **index.php?clanek=o-php**.

Kód + vysvetlenie
--------------------------

Superglobálna premenná `$_GET` obsahuje kľúče s parametrami z adresy URL

```php
echo $_GET['Článok'] ?? '';
```

Bezpečnostné a dĺžkové obmedzenia
--------------------------

Metóda GET nie je bezpečná, preto by sa cez ňu nemali posielať dôverné údaje, jedným z hlavných dôvodov je, že ide o nešifrovanú komunikáciu a po druhé, že sa ukladá do histórie.

Dôverné údaje alebo len všetko by sa malo posielať pomocou metódy <a href="/method-post">POST</a>. GET sa hodí skôr pre furmuláre, kde je dobré zobraziť parametre (napríklad vyhľadávače, stránka s článkom), aby bolo možné na stránku odkázať.

Dĺžka GET nie je neobmedzená! Veľa začiatočníkov za to platí. Maximálna dĺžka je približne 1024 znakov (niekde sa uvádza 1088). Pri dlhších textoch teda pošlite <a href="/method-post">POST</a> s.
