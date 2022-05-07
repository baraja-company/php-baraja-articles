Jak działa Captcha (obrazek opisowy)
====================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	pl: jak-dziala-captcha-obrazek-opisowy
> 
> perex:
> 	- 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> 	- 'Captcha to rodzaj obrazka, który weryfikuje, czy użytkownik nie jest robotem i chroni aplikację internetową. Opcje implementacji PHP.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

Captcha jest obecnie jednym z najbardziej rozpowszechnionych sposobów ochrony wolnych formatów. Pierwotnie został stworzony nie po to, by chronić bezpieczeństwo danych, ale by chronić przed spamem i rozpoznawać, że jest to człowiek.

Jest ona generowana maszynowo, więc nie zawsze jest idealna, ale wskaźnik powodzenia testu captcha wynosi około 99%, a tylko 1% obrazów może zostać rozszyfrowanych przez dobrze przygotowanego robota spamowego.

Jak to działa?
--------------------------

Istnieje więcej opcji, ja na przykład korzystam z tego rozwiązania:

- Serwer otrzymuje informację, że użytkownik zażądał strony z formularzem i rozpoczyna jej generowanie.
- W przyszłym polu testowym captcha umieszczany jest obrazek z tajnym adresem, który nic nie mówi (np. losowa liczba, ciąg znaków, ... cokolwiek).
- Obraz jest generowany i zapisywany pod tym adresem. Jednocześnie poprawny zapis jest gdzieś zapisywany (być może do sesji lub bazy danych).
- Gdy użytkownik przesyła formularz, sprawdzane jest, czy transkrypt jest zgodny z tym, co wprowadził. Jeśli tak, formularz zostanie przetworzony. Jeśli nie, należy poprosić o ponowne opisanie innego obrazu.
- Po udanych i nieudanych próbach tymczasowy obrazek captcha jest usuwany (nie będzie już nigdy więcej używany). Jeśli formularz nie zostanie przesłany w określonym czasie, zostanie również usunięty (wygaśnie).

Poprawna transkrypcja
--------------------------

Jeśli test captcha da się rozwiązać, użytkownik jest prawdopodobnie człowiekiem. Dobrze jest jednak pomyśleć o użytkownikach, którzy nie są w stanie wykonać zadania, zwłaszcza o użytkownikach niewidomych. Dobrym rozwiązaniem jest łączenie wielu możliwych testów (zwłaszcza prefetching głosu). Jednak obecnie rozpoznawanie głosu przez maszynę jest znacznie bardziej efektywne niż odczytywanie obrazu. Dlatego to rozwiązanie nie zawsze jest idealne.

Niestandardowy prosty obrazek captcha
--------------------------

Dość często wystarczy wygenerować pusty obraz o określonych wymiarach i wprowadzić do niego kilka znaków w sposób czytelny bez dalszej edycji. Poważnie! Większość botów spamowych jest głupia i nie potrafi atakować formularzy ogólnych z tego typu zabezpieczeniami, mimo że jest to doskonale czytelny tekst, który lepszy OCR może doskonale przepisać.

Otrzymany obraz może wyglądać następująco:

```txt
<img src="captcha.php" alt="ukázková captcha">
```

Spróbuj odświeżyć stronę kilka razy, a zobaczysz, że kod zmienia się losowo za każdym razem. W celach demonstracyjnych jest on generowany bez zapisywania, a więc jest usuwany natychmiast po załadowaniu.

Kod źródłowy rozwiązałem za pomocą biblioteki PHPGD, która jest dostępna praktycznie na każdej instalacji i hostingu PHP:

```php
Header("Content-type: image/png");
$obr = ImageCreate(100, 35);
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //definicja koloru tła
$bila = ImageColorAllocate ($obr, 255, 255, 255); //definicja koloru białego dla tekstu
$styl = array ($pozadi);
ImageSetStyle ($obr, $styl);
  $nahodne_cislo = rand(11111,99999); //wylosowanie losowej liczby o długości 5 znaków
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); //funkcja do rysowania tekstu (w tym przypadku liczby)
ImagePNG($obr); //generowanie obrazu w pamięci i renderowanie
ImageDestroy($obr); //usuń obraz z pamięci (nie będzie już potrzebny, ponieważ został wygenerowany raz)
```

Renderowanie obrazu jest wtedy tylko kwestią języka HTML:

```html
<img src="captcha.php">
```

Należy pamiętać, że ten skrypt nie działa samodzielnie bez dalszych modyfikacji. Służy on jedynie jako demonstracja generowania prostego obrazu.

Streszczenie
--------------------------

Testy Captcha są dość wiarygodne, ale irytujące i czasochłonne. Czasami nie są one czytelne, dlatego dobrze jest dać użytkownikowi możliwość załadowania innego obrazu za pomocą javascript, aby nie trzeba było ponownie ładować całej strony i wypełniać wszystkiego od nowa.

Pamiętaj: To, co jest trywialnym zadaniem dla człowieka, może nigdy nie być osiągalne dla maszyny. Dlatego nawet ta prymitywna captcha z doskonale czytelnym tekstem może chronić przed ponad połową spamu.
