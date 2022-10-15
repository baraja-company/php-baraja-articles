Asynchroniczny światopogląd
===========================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	pl: asynchroniczny-swiatopoglad
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

Od dłuższego czasu zauważyłem, że nasz świat ma charakter asynchroniczny i zdecentralizowany. Kiedy zdasz sobie z tego sprawę i zaczniesz myśleć o tym, jak wykorzystać to na swoją korzyść, łatwo może pojawić się cała wielka koncepcja tego, jak patrzeć na rozwiązywanie złożonych problemów. W tym poście chciałbym wyjaśnić kilka pomysłów, z których już korzystam. Nie czerpię ich z żadnego konkretnego źródła, to raczej połączenie wielu doświadczeń i własnych pomysłów. Zasady te nie sprawdzają się we wszystkich przypadkach.

Określenie środowiska i celów
-------------------------

Prawie wszystkie projekty, którymi się zajmowałem (czy to sam, czy w zespole) miały dość konkretnie określony cel. Takie podejście ma wiele sensu, bo ważne jest, by wiedzieć, czego chcemy. Z drugiej strony uważam, że zdefiniowanie konkretnego celu jest niemożliwe w złożonym środowisku i często podczas realizacji okazuje się, że tak naprawdę chcemy dotrzeć gdzie indziej.

Więc zamiast konkretnego celu buduję obszar różnych celów, gdzie są alternatywy, albo nawet zupełnie nowy wyizolowany niezależny cel może być właściwym celem, jeśli ma sens.

Przykłady z praktyki:

- Muszę stopniowo rozszerzać działalność na inne rynki. Jednym z podcelów, które odkryłem podczas wdrażania, może być maszynowe tłumaczenie sieci.
- Muszę odebrać 6 przesyłek w różnych miejscach w okolicach Pragi i nie znam najkrótszej trasy. Nie chcę tego odkrywać w skomplikowany sposób, więc po prostu kieruję się w stronę najdalszego punktu i po drodze zmieniam plan w kierunku innych tras. Na przykład, gdy przesiadam się na Andělu, decyduję się na tramwaj, który przyjeżdża jako pierwszy, co dynamicznie wpływa na kolejny plan trasy.
- Muszę napisać ten post, ale nie mam godziny czasu pod rząd. Dlatego mogę ją pisać w częściach podczas jazdy metrem jako poszczególne rozdziały. Następnie w przyszłości dokonam ostatecznego scalenia do tej formy.
- Nie wiem jak działa algorytm obliczania wykupu towaru dla mojego klienta, który muszę przeprogramować. Więc postawię zapytanie do wielu osób w tym samym czasie i rozwiążę coś innego, gdy to zrobię. Asynchronicznie w ciągu 2 dni pojawi się wiele różnych odpowiedzi, na podstawie których dopiero później podejmę decyzję.
- Od dłuższego czasu muszę pozbyć się PHP na serwerze, więc stopniowo przepisuję jedną stronę na raz do Reacta. Stopniowo przenoszę strony do chmury i buduję je na statycznie wygenerowanym Nektarze.

Żaden z celów podróży nie jest celem ostatecznym. Jeśli pracujesz z powierzchnią, a nie punktem, masz znacznie większe szanse na trafienie. Jednocześnie motywacja sprawdza się tutaj doskonale, bo najgorsze, co może się zdarzyć, to osiągnąć swój cel, a potem nie mieć na co liczyć w przyszłości.

Dużo też trzeba powiedzieć, że aby mieć podobne nastawienie trzeba mieć w miarę stabilne środowisko, w którym można popełniać błędy. Takie podejście nie może zagwarantować konkretnego terminu rozwiązania konkretnego pojedynczego zadania. Z drugiej strony, może optymalizować rozwiązywanie jak największej liczby równoległych zadań w jak najkrótszym czasie, choć niekoniecznie w kolejności, w jakiej przybyły do kolejki.

Zasadę tę można by porównać do tego, jak działają obliczenia np. w programowaniu równoległym. W skrócie, dekomponujesz zadania na poszczególne wątki, które rozwiązują różne podzadania jednocześnie, a gdy już uzyskasz wszystkie odpowiedzi, możesz skomponować rozwiązanie.

Wdrażanie nowych wersji oprogramowania
--------------------------------

Wszędzie bardzo się z tym zmagam.

Sposób, w jaki rozwój oprogramowania jest zwykle obsługiwany, polega na tym, że masz jakąś stabilną wersję, która działa produkcyjnie dla użytkowników, następnie masz jakieś środowisko testowe, w którym odbywa się nowy rozwój, a raz na jakiś czas przenosisz nowości ze środowiska testowego na produkcję, co nazywa się wydaniem. Proces ten jest stosunkowo bezpieczny i powolny.

Ponieważ stopniowo przeszedłem do architektury mikro-frontendowej, w której frontend aplikacji (to, co widzi użytkownik) jest całkowicie oddzielony od backendu (gdzie dzieje się obliczanie i przetwarzanie danych), proces wydania może działać inaczej.

Nadal mam jedno środowisko produkcyjne, które zawsze działa. Z niego dla każdego zadania tworzona jest nowa gałąź, która ma zająć się konkretną cechą. Ponieważ używam Vercel (bardzo dobrego dostawcy chmury) do hostowania frontendu, mogę mieć niestandardowy adres URL dla każdej gałęzi rozwoju.

Po przetestowaniu nowej funkcji, jest ona natychmiast scalana i wdrażana do produkcji w procesie asynchronicznym. Dlatego powszechnie zdarza się, że dziennie jest może 15 wdrożeń nowych wersji.

Wiele z tego odnosi się do idei, która została mi przekazana w LMC - "Funkcja, którą dostarczasz klientowi jutro, nawet jeśli w niedoskonałym stanie, ma dla niego znacznie większą wartość niż funkcja, która jest doskonale odfajkowana, ale będzie dostępna dopiero za rok". Ale to może działać tylko wtedy, gdy masz sposób na szybką dystrybucję wiadomości - typowo web development.

To co mi się bardzo podoba w frameworku Next.js (zbudowanym na Recto) i Vercel to zasada, że podłączasz swoje repozytorium GitHub bezpośrednio do Vercel, a z każdym commitem następuje automatyczny build, testy, a następnie deployment na produkcję, gdy wszystko jest ok. Więc deweloper nie musi się o nic martwić, a aplikacja wdraża się co godzinę przy zerowym wysiłku. Bardzo ważne jest sformalizowanie rutynowych rzeczy, a następnie ich automatyzacja.

Publikowanie treści
----------------

Rozbiłem wyżej dziesiątki tematów i postów u mnie w Apple Notes. Dla każdego tematu nieustannie przychodzą mi do głowy nowe pomysły, które mogę zanotować i posortować według tagów. Gdy dla danego tematu wygeneruje się wiele notatek, przekształcam je w rozdziały, a następnie grupę rozdziałów przekształcam w artykuły i posty na FB.

Cechy tej zasady:

- Nie wiem z góry ile artykułów kiedykolwiek opublikuję,
- Ale potem znowu wiem, że może być ich dużo,
- Jestem w stanie zapewnić, że każdy post jest pełen informacji, spostrzeżeń i pomysłów, które dojrzewały w czasie (dojrzewanie tego postu trwało około 3 miesięcy),
- Jest zrównoważony i sprawia mi przyjemność, bo nie muszę pisać tony tekstu w jednym momencie i ryzykować, że o czymś zapomnę.

A potem proces wydawniczy.

Wiele treści publikuję najpierw po cichu w sieci, aby Google zauważył je jako pierwsze i strona została zaindeksowana. Gdy ma dobre pokrycie słowami kluczowymi i jestem z niego ogólnie zadowolony, temat trafia np. do mediów społecznościowych.

W przypadku wielu tematów wiem, że nie będą one interesujące, a bardziej prawdopodobne, że będą Cię denerwować. Ale jednocześnie chcę je mieć w sieci, bo ktoś może w przyszłości ich szukać. W takim przypadku artykuł zostaje tylko w sieci. Przykładem takich artykułów są różne artykuły nakładkowe, które łączą cały obszar tematyczny, tak abym miał jak najwięcej poruszonych tematów na stronie. Artykuły okładkowe są często bardziej techniczne i nudne. Albo są to kategorie generowane maszynowo, gdzie po prostu organizuję wiele artykułów w tematy, obejmując w ten sposób jak najwięcej słów kluczowych, które ktoś może potem chcieć wyszukać.

Wiedza, edukacja, testy
------------------------------

Lubię bawić się technologiami, w których od początku nie wiadomo, do czego się kiedyś przyda.

Jak tłumaczenie maszynowe. Na pierwszy rzut oka nie ma sensu tłumaczyć dziesiątek artykułów na języki obce dla czytelników, z którymi nie mam kontaktu. Z drugiej strony może to być jeden z kroków do podjęcia w przyszłości decyzji, do której konieczne jest spełnienie przesłanek.

W zasadzie nikt nie wie, jak będzie wyglądała przyszłość. Więc ma to dla mnie ogromny sens, aby objąć jak najwięcej możliwości, które chcesz zrozumieć, przynajmniej powierzchownie, i być może zająć się nimi w przyszłości. Dobrze jest myśleć o krokach nie tylko jako o rozwiązanym zadaniu, ale jako o niekończącym się procesie ewolucyjnym, który nie ma ostatecznego celu.

Czy takie podejście sprawdza się przy dostarczaniu projektów na zamówienie?
--------------------------------------------------------

W większości przypadków nie.

Musisz mieć klienta, który jest twoim partnerem i oboje chcecie dostarczyć najlepsze możliwe rozwiązanie, nawet jeśli z góry wiesz, że do końca nie dowiesz się, co to jest.

W naszej branży nazywa się to zwinnym rozwojem, a te zasady bazują na tym. Z drugiej strony zaobserwowałem, że agile development w znanej mi formie nie pasuje mi bezpośrednio i lubię wymyślać koślawe alternatywy.

Przy agility widzę dużo zasady zaangażowania w kontekście tego, kto co rozwiązuje w ramach danego sprintu. Dla mnie bardziej sensowne jest zbudowanie środowiska, abyś miał ogromny backlog zadań, które są rozwiązywane w oparciu o to, co jest najbardziej potrzebne w tej chwili, lub mam zdolność umysłową do zrobienia. Kiedy jestem w drodze, na przykład, mam tendencję do zajmowania się bardziej rozwojowymi zadaniami związanymi z myśleniem, które mogę wykonać na zewnątrz bez komputera. Kiedy znów jestem w biurze, zajmuję się bardziej algorytmicznymi i matematycznymi zadaniami, ponieważ mogę się w pełni skupić i zainwestować dużo energii umysłowej.

Nie polecam tej zasady, jeśli zakładasz zupełnie nowy sklep internetowy lub Twój biznes plajtuje. Potrzebujesz stabilnego środowiska, które samo się uruchomi, a ty będziesz tylko jubilerem. Ale jednocześnie wiesz, że jeśli nic nie zrobisz, to twoje stabilne środowisko kiedyś się skończy.
