Szyfr Cezara - jak to działa
============================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	pl: szyfr-cezara---jak-to-dziala
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

> **Ostrzeżenie:** Ten artykuł został napisany wiele lat temu i niektóre informacje mogą być nieaktualne lub nieprawidłowe. Należy o tym pamiętać podczas lektury.

Szyfr Cezara jest jedną z najprostszych funkcji haszujących. W swoich czasach był praktycznie nie do złamania, ale w dobie nowoczesnych komputerów jego złamanie zajmuje tylko kilkadziesiąt sekund, a nawet kilka minut. Opiera się ona na kluczu, według którego wiadomość jest szyfrowana, a następnie według którego można ją ponownie rozwinąć. Klucz jest więc tajny. W momencie szyfrowania wiadomość może być widoczna i nic nie będzie znaczyć (tylko zlepek znaków). Jedynym sposobem złamania szyfru jest odgadnięcie klucza.

Klucz
--------------------------

Kluczem może być dowolna liczba całkowita, która ma mniej cyfr niż sam komunikat. Zazwyczaj podawane są 3 prawidłowe cyfry (istnieje więc 999 kombinacji). Każda dodatkowa cyfra zwiększa bezpieczeństwo. Aby dwie strony mogły się komunikować, obie muszą znać ten tajny klucz (muszą więc w jakiś bezpieczny sposób przekazać go sobie nawzajem). Jeśli klucz jest znany tylko im, a nie drugiej stronie, to wiadomość można rozprzestrzeniać nawet w sposób niezabezpieczony i potencjalnie nie naruszyć jej treści, ponieważ potencjalny napastnik nie zna procedury odzyskania wiadomości.

Do celów demonstracyjnych użyję klucza **123**, zwykle stosuje się jakąś inną liczbę losową, która z założenia nie jest tak łatwa do odgadnięcia.

Procedura szyfrowania
--------------------------

Zasada działania opiera się na zamianie znaków wiadomości na inne przy użyciu klucza. Ja nazywam to "zmianą postaci".

Na przykład mamy wiadomość, którą chcemy zaszyfrować:

```php
TAJNA ZPRAVA
```

Teraz przypisz numer każdemu znakowi. Zazwyczaj według alfabetu (jego numeru seryjnego). Będę używać alfabetu angielskiego, a więc tej serii znaków:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

Jeśli każdemu znakowi przypiszemy liczbę, otrzymamy coś takiego:

```php
20-1-10-14-1   26-16-18-1-22-1
```

Teraz przychodzi czas na klucz. Bierzemy każdy numer z osobna i dodajemy do niego klucz. Kolorem zaznaczam, co gdzie dodałem:

`1 2 3`

```php
21 3 13 15 3   3 17 20 4 23 3
```

Zauważ, że gdy szyfruję znak **Z**, zapisuję cyfrę 3. Dzieje się tak dlatego, że **Z** jest ostatnim znakiem alfabetu, więc gdy dojdzie do końca, jest liczony ponownie od początku wiersza.

Przekazywanie komunikatu
--------------------------

Teraz możemy przekazać wiadomość w dowolny sposób. Czasami nawet publicznie. Inni zobaczą tylko nielogiczną serię liczb, więc prawdopodobnie nawet nie zorientują się, że jest to jakiś szyfr. Bezpieczeństwo zwiększa również sam klucz, który jest tajny. Czasami należy przekazać komunikat w postaci serii liczb, a czasami należy przekształcić te liczby w znaki (ponownie, stosując tę samą serię alfabetyczną), a następnie przekazać sekwencję znaków. Wiele zależy od okoliczności. Na ogół jednak lepsza jest sekwencja numeryczna, ponieważ niewiele osób będzie podejrzewać, że jest to zakodowana wiadomość.

Odszyfrowanie u odbiorcy
--------------------------

Odbiorca odszyfrowuje przy użyciu tej samej procedury. Bierze każdy znak z osobna i odejmuje od niego liczby zgodnie z kluczem, a następnie przekształca otrzymane wartości z powrotem na znaki za pomocą alfabetu. Jest to po prostu procedura odwrotnego szyfrowania. Ważne jest, aby znać **klucz** i **zestaw znaków** - czyli kolejność znaków.

Szyfrowanie i odszyfrowywanie
--------------------------

Jedyną możliwą procedurą łamania jest wypróbowanie każdej możliwej kombinacji wszystkich potencjalnych kluczy. Jeśli nie znamy długości klucza, cały proces komplikuje się jeszcze bardziej. Ogólnie rzecz biorąc, współczesne komputery mogą wypróbować około 100 kluczy w ciągu jednej sekundy, więc złamanie losowego klucza o długości 3 znaków zajmuje około minuty.

Jeśli jednak klucz jest równie długi lub dłuższy niż oryginalna wiadomość, nie da się go złamać, ponieważ każdy znak ma swój własny klucz, więc dla każdego znaku trzeba wypróbować wszystkie kombinacje.

Gdybym miał wiadomość "**TAKE MESSAGE**", miałaby ona długość 11 znaków (spacje się nie liczą). Gdybym chciał klucz o długości 11 znaków i użył 26-znakowego ciągu alfabetu angielskiego, to istnieje **11^26** = 1,191817654*10²⁷ kombinacji, przeciętny komputer złamałby ten klucz w 1,310999419×10²⁶ sekund = 10^20 dni :)
