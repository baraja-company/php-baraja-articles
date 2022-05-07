Pewnego dnia hakerzy zaatakują Twoją witrynę
============================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	pl: pewnego-dnia-hakerzy-zaatakuja-twoja-witryne
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: a74c964823fb0c4231b711d113055a24

To ci się nie przytrafi? :DDDDDDDDD

Zarządzając ponad 300 witrynami internetowymi, od czasu do czasu spotykam się z różnymi sytuacjami awaryjnymi. Czasami są one dość gorące, ale często jest to zupełna błahostka. Jeśli, tak jak ja, w przeszłości kusiło Cię programowanie i wiesz, że [programowanie to ból](http://borisovo.cz/programming-sucks-cz.html), na pewno się ze mną zgodzisz.

W objęciach złych hakerów
----------------------

Gdy aplikacja internetowa staje się popularna, staje się kuszącym celem dla hakerów. Ich motywacją zazwyczaj nie jest doprowadzenie do awarii całej witryny, wręcz przeciwnie. W rzeczywistości **hakerzy chcą, abyś jako administrator serwera był całkowicie nieświadomy ich istnienia**. Dobry haker czeka miesiącami, obserwując serwer WWW i zdobywając to, co najcenniejsze - Twoje dane.

Aby chronić użytkowników, konieczne jest szyfrowanie wszystkich danych i stosowanie wielu warstw zabezpieczeń. W przypadku haseł należy użyć jednej z wolnych funkcji, takich jak [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 lub Scrypt. Michal Spacek napisał już o [rating bezpieczeństwa](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes) i dziękuję mu za uzupełnienie artykułu. Jako właściciel witryny, podczas rozmów z programistą musisz nalegać na prawidłowe haszowanie haseł. W przeciwnym razie będziesz płakać, tak jak wielu ludzi przed tobą, którzy również łudzili się, że to ich nie dotyczy.

Czy potrafisz rozpoznać, kiedy jesteś atakowany?
---------------------------------

Zauważyłem, że gdy serwer WWW osiągnie pewien próg ruchu (w Czechach jest to około 50+ odwiedzających dziennie), zaczyna być atakowany znikąd. Aby nie było łatwo, atakujący zawsze ma przewagę, ponieważ może wybrać, którą część infrastruktury zacząć atakować, a Ty - jako właściciel serwera WWW - musisz to w porę rozpoznać, obronić się i być lepiej przygotowanym na atak następnym razem.

Ile ataków skierowano na Ciebie na przykład w ciągu ostatniego miesiąca? Czy wiesz dokładnie? Ilu z nich udało Ci się obronić? Czy ktoś inny ma dostęp do Twojego serwera?

Co zrobić, jeśli ktoś właśnie teraz Cię atakuje? A teraz... a teraz także...?

Niestety, nie ma jednego uniwersalnego sposobu na rozpoznanie ataku. Istnieją jednak narzędzia, które mogą dać Ci znaczną przewagę.

To, co ostatnio najlepiej mi się sprawdza:

- Jeśli atakujący nie wie, gdzie fizycznie znajdują się serwery**, ma o wiele trudniejszą sytuację. Informacje o architekturze fizycznej można bardzo dobrze ukryć za pomocą rozwiązania [Clouflare](https://www.cloudflare.com/), które pośredniczy (i dwukierunkowo szyfruje) w całej komunikacji sieciowej. Ma ona również wiele innych zalet, takich jak szybsze pobieranie danych z zagranicy, automatyczna ochrona przed atakami DDOS, kompresja zasobów i wreszcie automatyczne blokowanie nieuczciwych użytkowników. Używam Cloudflare dla wszystkich moich witryn (a prawie każdy projekt wykorzystuje inne technologie).
- **Logowanie błędów**. Zawsze i we wszystkim. Istnieje duże prawdopodobieństwo, że Twoja aplikacja zawiera wiele błędów popełnionych przez programistę. Może to być [uncaught exceptions](https://php.baraja.cz/vyjimky), błędy aplikacji, wstrzyknięcie kodu SQL itd. Jeśli programujesz w PHP i nie masz pojęcia o logowaniu, jest wystarczająco dużo problemów, które wyłapie [Tracy](https://tracy.nette.org/) (i ewentualnie inne narzędzia, takie jak [Sentry](https://sentry.io/)) oraz logi dostępu na samym serwerze. Rejestrowanie błędów nie jest czymś, co warto mieć, ale jest koniecznością. Rejestruję błędy na wszystkich witrynach, którymi się opiekuję, a informacje o nowych błędach wysyłam na mój adres e-mail, dzięki czemu natychmiast się o nich dowiaduję.
- **Bezpieczna i zaktualizowana platforma**. Czy masz na swojej stronie system WordPress? Czy wiesz, jak prawidłowo zaktualizować system? Czy znasz wszystkie rodzaje ryzyka, na które jesteś narażony? Gdy coś jest za darmo, prawie zawsze odbywa się to kosztem czegoś innego. Osobiście do tworzenia aplikacji używam [Nette framework](https://nette.org/cs/) w wersji 3, który aktualizuję co tydzień i aktywnie poszukuję luk w zabezpieczeniach, które należy załatać. Tak jak regularnie serwisujesz swój samochód, tak samo musisz regularnie, przynajmniej raz w miesiącu, serwisować swoją stronę internetową. Konserwacja witryny obejmuje aktualizację wszystkich bibliotek zewnętrznych oraz stałe monitorowanie dzienników. Jeśli używasz starej wersji biblioteki, jest bardzo prawdopodobne, że osoba atakująca wie o tej luce i będzie próbowała ją wykorzystać.
Pytanie brzmi: kiedy
Wiele osób z mojego otoczenia w niewiarygodny sposób lekceważy bezpieczeństwo i udostępnia w Internecie treści, które bardzo łatwo złamać i wykorzystać.

W rzeczywistości nawet duże firmy popełniają błędy, nawet w tak trywialnych sprawach, jak atak [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

Nie chodzi o to, czy ktoś kiedykolwiek złamie Twoją stronę. Pytanie brzmi: kiedy to nastąpi i czy będziesz wystarczająco przygotowany, aby sobie z tym poradzić. Być może haker ma dostęp do Twojego serwera od miesięcy, a Ty o tym nie wiesz (jeszcze).

Co robić, gdy pojawią się problemy?
-------------------------------

Gdy użytkownik dowiaduje się o pracy hakera, jest już zwykle za późno, a haker zrobił wszystko, co było do zrobienia. Bardzo prawdopodobne, że umieścił on oprogramowanie mallware na całym serwerze i ma przynajmniej częściową kontrolę nad maszyną.

Jest to **niebezpieczny problem i trzeba działać szybko**.

Ponieważ jest to ostatni etap ataku, osobiście zalecam całkowite odłączenie serwera WWW od Internetu i rozpoczęcie stopniowego ustalania, co się stało. Ponadto nigdy nie dowiemy się dokładnie, czy serwer nie ukrywa czegoś innego. Atakujący zawsze ma znaczną przewagę.

Jeśli zdecydujemy się umieścić ostatnią działającą kopię zapasową z powrotem na serwerze, bardzo pomożemy atakującemu. Wie on już, jak włamać się na Twój serwer i nic nie stoi na przeszkodzie, aby za kilka godzin zrobił to ponownie. Ponadto kopia zapasowa prawdopodobnie zawiera oprogramowanie mallware lub inny rodzaj wirusa.

Chociaż jest to bardzo niepopularna opcja wśród moich klientów, osobiście zawsze zalecam **całkowite usunięcie najpierw witryn WordPress** lub wszelkich systemów, których klient nie aktualizuje, a które zawierają wiele zagrożeń bezpieczeństwa. Na przykład, jeśli na jednym serwerze znajduje się 30 witryn, które mają więcej niż 2 lata, istnieje praktycznie gwarancja, że przynajmniej jedna z nich zawiera jakąś lukę.

Jeśli nie rozumiesz problemu, lepiej od razu skontaktuj się z odpowiednim specjalistą, który ma głęboką wiedzę na ten temat i rozumie sprawy w szerokim kontekście.

**Uwaga etyczna:**

Jeśli zarządzasz witryną klienta, u którego wystąpił incydent bezpieczeństwa, Twoim obowiązkiem jest poinformowanie go o tym. W przypadku wycieku jakichkolwiek danych (takich jak hasła, ale często znacznie więcej) należy również poinformować użytkowników końcowych, że ich hasła są powszechnie znane i powinni je wszędzie zmieniać. Jeśli używasz powolnej funkcji haszującej (na przykład **Bcrypt + sól**), znacznie utrudnisz atakującemu złamanie haseł (niestety, [Bcrypt passwords can be cracked](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Jeśli chcesz regularnie otrzymywać informacje o tym, czy Twoje konto zostało wycieknięte, zalecam zarejestrowanie się w serwisie [Have i been pwned?](https://haveibeenpwned.com/). Więcej informacji na temat [naruszenia bezpieczeństwa](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni) można znaleźć na stronie internetowej OOO.

Nawyki atomowe
--------------

W ciągu ostatnich sześciu miesięcy skończyłem czytać książkę [Atomic Habits] (https://www.melvil.cz/kniha-atomove-navyky/), która otworzyła mi oczy. Opisuje proces codziennego, stopniowego ulepszania o 1%, który w dłuższej perspektywie przyniesie wiele korzyści.

Ponieważ zwracają się do mnie firmy i osoby na różnych etapach zadłużenia technologicznego, chciałbym podsumować najgorsze rzeczy:

- **FTP do komunikacji z serwerem nie ma sensu**. Jest to niezabezpieczony protokół, który jest kontrolowany za pomocą hasła. Jeśli chcesz dać komuś dostęp, musi on znać hasło i może robić to samo, co Ty. Jeśli chcesz odebrać mu dostęp, nie możesz tego zrobić, jedynym sposobem jest zmiana hasła. Lepiej całkowicie przestawić się na SSH i używać kluczy RSA. Często jestem zaskoczony, że nawet firmy rozwiązują ten problem w dzisiejszych czasach. Nigdy nie należy tworzyć jednej tabeli z wszystkimi hasłami. I z pewnością nigdy nie jest to wspólne dla całej drużyny!
- Kod źródłowy znajduje się w Git**, dane w bazie danych, a zawartość statyczna (obrazy, pliki PDF, ...) w plikach na dysku. Wybór niewłaściwego magazynu danych pogarsza projekt aplikacji i jej bezpieczeństwo. Adres URL do pliku PDF (np. z fakturą) powinien zawierać losowo wygenerowaną treść, którą zna tylko odbiorca. W przypadku wyjątkowo wrażliwych treści (takich jak wyciąg z konta bankowego) ważne jest zaszyfrowanie ich na dysku i udostępnienie dopiero po zalogowaniu.
- Niepubliczne części witryny muszą zawierać nagłówki META `noindex` i `nofollow`, aby nie zostały zaindeksowane przez Google. Wrażliwe treści, których nie może poznać konkurencja, muszą być chronione hasłem.
- Wyłącz opcję [directory content listing](https://www.simplified.guide/apache/disable-directory-listing) na swoim serwerze. W przypadku nieodpowiedniego ustawienia cała witryna może być przeszukiwana maszynowo w celu znalezienia poufnych plików.
- Projekt korzeń! W żadnym wypadku nie może być bezpośredniego adresu URL do plików konfiguracyjnych. Na przykład [Nette robi to dobrze](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) już w pierwszym samouczku. To samo dotyczy [publicznie dostępnych repozytoriów git](https://smitka.me/open-git/). Ten rodzaj podatności bardzo łatwo jest zbagatelizować, a konsekwencje bywają tragiczne.
- Błąd może również powstać w wyniku niezamierzonego przeoczenia programisty. Bardzo ważne jest, aby dla każdej zmiany przeprowadzać przegląd kodu przez żywego człowieka. Wiele błędów jest wykrywanych przez [PHPStan](https://github.com/phpstan/phpstan) lub podobne narzędzia, zanim kod zobaczy człowiek.
- Nigdy nie należy [wysyłać haseł w czytelnej formie pocztą elektroniczną](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), zawsze można to zrobić w inny sposób.
- Problemy z bezpieczeństwem w starych wersjach bibliotek i oprogramowania. W każdej sekundzie roboty przeszukują Internet i atakują znane luki w zabezpieczeniach (SQL injection, XSS, CSRF, ...). Wiele znanych dziur dotyczy starszych wersji WordPressa.

Podatności jest znacznie więcej, a problemy różnią się w zależności od projektu. Jeśli chcesz przeprowadzić szybki audyt serwera, zalecam użycie programu [Maldet](https://www.rfxn.com/projects/linux-malware-detect/), a następnie zatrudnienie odpowiedniego specjalisty, który pomoże Ci przeprowadzić [pełny audyt witryny](https://baraja.cz/audit-webu), i to nie tylko ze względów bezpieczeństwa.

Inżynierowie ds. bezpieczeństwa są jak plastik w oceanie - po prostu na zawsze. Przyzwyczajaj się do tego.

Zasada akcji i reakcji w przyrodzie
-------------------------------

Zapewne zauważyłeś też, że w przyrodzie zawsze obowiązuje zasada reakcji. Oznacza to, że w pewnym środowisku coś się dzieje, a otaczające nas organizmy reagują na to w różny sposób. Takie podejście ma wiele zalet, ale jedną ogromną wadę - zawsze pozostaniesz w tyle.

Jako osoba myśląca (projektant, programista, konsultant) masz ogromną przewagę, którą jest zdolność do działania - to znaczy do poznania z wyprzedzeniem pewnej części dużych zagrożeń i aktywnego zapobiegania im w pierwszej kolejności. Nie da się zapobiec wszystkim błędom, ale można przynajmniej złagodzić ich konsekwencje lub zbudować mechanizmy kontrolne, które wcześnie wykrywają problemy, dzięki czemu jest czas na reakcję.

Większość ataków jest przeprowadzana przez długi czas, a jeśli prowadzisz rejestr, masz zazwyczaj wystarczająco dużo czasu, aby wykryć problem.
