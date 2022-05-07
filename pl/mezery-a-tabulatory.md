Wcięcie kodu przy użyciu spacji i tabulatorów
=============================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	pl: wciecie-kodu-przy-uzyciu-spacji-i-tabulatorow
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

Aby kod był łatwy do odczytania dla innych programistów i elegancki, musimy nauczyć się go formatować w jednolity sposób. W tym artykule omówiono stosowanie spacji i tabulatorów.

Czy do wcięć w kodzie lepiej używać spacji czy tabulatorów? Jest to często niekończący się temat do dyskusji. Jeśli szukasz szybkiej i jednoznacznej odpowiedzi, to większość dobrych programistów woli używać tabulatorów, ale wyjaśnijmy to sobie w skrócie.

Miejsca
----------------------

Każdy programista i edytor używa innej ilości spacji do wcięć (ale najczęściej 4), co prowadzi do niespójności kodu, który może być trudniejszy do odczytania podczas czytania kodu innej osoby. Ponadto wcięcie wymaga większej liczby znaków (co zwiększa rozmiar danych).

Jednak spacje mają przewagę podczas renderowania kodu w przeglądarce internetowej (gdzie do wcięć używana jest encja HTML `&nbsp;`), więc jest to format stosunkowo łatwy do przenoszenia, który zyskuje przewagę tylko jako stabilna i niezawodna metoda renderowania (4 spacje zawsze będą wyświetlane jako 4 spacje).

Tabulatory
----------------------

Ich szerokość jest taka, jaką programista ustawi w edytorze (jeśli edytor to potrafi), więc jeśli lubisz konkretne wcięcia, nie ma problemu - możemy oglądać ten sam kod z różnymi szerokościami tabulatorów. Jednocześnie jest to bardzo oszczędna postać, która nie musi być powtarzana tak często jak zwykłe spacje.

Podczas renderowania kodu z wcięciami tabulacji na stronie HTML zwyczajowo zastępuje się tabulatory stałymi odstępami, aby zapewnić poprawne wyświetlanie we wszystkich przeglądarkach:

```php
$code = '<?php
    $a = 5+3;
    $b = 4;
    if ($a > $b) {
        echo $a . " > " . $b;
    } else {
        echo $b . " <= " . $a;
    }
?>';

echo str_replace("\t", '&nbsp;', $code);
```
