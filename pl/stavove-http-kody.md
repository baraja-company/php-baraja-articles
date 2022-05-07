Kody stanu HTTP
===============

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	pl: kody-stanu-http
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

W komunikacji HTTP przesyłane są tzw. kody stanu, czyli informacje o tym, w jaki sposób został wykonany transfer. Jestem pewien, że wiesz, iż kod `200` oznacza sukces, a kod `404` oznacza nieistniejącą stronę.

Kody stanu są podzielone na kilka grup według ich prefiksu.

1xx Informacyjny
--------------

| Kod | Znaczenie |
|-------|--------|
| `100` | Kontynuuj |
| 101` | Protokół przełączania |

2xx Sukces
----------

| Kod | Znaczenie |
|-------|--------|
| `200` | OK (wszystko w porządku) |
|201` | Utworzono |
|202` | Przyjęte |
| `203` | Nieautoryzowane informacje |
|204` | Bez zawartości |
| `205` | Przywróć zawartość |
| `206` | Zawartość częściowa |

Przekierowanie 3xx
----------------

| Kod | Znaczenie |
|-------|--------|
| 300` | Wybór wielokrotny |
| `301` | Stałe przekierowanie |
| 302` | Znaleziono |
| 303` | Zobacz więcej |
| `304` | Bez zmian |
| `305` | Użyj proxy |
| 306` | **Stary, ale zarezerwowany do przyszłego użytku** |
| 307` | Tymczasowe przekierowanie |

4xx Błąd klienta (użytkownika)
-----------------------------

| Kod | Znaczenie |
|-------|--------|
| 400` | Złe żądanie |
| `401` | Nieautoryzowane połączenie |
| 402` | Payment Requested |
| 403` | Wyłączone |
| 404` | Not Found |
| `405` | Metoda niedozwolona |
| 406` | Niedopuszczalne |
| 407` | Wymagane uwierzytelnienie proxy |
| 408` | Request timed out |
| `409` | Konflikt sieciowy |
| 410` | Dane przepadły |
| `411` | Żądana długość nie pasuje |
| 412` | Założenie nie powiodło się |
| `413` | Żądanie podmiotu jest zbyt duże |
| `414` | Request-URI jest zbyt długi |
| `415` | Niewłaściwy typ nośnika |
| `416` | Żądany zakres nie jest zadowalający |
| `417` | Oczekiwanie nie powiodło się |

5xx Błąd serwera
--------------

| Kod | Znaczenie |
|-------|--------|
| `500` | Błąd wewnętrzny serwera |
| `501` | Nie wdrożony |
| 502` | Bad Gateway |
| 503` | Usługa niedostępna |
| 504` | upłynął limit czasu bramy |
| `505` | Wersja HTTP nie jest obsługiwana |
| 509` | Przekroczono limit przepustowości |
