Zmienne Zmienne
===============

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	pl: zmienne-zmienne
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

> **Ostrzeżenie:** Ten artykuł został napisany wiele lat temu i niektóre informacje mogą być nieaktualne lub nieprawidłowe. Należy o tym pamiętać podczas lektury.

**Zmienne** nie są przeznaczone do powszechnego stosowania (rozwiązują problemy, które można rozwiązać w inny sposób), służą głównie do zwięzłego zapisu i bardziej złożonego dostępu do pamięci.

Rozważmy następujący przykład:

```php
$x = 25;                  // zawiera 25
$nacitana_promenna = 'x'; // zawiera "x"
$y = $$nacitana_promenna; // zawiera 25
echo $y;                  // drukuje 25
```

Zwróć uwagę na dwa dolary następujące po sobie. W tym przypadku wartość zmiennej $y zostanie wczytana do zmiennej o nazwie zawartej w zmiennej $nacitana_variable.

Trochę to zagmatwane, co? Dlatego lepiej nie używać zmiennych.
> **Uwaga:** Zmienne zmienne są specjalnością PHP ze względu na znak dolara. W innych językach początek nazwy zmiennej nie jest oznaczany żadnym znakiem, więc nie można używać zmiennych zmiennopozycyjnych, ponieważ byłoby niejednoznaczne, kiedy jest to zmienna klasyczna, a kiedy nie.
