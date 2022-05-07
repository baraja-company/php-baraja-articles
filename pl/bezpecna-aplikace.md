Bezpieczne stosowanie
=====================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 	pl: bezpieczne-stosowanie
> 
> publicationDate: '2019-10-01 14:19:04'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '74e52382b995b303550d8e4b783bbb84'

Jeśli poważnie myślisz o tworzeniu aplikacji internetowych, a witryna będzie później dostępna w Internecie, bardzo ważne jest, aby zająć się kwestią bezpieczeństwa.

Realistycznie rzecz biorąc, na deweloperów czekają następujące zagrożenia:

- **Aplikacja ma wewnętrzny błąd**, na przykład dlatego, że programista popełnił błąd, którego nie zauważył podczas pisania kodu, lub błąd objawia się tylko "czasami"
- Serwer jest źle skonfigurowany** lub środowisko **zmieniło się**, ponieważ administrator serwera zmienił jego zachowanie, a strony nie zostały do tego przystosowane. Ewentualnie wdrażamy na nowym komputerze i nie znamy jego dokładnej konfiguracji.
- Ktoś próbuje zaatakować** - albo atakujący z zewnątrz, albo były pracownik z wewnątrz systemu.

Wszystkie sytuacje są bardzo nieprzyjemne i wymagają natychmiastowego działania. Zaprojektowanie aplikacji w taki sposób, aby nigdy nie uległa awarii, nie jest technicznie możliwe.

Podczas opracowywania kodu bardzo ważne jest jego testowanie po napisaniu, a jeśli na serwerze pojawi się błąd, należy natychmiast poinformować o tym programistę, podając dokładny opis problemu.

Do wykrywania błędów i monitorowania stanu serwera polecam narzędzie <a href="https://tracy.nette.org/">Tracy</a>, które zapisuje wszystkie błędy śmiertelne, nieobsługiwane wyjątki i inne do plików bezpośrednio na dysku serwera. Dodatkowo można skonfigurować adres e-mail, na który będą wysyłane powiadomienia.

Chcesz skorzystać z konsultacji w zakresie bezpieczeństwa w swojej aplikacji?

Skontaktuj się ze mną.
