Konfigurowanie połączenia Doktryna Baraja
=========================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	pl: konfigurowanie-polaczenia-doktryna-baraja
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

Aby nawiązać połączenie z bazą danych w ramach [Doktryny Baraja](https://github.com/baraja-core/doctrine), należy użyć pliku konfiguracyjnego Neon, który jest wspólną częścią frameworka Nette.

Konfiguracja może wyglądać następująco:

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

Po skompilowaniu kontenera DI następuje weryfikacja konfiguracji i wyświetlany jest komunikat o błędzie opisujący konkretny błąd.

Dane uwierzytelniające do logowania są bezpiecznie weryfikowane podczas kompilacji kontenera, a następnie fizycznie przechowywane w kontenerze. Dostęp do loginów ma tylko usługa zapewniająca połączenie z bazą danych, a loginy nie mogą zostać pozyskane przez zewnętrzną usługę lub nieuczciwego gościa z paska Tracy.

Zgodność wsteczna
----------

W przeszłości stosowano definicje wykorzystujące parametry, np:

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

Jednak w celu zwiększenia bezpieczeństwa aplikacji ustawienie to zostało oznaczone jako **depregnowane**. Podczas korzystania z parametrów dowolna usługa (lub nawet część aplikacji) mogłaby zażądać podania danych uwierzytelniających do logowania lub aktywny pasek Tracy na stronie mógłby je zdradzić.
