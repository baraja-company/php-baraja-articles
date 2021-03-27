Mergování velkých polí v PHP
================================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slugCS: mergovani-velkeho-pole
> publicationDate: 2020-02-06 09:32:08
> mainCategoryId: 95374429-e651-46bd-9149-15aa716f8207

Často potřebujeme spojit více polí dohromady, to lze velmi elegantně provést funkcí `array_merge`:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// vrátí [1, 2, 3, 5, 6, 7]
$finalIds = array_merge($userIdsA, $userIdsB);
```

Funkce `array_merge` spojí dvě pole do jednoho velkého. Pokud nastane kolize v klíčích, vyhraje hodnota pravého pole.

Opakované mergování v cyklu
---------------------------

Často však získáváme pole polí, které vzniká až v cyklu (například z databáze a pak procházíme přes foreach) a proto předem nevíme počet mergování.

Naivní řešení může vypadat třeba takto:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Toto řešení je však velmi neefektivní vůči CPU, protože musíme s každou iterací mergovat pole dohromady a celé velké pole opakovaně procházet.

Existuje však jednoduché řešení, kdy upravíme mergovací algoritmus, abychom data procházeli jen jednou:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

V tomto případě v poli `$finalIds` bude vznikat o trochu více dat, ale stále to je menší problém, než výhoda, která vznikne uspořením času.

Samotné mergování se liší podle verze PHP, kterou používáte a řeší se elegantním trikem:

```php
/* PHP 5.6 a starší */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ a novější */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ a novější */
$finalIds = array_merge(...$finalIds);
```

Zejména řešení `array_merge(...$finalIds)` vypadá velmi zajímavě, protože využívá nového konceptu PHP 7, kdy lze předat dynamický počet argumentů do funkce pomocí znaku trojtečky na začátku. Proces mergování je pak maximálně efektivní a celou logiku si vyřeší PHP uvnitř automaticky.
