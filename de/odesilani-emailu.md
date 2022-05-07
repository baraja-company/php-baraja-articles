PHP-Funktion mail()
===================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	de: php-funktion-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

Die Funktion `mail()` sendet eine E-Mail-Nachricht über die Standard-Serverkonfiguration. Um korrekt zu funktionieren, müssen Sie die Funktion auf dem Server aktivieren und den Mailserver für den Versand einrichten.

**Die Funktion ist nur zum Senden gedacht. Sie müssen den Empfang von Nachrichten auf der Ebene des Mailservers regeln. Laden Sie zum Beispiel regelmäßig Nachrichten über das Protokoll "IMAP" oder "POP3" herunter.

> Von der Verwendung der Funktion rate ich derzeit dringend ab, da sich der Programmierer selbst um alles kümmern muss (z. B. die richtigen Header senden oder die Kodierung einstellen).
>
> Viel besser ist eine Verbindung über einen <a href="/send-email-mail-smtp">SMTP-Server</a>.

Verwendung von
-------

```php
mail ('jan@barasek.com', 'Thema', 'E-Mail-Text...');
```

Der erste Parameter ist die Adresse des Empfängers, der zweite der Betreff und der dritte der Text der Nachricht. Der vierte (optionale) Parameter gibt die zusätzliche Konfiguration der Nachricht an.
