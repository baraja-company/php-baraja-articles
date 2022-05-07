Niebezpieczne użycie stałych w PHP
==================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	pl: niebezpieczne-uzycie-stalych-w-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

Przy używaniu stałych w PHP należy pamiętać o dwóch ważnych rzeczach.

Stałe dynamiczne i statyczne
------------------------------

Stałą można zdefiniować w PHP albo statycznie bezpośrednio w klasie (najlepsze rozwiązanie), na przykład w następujący sposób:

```php
class Region
{
	public const PREFIX = 420;
}
```

A sposób wykorzystania jest dość jasny. W czasie kompilacji klasy wartość stałej jest ustalona i można się do niej dostać, wywołując nazwę klasy i samą stałą. Najczęściej przez napisanie `Region::PREFIX`.

Innym (znacznie gorszym) sposobem jest zdefiniowanie stałej dynamicznie w czasie uruchamiania (najczęściej gdzieś w skrypcie konfiguracyjnym), gdzie jest to coś w rodzaju

```php
define('BASE_DIR', __DIR__ . '/../');
```

Główną wadą definiowania stałej za pomocą funkcji `define` jest to, że skrypt definiujący stałą mógł nie zostać wywołany, więc stała nie będzie istniała przy próbie jej odczytania.

W połączeniu z użyciem stałej dynamicznej w definicji stałej statycznej w klasie, może to nawet spowodować błąd krytyczny:

```php
class InvoiceGenerator
{
	// To jest całkowicie błędne!
	public const DATA_DIR = BASE_DIR . '/dane/faktura';
}
```

> **Wyjaśnienie:**
>
> Użycie stałej dynamicznej wewnątrz stałej statycznej ma tę poważną wadę, że wartości stałej dynamicznej nie można odczytać w czasie kompilacji. Skrypt ten musi być zatem przetwarzany ponownie w każdym żądaniu (tzn. nie może być przechowywany w pamięci podręcznej OPCache w celu optymalizacji szybkości), a jeśli stała w ogóle nie istniała, wyrzucany jest fatalny błąd czasu kompilacji i aplikacja nie może w ogóle działać.

Jeśli używasz programu PhpStan, może on automatycznie ostrzec Cię o tym problemie:

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Kształcenie:**
>
> Wartość wszystkich stałych powinna być zawsze stała.


Dziedziczenie stałych podczas używania statycznych
-------------------------------------

W niektórych przypadkach sensowne jest użycie dziedziczenia w celu nadpisania wartości stałej. Jednak w takim przypadku przodek nie może odczytać wartości od potomka (a przynajmniej nie powinien).

Przykładem może być definiowanie krajów i regionów:

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// Fatalny błąd!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Paradoks polega na tym, że powyższy kod nie musi wyrzucać błędu, ale może zostać wyrzucony przez niewłaściwe użycie dziedziczenia.

Jeśli wywołamy metodę `getPrefix()` na potomku `CzechRepublic`, wszystko będzie w porządku, ponieważ wartość stałej zostanie poprawnie odczytana. Jeśli jednak potomek nie ustawi wartości stałej, zostanie wyrzucony błąd fatalny nieistniejącej stałej. Najgorsze w tym wszystkim jest to, że jest to **ukryta zależność**, która jest tworzona w implementacji metody, a programista dziedziczący klasę może nawet nie wiedzieć o tej zależności.

Najlepszym rozwiązaniem w tym przypadku jest albo zdefiniowanie stałej bezpośrednio w przodku z wartością domyślną (tak aby logika zawsze działała), albo przynajmniej rzucenie wyjątku w getterze.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('Region nie został określony.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan odpowiada na ten błąd w następujący sposób:

```txt
Access to undefined constant static(Region):REGION.
```
