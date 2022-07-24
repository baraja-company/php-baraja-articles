Hashing ciągów znaków i haseł
=============================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	pl: hashing-ciagow-znakow-i-hasel
> 
> perex:
> 	- 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> 	- 'Hash nie jest szyfrem! Metody haszowania danych i haseł. MD5, SHA1, Bcrypt. Weryfikacja hasła.'
> 
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: f6ea0b06d6ace3c41684a49938f7ce8e

Proces haszowania (w przeciwieństwie do szyfrowania) wytwarza z wejścia wyjście, z którego nie można już wyprowadzić oryginalnego ciągu.

Dlatego dobrze nadaje się do ochrony wrażliwych ciągów znaków, haseł i sum kontrolnych.

Inną miłą cechą funkcji haszujących jest to, że zawsze generują wyjścia o tej samej długości, a mała zmiana na wejściu zawsze całkowicie zmienia całe wyjście.

Funkcje haszowania
----------------

W PHP istnieje wiele funkcji hashowych, ważne z nich to:

- **Bcrypt: password_hash()** - Najbardziej bezpieczne haszowanie haseł, wolne obliczeniowo, używa wewnętrznej soli i haszuje iteracyjnie.
- **md5()** - Bardzo szybka funkcja nadająca się do haszowania plików. Wyjście ma zawsze 32 znaki.
- **sha1()** - Szybka funkcja haszująca do haszowania plików, używana wewnętrznie przez Git do haszowania commitów. Dane wyjściowe mają zawsze 40 znaków.

Hashing
-----------

```php
$password = 'secret-password';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> Ostrzeżenie:** Ani `md5()` ani `sha1()` nie nadają się do haszowania haseł, ponieważ jest obliczeniowo łatwo odkryć oryginalne hasło, lub przynajmniej wstępnie obliczyć hasła. Znacznie lepiej jest używać `bcrypt`, który został opracowany do haszowania haseł.
>
> Strona <a href="https://www.md5cracker.com/">md5cracker.com</a> zawiera bazę danych sum kontrolnych (hashy), spróbuj wyszukać hash: `79c2b46ce2594ecbc5b73e928345492`, jak widać, tak więc czyste `md5()` nie jest aż tak bezpieczne dla zwykłych słów i haseł.

Jedyne poprawne rozwiązanie: `Bcrypt + sól`.
--------------------------------------

W wykładzie <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Jak nie narozrabiać w płaszczyźnie docelowej</a>, David Grudl zajął się sposobami prawidłowego hashowania i przechowywania haseł.

Jedyne poprawne rozwiązanie to: `Bcrypt + sól`.

Konkretnie:

```php
$password = 'hasz';

// Generuje bezpieczny hash
echo password_hash($password, PASSWORD_BCRYPT);

// Alternatywnie z większą złożonością (domyślnie 10):
echo password_hash($password, PASSWORD_BCRYPT, ['koszt' => 12]);
```

Zaletą Bcryp jest przede wszystkim szybkość i automatyczne solenie.

Fakt, że generowanie trwa **długo**, powiedzmy 100 ms, sprawia, że testowanie wielu haseł jest dla atakującego bardzo kosztowne.

Dodatkowo, wyjściowy hash jest automatycznie traktowany **losową solą**, co oznacza, że przy wielokrotnym haszowaniu tego samego hasła, wyjściem jest zawsze inny hash. Dlatego atakujący nie będzie mógł użyć wstępnie obliczonej tablicy hashowej.

Dlatego nie będziemy mogli zweryfikować poprawności hasła poprzez wielokrotne haszowanie, lecz będziemy musieli wywołać specjalistyczną funkcję:

```php
if (password_verify($password, $hash)) {
    // Hasło jest poprawne
} else {
    // Hasło jest nieprawidłowe
}
```

Solenie haseł
------------

Aby utrudnić łamanie haszy, dobrze jest wstawić do oryginalnego wejścia jakiś dodatkowy ciąg znaków. Najlepiej przypadkowy. Proces ten nazywany jest **soleniem hasła**.

Bezpieczeństwo opiera się na idei, że atakujący nie będzie mógł użyć wstępnie obliczonej tabeli haseł i haszy, ponieważ nie będzie znał soli i będzie musiał złamać hasła indywidualnie.

Na przykład:

```php
$password = 'tajny_paszport';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // drukuje oryginalne hasło
echo $hash;     // wypisuje hash hasła wraz z solą
```

Złożone funkcje haszujące
------------------------

Możesz myśleć, że dobrym pomysłem byłoby wielokrotne wykonywanie funkcji haszującej, zwiększając w ten sposób złożoność złamania go, ponieważ oryginalne hasło będzie musiało być wielokrotnie haszowane.

Na przykład:

```php
$password = 'hasło';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

Paradoksalnie, trudność przebicia się zmniejsza się lub pozostaje prawie taka sama.

Powodem jest to, że funkcja `md5()` jest niezwykle szybka i może obliczyć ponad milion haseł na sekundę na zwykłym komputerze, więc próbowanie haseł jedno po drugim nie spowalnia zbytnio.

Drugi powód to bardziej teoria, a mianowicie możliwość natrafienia na tzw. kolizję. Jeśli hashujemy hasło wielokrotnie, z czasem może się zdarzyć, że trafimy na hash, który atakujący już zna, a to pozwoli mu na hashowanie hasła przy użyciu bazy danych.

Dlatego lepiej jest użyć wolnej bezpiecznej funkcji haszującej i wykonać haszowanie tylko raz, jednocześnie traktując końcowe wyjście z soleniem.

Niezwykle bezpieczne porównanie dwóch hashy/ciągów znaków
---------------------------------------------------

Czy wiesz, że operator == nie jest najbezpieczniejszym wyborem do porównywania haseł w weryfikacji haseł?

Podczas porównywania ciągów przechodzi przez dwa ciągi znak po znaku, aż dojdzie do końca (sukces, są takie same) lub nie ma różnicy (ciągi są różne).

I to może być wykorzystane w ataku. Jeśli mierzysz czas wystarczająco dokładnie, możesz oszacować, ile jeszcze znaków pozostało do dodania, aby uzyskać dokładne dopasowanie i dotrzeć do końca, lub możesz oszacować, jak daleko ciągi przeszły podczas porównywania ciągów.

Rozwiązaniem jest użycie funkcji hash_equals() wszędzie tam, gdzie porównywane są ciągi znaków i miałoby to znaczenie, gdyby atakujący mógł poznać pozycję, w której porównanie się nie powiodło.

A w jaki sposób funkcja to robi? Sprawia, że porównanie dowolnych 2 ciągów zawsze trwa tyle samo czasu, więc nie można powiedzieć, mierząc czas, gdzie wystąpiła różnica. Uważam, że niektóre rodzaje ataków są naprawdę bardzo mało prawdopodobne i trudne do zrealizowania. To jest jeden z nich.
