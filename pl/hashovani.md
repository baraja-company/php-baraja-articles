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
> sourceContentHash: '5d1e289fd93e18ad73eb23ee1bbba8ee'

W procesie haszowania (w przeciwieństwie do szyfrowania) na podstawie danych wejściowych tworzone jest wyjście, z którego nie można już uzyskać oryginalnego ciągu znaków.

Dzięki temu dobrze nadaje się do ochrony poufnych ciągów znaków, haseł i sum kontrolnych.

Inną zaletą funkcji haszujących jest to, że zawsze generują one dane wyjściowe o tej samej długości, a niewielka zmiana danych wejściowych zawsze całkowicie zmienia całe dane wyjściowe.

Funkcje haszowania
----------------

W PHP istnieje wiele funkcji haszujących, najważniejsze z nich to:

- **Bcrypt: password_hash()** - najbezpieczniejsze haszowanie haseł, wolne obliczeniowo, używa wewnętrznej soli i haszuje iteracyjnie.
- **md5()** - Bardzo szybka funkcja nadająca się do haszowania plików. Dane wyjściowe mają zawsze 32 znaki.
- **sha1()** - Szybka funkcja haszująca dla haszowania plików, używana wewnętrznie przez Git do haszowania commitów. Dane wyjściowe mają zawsze 40 znaków.

Hashing
-----------

```php
$password = 'secret-password';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Ostrzeżenie:** Ani `md5()`, ani `sha1()` nie są odpowiednie do haszowania haseł, ponieważ łatwo jest obliczeniowo odkryć oryginalne hasło lub przynajmniej wstępnie obliczyć hasła. O wiele lepiej jest używać `bcrypt`, który został stworzony do haszowania haseł.
>
> Na stronie <a href="https://www.md5cracker.com/">md5cracker.com</a> znajduje się baza danych sum kontrolnych (hashy), spróbuj wyszukać hash: `79c2b46ce2594ecbcb5b73e928345492`, jak widać, czysty algorytm `md5()` nie jest tak bezpieczny dla popularnych słów i haseł.

Jedyne poprawne rozwiązanie: `Bcrypt + sól`.
--------------------------------------

W wykładzie <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Jak nie narobić bałaganu w płaszczyźnie docelowej</a> David Grudl omówił sposoby prawidłowego haszowania i przechowywania haseł.

Jedynym poprawnym rozwiązaniem jest: `Bcrypt + sól`.

Konkretnie:

```php
$password = 'hasz';

// Generuje bezpieczny hash
echo password_hash($password, PASSWORD_BCRYPT);

// Alternatywnie z większą złożonością (domyślnie 10):
echo password_hash($password, PASSWORD_BCRYPT, ['koszt' => 12]);
```

Zaletą systemu Bcrip jest przede wszystkim jego szybkość i automatyczne solenie.

Fakt, że generowanie hasła trwa **długo**, powiedzmy 100 ms, sprawia, że testowanie wielu haseł jest bardzo kosztowne dla atakującego.

Ponadto hasz wyjściowy jest automatycznie traktowany **solą losową**, co oznacza, że przy wielokrotnym haszowaniu tego samego hasła na wyjściu zawsze otrzymuje się inny hasz. Dlatego atakujący nie będzie mógł użyć wstępnie obliczonej tablicy haseł.

W związku z tym nie będziemy w stanie zweryfikować poprawności hasła za pomocą wielokrotnego haszowania, lecz będziemy musieli wywołać specjalizowaną funkcję:

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

Bezpieczeństwo opiera się na założeniu, że osoba atakująca nie będzie w stanie użyć wstępnie obliczonej tabeli haseł i skrótów, ponieważ nie będzie znała soli i będzie musiała łamać hasła indywidualnie.

Na przykład:

```php
$password = 'tajny_paszport';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // drukuje oryginalne hasło
echo $hash;     // wypisuje skrót hasła wraz z solą
```

Złożone funkcje skrótu
------------------------

Można by pomyśleć, że dobrym pomysłem byłoby wielokrotne wykonywanie funkcji haszującej, co zwiększyłoby złożoność jej złamania, ponieważ oryginalne hasło będzie musiało być wielokrotnie haszowane.

Na przykład:

```php
$password = 'hasło';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

Paradoksalnie, trudność przebicia się zmniejsza się lub pozostaje prawie taka sama.

Powodem jest to, że funkcja `md5()` jest bardzo szybka i może obliczyć ponad milion haseł na sekundę na zwykłym komputerze, więc próbowanie haseł jedno po drugim nie spowalnia zbytnio działania.

Drugi powód jest bardziej teoretyczny, a mianowicie możliwość natrafienia na tzw. kolizję. Jeśli hashujemy hasło wielokrotnie, z czasem może się okazać, że trafimy na hash, który atakujący już zna, co pozwoli mu na hashowanie hasła przy użyciu bazy danych.

Dlatego lepiej jest użyć wolnej, bezpiecznej funkcji haszującej i wykonać haszowanie tylko raz, jednocześnie traktując końcowe dane wyjściowe soleniem.
