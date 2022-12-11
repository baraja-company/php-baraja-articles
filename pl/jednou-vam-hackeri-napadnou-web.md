Pewnego dnia hakerzy zaatakują Twoją stronę internetową
=======================================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	pl: pewnego-dnia-hakerzy-zaatakuja-twoja-strone-internetowa
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '0f0ac4c091115e9c1f0833d318f34f5d'

To ci się nie przytrafi? :DDDDDDDDD

Zarządzając ponad 300 stronami internetowymi, od czasu do czasu trafiają mi się różne sytuacje awaryjne. Czasami są one dość gorące, ale często jest to kompletny banał. Jeśli tak jak ja, w przeszłości kusiło Cię programowanie i wiesz też, że [programowanie to ból](http://borisovo.cz/programming-sucks-cz.html), to na pewno się ze mną zgodzisz.

W objęciach złych hakerów
----------------------

Kiedy aplikacja internetowa staje się popularna, staje się kuszącym celem dla hakerów. Ich motywacją zazwyczaj nie jest doprowadzenie do upadku całej witryny, wręcz przeciwnie. W rzeczywistości **hakerzy chcą, abyś jako administrator serwera był ich zupełnie nieświadomy**. Dobry haker czeka miesiącami, obserwując serwer WWW i zdobywając to, co najcenniejsze - Twoje dane.

Aby chronić swoich użytkowników, konieczne jest szyfrowanie wszystkich danych i posiadanie wielu warstw ochrony. Dla haseł użyj jednej z wolnych funkcji, takich jak [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 lub Scrypt. Michał Spacek pisał już o [ocenie bezpieczeństwa](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), dziękuję mu za uzupełnienie artykułu. Jako właściciel strony, musisz nalegać na prawidłowe hashowanie hasła podczas dyskusji z programistą. Inaczej będziesz płakać, tak jak wielu ludzi przed tobą, którzy też łudzili się, że to ich nie dotyczy.

Czy potrafisz powiedzieć, kiedy jesteś atakowany?
---------------------------------

Zauważyłem, że gdy serwer internetowy osiągnie pewien próg ruchu (w Czechach jest to około 50+ odwiedzających dziennie), zaczyna dostawać serię ataków skierowanych na niego znikąd. Żeby nie było łatwo, atakujący zawsze ma przewagę, bo może wybrać, którą część infrastruktury zacznie atakować, a Ty - jako właściciel serwera WWW - musisz to w porę rozpoznać, obronić się i następnym razem być na to lepiej przygotowanym.

Ile ataków zostało skierowanych na Ciebie np. w ciągu ostatniego miesiąca? Czy wiesz dokładnie? Ile z nich udało ci się obronić? Czy ktoś jeszcze ma dostęp do Twojego serwera?

A co jeśli ktoś cię teraz atakuje? A teraz... a teraz także...?

Niestety, nie ma jednego uniwersalnego sposobu na rozpoznanie ataku. Są jednak narzędzia, które mogą dać Ci znaczną przewagę.

To, co dla mnie ostatnio działa najlepiej:

- **Kiedy atakujący nie wie, gdzie fizycznie znajdują się Twoje serwery**, ma znacznie trudniejszą pozycję. Informacje o architekturze fizycznej można bardzo dobrze ukryć za pomocą [Cloudflare](https://www.cloudflare.com/), który proxy (i dwukierunkowo szyfruje) całą komunikację sieciową. Ma również szereg innych zalet, takich jak szybsze pobieranie z zagranicy, automatyczna ochrona przed atakami DDOS, kompresja aktywów i wreszcie automatyczne blokowanie nieuczciwych gości. Używam Cloudflare dla wszystkich moich stron internetowych (i prawie każdy projekt wykorzystuje inne technologie).
- **Logowanie błędów**. Zawsze i wszystko. Szanse są takie, że Twoja aplikacja zawiera wiele błędów popełnionych przez Twojego programistę. Czy to [uncaught exceptions](https://php.baraja.cz/vyjimky), błędy aplikacji, SQL injection i tak dalej. Jeśli programujesz w PHP i nie rozumiesz logowania, to jest wystarczająco dużo problemów, które wyłapie [Tracy](https://tracy.nette.org/) (i ewentualnie inne narzędzia jak [Sentry](https://sentry.io/)) oraz logi dostępu na samym serwerze. Rejestrowanie błędów nie jest miłym dodatkiem, to konieczność. Rejestruję błędy na wszystkich stronach, którymi się aktywnie opiekuję, a informacje o nowych błędach wysyłam na mój e-mail, więc wiem o nich natychmiast.
- **Bezpieczna i zaktualizowana platforma**. Czy masz WordPressa na swojej stronie? Czy wiesz, jak go prawidłowo zaktualizować? Czy znasz wszystkie zagrożenia, na które jesteś narażony? Gdy coś jest za darmo, to prawie zawsze kosztem czegoś innego. Osobiście do tworzenia aplikacji używam [Nette framework](https://nette.org/cs/) w wersji 3, który aktualizuję co tydzień i aktywnie szukam luk bezpieczeństwa do załatania. Tak jak regularnie serwisujesz swój samochód, tak samo musisz regularnie, co najmniej raz w miesiącu, serwisować swoją stronę internetową. Utrzymanie witryny obejmuje aktualizację wszystkich zewnętrznych bibliotek i konsekwentnie monitorowane logi. Jeśli używasz starej wersji biblioteki, jest bardzo prawdopodobne, że atakujący wie o luce i będzie próbował ją wykorzystać.
Pytanie brzmi: kiedy
Duża liczba osób w mojej okolicy w niesamowity sposób bagatelizuje bezpieczeństwo i wystawia do Internetu treści, które bardzo łatwo złamać i wykorzystać.

W rzeczywistości nawet duże firmy popełniają błędy, nawet w tak banalnych sprawach jak atak [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

Pytanie nie brzmi, czy ktoś kiedykolwiek złamie twoją stronę. Pytanie brzmi, kiedy to nastąpi i czy będziesz wystarczająco przygotowany, aby sobie z tym poradzić. Może haker ma dostęp do Twojego serwera od miesięcy, a Ty o tym nie wiesz (jeszcze).

Co robić, gdy pojawią się kłopoty
-------------------------------

Kiedy dowiadujesz się o pracy hakera, zwykle jest już za późno i haker zrobił wszystko, co było mu potrzebne. Bardzo prawdopodobne, że umieścił złośliwe oprogramowanie na całym serwerze i ma przynajmniej częściową kontrolę nad maszyną.

To jest **chaotyczny rodzaj problemu i trzeba działać szybko**.

Ponieważ jest to ostatni etap ataku, osobiście zalecam całkowite odłączenie serwera WWW od internetu i rozpoczęcie stopniowego dochodzenia do tego, co się wszystko stało. Dodatkowo nigdy nie dowiemy się dokładnie, czy serwer nie ukrywa czegoś innego. Atakujący zawsze ma znaczną przewagę.

Jeśli zdecydujemy się na umieszczenie ostatniej działającej kopii zapasowej z powrotem na serwerze, bardzo pomożemy atakującemu. On już wie jak włamać się na Twój serwer i nic nie stoi na przeszkodzie, aby za kilka godzin zrobił to ponownie. Ponadto kopia zapasowa prawdopodobnie zawiera złośliwe oprogramowanie lub jakiś inny rodzaj wirusa.

Chociaż jest to bardzo niepopularna opcja wśród moich klientów, osobiście zawsze polecam w pierwszej kolejności **całkowite usunięcie stron WordPressa** lub wszelkich systemów, których klient nie aktualizuje, a które mają dużo zagrożeń bezpieczeństwa. Na przykład, jeśli na jednym serwerze hostujesz 30 stron, które mają więcej niż 2 lata, masz praktycznie gwarancję, że przynajmniej jedna z nich zawiera jakąś formę luki.

Jeśli nie rozumiesz zagadnienia, lepiej zawczasu skontaktuj się z odpowiednim specjalistą, który ma głęboką wiedzę na temat Twojego zagadnienia i rozumie sprawy w szerokim kontekście.

**Uwaga etyczna:**

Jeśli zarządzasz stroną internetową dla klienta, który miał incydent bezpieczeństwa, Twoim obowiązkiem jest poinformowanie go o tym. W przypadku wycieku jakichkolwiek danych (takich jak hasła, ale często znacznie więcej) należy również poinformować użytkowników końcowych, że ich hasło jest wiedzą publiczną i powinni je wszędzie zmienić. Jeśli użyjesz wolnej funkcji haszującej (na przykład **Bcrypt + sól**), znacznie utrudnisz atakującemu złamanie haseł (niestety, [Bcrypt passwords can be cracked](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Jeśli chcesz otrzymywać regularne informacje o tym, czy Twoje konto zostało wycieknięte, polecam zarejestrować się w [Have i been pwned?](https://haveibeenpwned.com/). Więcej informacji na temat [naruszenia bezpieczeństwa](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni) można znaleźć na stronie internetowej OOO.

Atomowe nawyki
--------------

W ciągu ostatnich sześciu miesięcy skończyłem czytać książkę [Atomowe nawyki](https://www.melvil.cz/kniha-atomove-navyky/), która była otwieraczem oczu. Opisuje proces przyrostowej poprawy o 1% każdego dnia, który w dłuższej perspektywie zrobi bardzo dużo.

Ponieważ zgłaszają się do mnie firmy i osoby na różnych etapach zadłużenia technologicznego, chciałbym podsumować najgorsze rzeczy:

- **FTP do komunikacji z serwerem nie ma sensu**. Jest to niezabezpieczony protokół, który jest kontrolowany za pomocą hasła. Jeśli chcesz dać komuś dostęp, musi znać hasło i może zrobić to samo, co ty. Jeśli chcesz odebrać mu dostęp, nie możesz, jedynym sposobem jest zmiana hasła. Lepiej całkowicie przejść na SSH i używać kluczy RSA. Dość często jestem zaskoczony, że nawet firmy rozwiązują ten problem w dzisiejszych czasach. Nigdy nie rób jednej tabeli wszystkich haseł. A już na pewno nigdy wspólnego dla całego zespołu!
- **Kod źródłowy należy do Git**, dane do bazy danych, a zawartość statyczna (obrazy, PDF-y, ...) do plików na dysku. Wybór złego przechowywania danych pogarsza projekt aplikacji i bezpieczeństwo. Adres URL do pliku PDF (na przykład z fakturą) powinien zawierać losowo wygenerowaną treść, którą zna tylko odbiorca. W przypadku wyjątkowo wrażliwych treści (jak np. wyciąg z banku) należy zaszyfrować je na dysku i udostępnić dopiero po zalogowaniu.
- Niepubliczne części strony muszą zawierać nagłówek META `noindex` i `nofollow`, aby nie zostały zaindeksowane przez Google. Wrażliwe treści, których nie może poznać Twoja konkurencja, muszą być chronione hasłem.
- Wyłącz [directory content listing](https://www.simplified.guide/apache/disable-directory-listing) na swoim serwerze. W przypadku niewłaściwego ustawienia cała witryna może być przeszukiwana maszynowo w celu znalezienia wrażliwych plików.
- Projekt korzeń! Nigdy nie może być bezpośredniego adresu URL do plików konfiguracyjnych. Na przykład [Nette does it right](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) zaraz po pierwszym tutorialu. To samo dotyczy [publicznie dostępnych repozytoriów git](https://smitka.me/open-git/). Tego typu podatności niezwykle łatwo jest zbagatelizować, a konsekwencje bywają tragiczne.
- Bug może również powstać w wyniku nieumyślnego błędu deweloperskiego. Bardzo ważne jest, aby dla każdej zmiany wykonać przegląd kodu przez żywego człowieka. Wiele błędów jest odkrywanych przez [PHPStan](https://github.com/phpstan/phpstan) lub podobne narzędzia zanim człowiek zobaczy kod.
- Nigdy nie [wysyłaj haseł w czytelnej formie mailem](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), zawsze jest inny sposób rozwiązania.
- Problemy z bezpieczeństwem w starych wersjach bibliotek i oprogramowania. Roboty pełzają po internecie co sekundę i atakują znane luki w zabezpieczeniach (SQL injection, XSS, CSRF, ...). Wiele znanych dziur mają starsze wersje WordPressa.

Podatności jest znacznie więcej, a problemy różnią się w zależności od projektu. Jeśli potrzebujesz zrobić szybki audyt serwera, polecam użyć [Maldet](https://www.rfxn.com/projects/linux-malware-detect/), a następnie zatrudnić odpowiedniego specjalistę, który pomoże Ci zrobić [pełny audyt strony](https://baraja.cz/audit-webu), i to nie tylko ze względów bezpieczeństwa.

Inżynierowie bezpieczeństwa są jak plastik w oceanie - tylko na zawsze. Przyzwyczaj się do tego.

Zasada akcji i reakcji w przyrodzie
-------------------------------

Zauważyliście również, że natura prawie zawsze stosuje zasadę reakcji. Oznacza to, że w danym środowisku coś się dzieje i otaczające nas organizmy reagują na to w różny sposób. Takie podejście ma wiele zalet, ale jedną ogromną wadę - zawsze pozostaniesz w tyle.

Jako osoba myśląca (projektant, developer, konsultant) masz ogromną przewagę, a jest nią bycie actionable - czyli znajomość z wyprzedzeniem pewnej części dużych zagrożeń i aktywne zapobieganie im w pierwszej kolejności. Być może nigdy nie uda się zapobiec wszystkim wpadkom, ale można przynajmniej złagodzić ich konsekwencje lub zbudować mechanizmy kontrolne, które wcześnie wykryją problemy, aby mieć czas na reakcję.

Większość ataków odbywa się przez długi okres czasu, a jeśli logujesz się, zwykle będziesz miał wystarczająco dużo czasu, aby wykryć problem.
