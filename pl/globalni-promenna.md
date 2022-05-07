Zmienne globalne w PHP
======================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	pl: zmienne-globalne-w-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Zmienne globalne są dostępne w dowolnym momencie w dowolnej części aplikacji i nie trzeba ich przekazywać.

> **Ostrzeżenie:** Dobrze zaprojektowana aplikacja nie powinna używać zmiennych globalnych, ponieważ naruszają one zasadę hermetyzacji i mogą powodować trudne do wykrycia błędy, jeśli są obsługiwane nieostrożnie.

Przykładowe zastosowanie:

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // wypisuje liczbę 3, ponieważ zmienna $b jest globalna
```

Zwróćmy uwagę, że zmienne `$a` i `$b` otrzymaliśmy poza ich naturalnym kontekstem. Takie zachowanie jest określane mianem "magii", ponieważ jeśli inna funkcja zastąpi aktualnie używane zmienne, w aplikacji wystąpi nieoczekiwany stan.

Prawidłowo aplikacja powinna za każdym razem **kapsułkować** i przekazywać zmienne:

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // drukuje 3
```

Dzięki temu możemy dynamicznie wywoływać funkcję z różnymi parametrami wejściowymi, a jej wynik będzie zależał tylko od danych wejściowych, a nie od otoczenia.

Pobieranie parametrów wejściowych z adresu URL
---------------------------------

Być może jedynym sensownym zastosowaniem zmiennych globalnych jest parsowanie danych wejściowych użytkownika, w którym to przypadku mówimy o <a href="/superglobal-variable">zmiennych globalnych</a>.

W tym przypadku jest to czysty projekt, ponieważ zmienna powinna być tylko do odczytu, a nie tylko do zapisu, a ponadto jest taka sama w całej aplikacji:

```php
function getNameFromUrl(): string
{
    return isset($_GET['nazwa'])
    	? htmlspecialchars($_GET['nazwa'])
    	: '';
}

echo getNameFromUrl();
```
