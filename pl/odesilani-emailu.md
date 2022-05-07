Funkcja PHP mail()
==================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	pl: funkcja-php-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

Funkcja `mail()` wysyła wiadomość e-mail za pomocą domyślnej konfiguracji serwera. Aby funkcja działała poprawnie, należy ją włączyć na serwerze i skonfigurować serwer pocztowy do wysyłania wiadomości.

**Funkcja ta służy wyłącznie do wysyłania. Odbieranie wiadomości należy zorganizować na poziomie serwera pocztowego. Na przykład, regularne pobieranie wiadomości przy użyciu protokołu `IMAP` lub `POP3`.**.

> Obecnie zdecydowanie odradzam korzystanie z tej funkcji, ponieważ programista musi sam o wszystko zadbać (na przykład o wysłanie poprawnych nagłówków lub ustawienie kodowania).
>
> Znacznie lepszym rozwiązaniem jest połączenie za pomocą <a href="/send-email-mail-smtp">serwera SMTP</a>.

Korzystanie z witryny
-------

```php
mail ('jan@barasek.com', 'Temat:', 'Tekst e-mail...');
```

Pierwszy parametr to adres odbiorcy, drugi to temat, a trzeci to tekst wiadomości. Czwarty (opcjonalny) parametr określa dodatkową konfigurację komunikatu.
