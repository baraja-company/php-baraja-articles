Jak złamać funkcję md5
======================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	pl: jak-zlamac-funkcje-md5
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 jest bardzo często używaną funkcją do obliczania haszy.

Początkujący użytkownicy często używają go do <a href="/hashovani">hashowania haseł</a>, co nie jest dobrym pomysłem, ponieważ istnieje wiele sposobów na odzyskanie oryginalnego hasła.

W tym artykule opisano konkretne metody pozwalające to osiągnąć.

Złożoność czasowa
----------------

Wszystkie zabezpieczenia opierają się na fakcie, że wypróbowanie wszystkich haseł zajmuje nieproporcjonalnie dużo czasu. No cóż, powinno. Problem z algorytmem `md5()` polega w szczególności na tym, że jest to bardzo szybka funkcja. Na zwykłym komputerze bez problemu można obliczyć ponad milion hashy na sekundę.

Jeśli złamiemy hasło, próbując kolejno jego kombinacji, jest to **atak brute force**.

Metody krakowania
----------------

Istnieje kilka strategii:

- Sukcesywne testowanie metodą prób i błędów (atak brute force)
- Testowanie haseł słownikowych
- Tablice tęczowe (baza danych z wstępnie obliczonymi hashami)
- Wyszukiwania w Google
- Kolizje trafień w algorytmie

Istnieje wiele innych metod, w tym artykule opisano tylko te najczęściej stosowane.

Strategie łamania metodą Brute Force
-----------------------------

Próbowane są po kolei wszystkie kombinacje liter, cyfr i innych znaków.

Wygenerowane próby są kolejno haszowane i porównywane z oryginalnym haszem.

Tak więc, na przykład:

```php
aaaaaaabaaacaaadaaaeaaaf...
```

Problem z tym atakiem leży w samym algorytmie `md5()`. Gdybyśmy mieli wypróbować tylko małe litery alfabetu angielskiego i cyfry, wypróbowanie wszystkich kombinacji na powszechnie dostępnym komputerze zajęłoby najwyżej kilkadziesiąt minut.

Dlatego ważne jest, aby wybierać długie hasła, najlepiej losowe, zawierające znaki specjalne.

Strategia ataku słownikowego
----------------------------

Ludzie zazwyczaj wybierają słabe hasła, które istnieją w słowniku.

Jeśli skorzystamy z tego faktu, możemy szybko odrzucić mało prawdopodobne warianty, takie jak `6w1SCq5cs`, i zamiast tego odgadnąć istniejące słowa.

Co więcej, z poprzednich wycieków haseł dużych firm wiemy, że użytkownicy wybierają dużą literę na początku hasła i cyfrę na końcu. Zastanówmy się - czy Twoje hasło też tak ma? :)

Tablice tęczowe - wstępnie obliczona baza danych
--------------------------------------

Ponieważ jedno hasło zawsze odpowiada temu samemu hashowi, łatwo jest przeliczyć ogromną bazę danych, w której hasła będą wyszukiwane w pierwszej kolejności.

W rzeczywistości wyszukiwanie jest zawsze o rzędy wielkości szybsze niż wielokrotne przeszukiwanie haszy.

Ponadto, w przypadku większych wycieków danych, hasła mogą być w ten sposób równolegle haszowane, dzięki czemu można szybko odzyskać np. 10% wszystkich haseł użytkowników.

Dobrą bazą danych haseł jest na przykład <a href="https://crackstation.net/">Crack Station</a>.

Wyszukiwanie w Google
-------------------

Wiele prostych haseł jest znanych bezpośrednio firmie Google, ponieważ indeksuje ona strony zawierające hashe.

Zawsze korzystam z Google jako pierwszej opcji. :)

Znajdowanie kolizji w algorytmie
--------------------------

Zasada <a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Zasada Dirichleta</a> mówi, że jeśli mamy zbiór haseł, które zawsze mają 32 znaki, to istnieją co najmniej 2 różne hasła o długości 33 znaków (jedno dłuższe), które generują ten sam hash.

W praktyce szukanie kolizji nie ma sensu, ale czasami sam autor aplikacji ułatwia zgadywanie, przeliczając kolizje.

Na przykład:

```php
$password = 'hasło';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

W tym przypadku sensowne jest odgadnięcie kolizji, a nie oryginalnego hasha.

Zdrowie za pieczenie!
