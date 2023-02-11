Atak hakerów na agencję
=======================

> id: '7d5edd94-cce4-4a32-9ec5-51260dcd1653'
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
> 	pl: atak-hakerow-na-agencje
> 
> publicationDate: '2023-02-11 14:20:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '18511f7b3ff80124720244a93e140932'

Historia z 2017 roku: pracujesz jako lead developer w agencji i zarządzasz około 300 projektami różnej wielkości, które firma rozwinęła w tym czasie. Większość z nich to proste aplikacje Nette z maksymalnie 10 szablonami, kilkoma formularzami i tabelami bazy danych. Nic wymyślnego. Nie wiesz tak wiele o projektach, bo każdy z nich był tworzony przez nieco innego dostawcę, ludzie rotują w firmie i z niej wychodzą, a ty masz nadzieję, że jakoś to załatwisz. Ponieważ firma robi dużo optymalizacji kosztów, na jednym serwerze hostują może 50 projektów jednocześnie.

Nagle przybiega do Ciebie kierownik projektu mówiąc, że jedna z ważnych stron klienta przestała działać. Zamiast prawdziwej strony, dostajesz czarny ekran, a tam wszelkiego rodzaju tekst mówiący, że strona została zhakowana i ma dostęp do całego serwera.

Po raz kolejny nie wiesz (i nie możesz ważnie) tak dużo o architekturze i zawartości projektów, a wiele projektów działa tylko na serwerze. Projektor wciska Ci, że strona musi działać, a przecież nie znasz zakresu ataku i tego, gdzie napastnik się udał, czy są backdoory z przeszłości.

Jak się zdecydować?

1. nie robisz nic i starasz się najpierw przeanalizować zdarzenie.
2) Zdejmujesz serwer w trybie offline. Nie znasz rozmiaru szkód, a atakujący może dalej penetrować serwer i wykradać dane. Jednocześnie oznacza to, że wiele stron jest niedostępnych.
3. Wykonujesz przywracanie określonej witryny z kopii zapasowej sprzed dnia, a w międzyczasie zajmujesz się tym, co się stało. Atakujący może jednak odzyskać dostęp i dalej wykradać dane.
4. Inne rozwiązanie...
