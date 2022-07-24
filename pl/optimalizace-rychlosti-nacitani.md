Jak skutecznie optymalizować szybkość ładowania strony
======================================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	pl: jak-skutecznie-optymalizowac-szybkosc-ladowania-strony
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

Kiedy podróżuję, często spotykam się ze słabymi połączeniami internetowymi, co jako projektant stron internetowych zmusza mnie do zastanowienia się nad zasadami projektowania, aby rozwiązać prędkość strony internetowej nawet na słabym połączeniu.

Z praktyki wyniosłem kilka przydatnych trików:

Ważne strony docelowe są często proste i opłaca się używać niestandardowych stylów CSS
-----------------------------------------------------------------------------------

Na przykład strona logowania do systemu CMS jest często bardzo prosta. Czy prosty formularz naprawdę musi pobierać cały Bootstrap i inne frameworki CSS/JS? Ważne strony warto optymalizować i pisać natywny kod.

Po pełnym wyrenderowaniu strony dostajemy kilka sekund, w których użytkownik wypełnia swoje dane, a my możemy w tle pobrać i zbuforować pozostałe style w przeglądarce. Zanim użytkownik się zaloguje, prawdopodobnie będzie miał już pobrany Bootstrap (a jeśli jest na Edge, to nie...).

Zamiast ikonek często możemy używać emoji
-----------------------------------

Naprawdę! Emoji ma wiele praktycznych zalet:

- Zajmuje tylko 4 bajty, więc jest ważniejszy niż jakikolwiek obrazek.
- Nigdy nie udaje się go pobrać, więc użytkownik zawsze widzi przynajmniej ikonę, a nie puste miejsce
- Wyświetla się w stylu wizualnym użytkownika
- Obecnie posiada już szerokie wsparcie dla urządzeń
- Zapisuje żądanie serwera i nie musimy się zajmować tym, skąd wziąć obraz (można to zoptymalizować poprzez CDN, ale po co, prawda)
- Wiele ikon, z którymi prawdopodobnie nie chcesz mieć do czynienia. Na przykład, skąd wziąć ikony flag wszystkich państw, które chcesz wyświetlić w administracji? Użyj emoji: https://github.com/bara.../country/blob/main/flag-emoji.json

Użyj krytycznych stylów bezpośrednio w HTML podczas ładowania strony
------------------------------------------------------

Zdarza mi się umieszczać ważne style CSS definiujące układ strony i podstawowy layout bezpośrednio w HTML-u. Wprowadzam limit 1 KB, do którego staram się wrzucić jak najwięcej. Minusem tego podejścia jest to, że style muszą być przesyłane w każdym żądaniu i nie mogą być buforowane, z drugiej strony pobierają się nieporównywalnie szybciej niż obraz.

Nie jestem takim ekspertem od szybkości ładowania, [Martin Michalek](https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) powiedziałby Ci lepiej.

Użyj ajaxa!
--------------

Zawsze używam ajaxa do pobierania nieistotnych lub powolnych treści. Dodaje to trochę do wymagań technologicznych aplikacji, ale mogę sobie na to pozwolić.

Przykładem dobrego miejsca do wykorzystania ajaxa jest prawie wszystko w administracji. Bardzo często jest wiele danych do pobrania, ale użytkownik nie jest zainteresowany wszystkim. Kiedy użytkownik ma tylko pobrany cienki klient javascript, a wszystkie dane są pobierane za pośrednictwem Vue i json, tylko minimalna ilość danych jest kiedykolwiek pobierana, a odpowiedzi są natychmiastowe.

Jak to zrobić w Vue: https://vue.baraja.cz/api-a-ajax-ve-vue-js

Na backendzie używam biblioteki dla Nette: https://github.com/baraja-core/structured-api

Użyj odpowiedniej sieci CDN
---------------------

Do dystrybucji treści statycznych zalecam używanie CDN dla wszystkich typów stron. Jeśli nie wiesz, jak skonfigurować CDN, przynajmniej użyj Cloudflare w trybie proxy. Będzie buforować statyczne treści automatycznie do siebie na podstawie nagłówków HTTP. Nie każdy dostawca hostingu ma dobrze skonfigurowany Pops, a w przypadku długich tras pozwoli to zaoszczędzić setki milisekund w każdym żądaniu.

Odpowiedni format obrazu
---------------------

Jeden z moich juniorów umieścił ostatnio na stronie głównej serwisu obrazek PNG, który zawierał zdjęcie i zajmował 3 MB. Powiedział, że to jest OK, ponieważ strona ładuje się szybko na jego łączu (tak jest na jego optyce w domu, prawda...), plus powiedział, że przesyłamy dużo danych w tych dniach, jak wideo.

Jestem staromodny w tej kwestii i optymalizuję to, co mogę.

Zdjęcia do JPG, lub lepiej WEBP. Ale zapisanie zdjęcia do JPG nic nie znaczy, trzeba je przepuścić przez serwis optymalizacyjny: https://tinyjpg.com

Jeśli masz obrazy w Git, to narzędzie może automatycznie skompresować je i wysłać pull request: https://imgbot.net. Po dodaniu nowego obrazu, ponownie wysyła PR.

Jeśli trzeba skompresować tysiące obrazów (np. całą galerię produktów na stronie), używam do tego celu aplikacji desktopowej dla Maca: https://imageoptim.com/mac

Odpowiednie skompresowanie obrazów pozwoli zazwyczaj zaoszczędzić najwięcej danych. Czasami nawet 50%.
