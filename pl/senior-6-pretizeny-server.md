Pilna naprawa przeciążonego serwera
===================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	pl: pilna-naprawa-przeciazonego-serwera
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

Zewnętrzne narzędzie monitorujące zgłosi Ci, że średni czas odpowiedzi 5 monitorowanych adresów URL podwoił się w ciągu ostatnich 30 minut. Projekt działa na pojedynczym fizycznym serwerze, który nie jest pod twoim zarządem i działa gdzieś w centrum danych. Łączysz się przez SSH, uruchamiasz htop i widzisz, że obciążenie procesora wynosi 95%, a pamięć jest od dawna przepełniona.

Według git, wiadomo, że jakiś tydzień temu robili migrację bazy danych do nowej struktury tabel, a kolega pisze na czacie, że musiał uruchomić migrację w nocy, bo przeliczanie kolumn i indeksów trwało około 5 godzin, podczas których prawie cała baza była zablokowana i nie działał ani INSERT, ani SELECT.

Więc problemy z wydajnością są prawdopodobnie spowodowane niewłaściwymi indeksami, źle przeprojektowanymi zapytaniami SQL lub dużymi pulami połączeń. Nie ma czasu na revert, na stronie jest 7 tys. użytkowników wg Google Analytics, a przerwa w działaniu przez 5 godzin oznaczałaby dla klienta zagrożenie reputacji i stratę w tym czasie od kilkudziesięciu do kilkuset tysięcy koron (trudno oszacować, projekciki wymyślają wystarczająco dużo). Zdajesz sobie sprawę, że testowanie tylko funkcjonalności na środowisku testowym nie wystarczy i musisz wdrożyć również test obciążeniowy.

Ponieważ jest to ważny sklep e-commerce Twojego największego klienta i spodziewasz się, że sytuacja może się pogorszyć, masz 30 sekund na podjęcie decyzji.

Jak postępować?

1. nie robisz nic. Strona będzie wolniejsza, ale dopóki serwer to wytrzyma, to chyba jest ok...
2. Spróbujesz przygotować plan przywrócenia struktury DB i uruchomić go tak szybko, jak to możliwe.
3. Migrujesz stronę na wydajniejszy serwer (ale to oznacza przyspieszone podniesienie ceny dla klienta, a następnie czekanie może 6 godzin na zakończenie migracji, ponieważ masz setki GB tabel bazy danych).
4. Dowiadujesz się, które zapytania SQL i które strony zajmują najwięcej czasu i po prostu je wyłączasz.
5. Uruchamiasz Cloudflare i pozwalasz mu statycznie sprawdzić, co może.
6. Jakaś inna magia (chyba nie ma co wymyślać)...
