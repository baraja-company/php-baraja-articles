Spájanie veľkých polí v PHP
===========================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	sk: spajanie-velkych-poli-v-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

Často potrebujeme zlúčiť viacero polí dohromady, čo sa dá urobiť veľmi elegantne pomocou funkcie `array_merge`:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// vráti [1, 2, 3, 5, 6, 7]
$finalIds = array_merge($userIdsA, $userIdsB);
```

Funkcia `array_merge` zlúči dve polia do jedného veľkého poľa. Ak dôjde ku kolízii kľúčov, vyhráva hodnota pravého poľa.

Opakované spájanie v slučke
---------------------------

Často však dostávame pole polí, ktoré sa vytvára až v cykle (napríklad z databázy a potom sa prechádza cez foreach), a preto vopred nepoznáme počet zlúčení.

Naivné riešenie by mohlo vyzerať takto:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Toto riešenie je však procesorovo veľmi neefektívne, pretože pri každej iterácii musíme polia spojiť a iterovať nad celým veľkým poľom.

Existuje však jednoduché riešenie, pri ktorom upravíme algoritmus zlučovania tak, aby údaje prechádzali iba raz:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

V tomto prípade pole `$finalIds` vygeneruje o niečo viac údajov, ale stále je to menší problém ako výhoda úspory času.

Samotné zlučovanie sa líši v závislosti od verzie PHP, ktorú používate, a rieši sa elegantným trikom:

```php
/* PHP 5.6 a staršie */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ a novšie */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ a novšie pre neprázdne polia */
$finalIds = array_merge(...$finalIds);
```

Najmä riešenie `array_merge(...$finalIds)` vyzerá veľmi zaujímavo, pretože využíva novú koncepciu PHP 7, v rámci ktorej môžete funkcii odovzdať dynamický počet argumentov pomocou trojitého znaku na začiatku. Proces zlučovania je potom čo najefektívnejší a celú logiku automaticky spracúva interný systém PHP.

Skrátený zápis `array_merge(...$finalIds)` možno použiť len pre neprázdne polia. Ak je to prázdne pole, funkcii nie je odovzdaný žiadny argument a funkcia vyhodí chybu `Funkcia array_merge vyvolaná s 0 parametrami, aspoň 1 požadovaný.`.
