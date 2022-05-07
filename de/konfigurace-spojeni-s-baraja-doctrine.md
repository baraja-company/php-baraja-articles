Konfigurieren der Baraja-Doktrin-Verbindung
===========================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	de: konfigurieren-der-baraja-doktrin-verbindung
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

Um eine Verbindung zur Datenbank innerhalb von [Baraja Doctrine] (https://github.com/baraja-core/doctrine) herzustellen, müssen Sie die Neon-Konfigurationsdatei verwenden, die ein gemeinsamer Teil des Nette-Frameworks ist.

Die Konfiguration kann wie folgt aussehen:

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

Wenn der DI-Container kompiliert wird, wird die Konfiguration überprüft und eine Fehlermeldung ausgegeben, die den spezifischen Fehler beschreibt.

Die Anmeldedaten werden bei der Kompilierung des Containers sicher überprüft und dann physisch im Container gespeichert. Nur der Dienst, der die Verbindung zur Datenbank herstellt, hat dann Zugriff auf die Logins, und sie können nicht einfach von einem externen Dienst oder einem böswilligen Besucher aus der Tracy Bar abgerufen werden.

Abwärtskompatibilität
----------

In der Vergangenheit wurden z. B. Definitionen über Parameter verwendet:

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

Diese Einstellung ist jedoch als **veraltet** gekennzeichnet, um die Anwendungssicherheit zu erhöhen. Bei Verwendung der Parameter könnte jeder Dienst (oder sogar ein Teil der Anwendung) Anmeldedaten anfordern, oder die aktive Tracy-Leiste auf der Seite könnte sie verraten.
