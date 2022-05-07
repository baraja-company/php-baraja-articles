Wysyłanie e-maili (funkcje mail() i SMTP) w PHP
===============================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	pl: wysylanie-e-maili-funkcje-mail-i-smtp-w-php
> 
> perex:
> 	- 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> 	- 'Opcje wysyłania poczty elektronicznej w PHP, funkcja mail(), SMTP, nagłówki, konfiguracja i Nette Mailer.'
> 
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

W PHP mamy zasadniczo dwa sposoby wysyłania wiadomości e-mail:

- Rodzima funkcja `mail()`, która ma sporo ograniczeń,
- lub za pośrednictwem serwera SMTP.

Funkcja `mail()` musi korzystać z serwera SMTP, co jest jedynym sposobem wysyłania poczty przez serwer SMTP.
---------------

Idea tego rozwiązania jest prosta: użytkownik wywołuje funkcję:

```php
mail('jan@barasek.com', 'Temat:', 'Tekst wiadomości...');
```

A PHP samo zajmie się wysyłaniem.

Wewnętrznie, wysyłanie działa poprzez odczytanie konfiguracji z pliku `php.ini` i wyszukanie domyślnego serwera SMTP, przez który ma być dostarczona poczta. Wymaga to więc wcześniejszego skonfigurowania serwera WWW.

Główną wadą funkcji `mail()` jest to, że programista musi sam wymyślić całą logikę. Obejmuje to na przykład usuwanie nagłówków dotyczących szyfrowania, łączenie certyfikatów z szyfrowaniem wiadomości itd.

W przypadku niepowodzenia wysyłania zwracana jest wartość `false`, którą musimy przechwycić i przetworzyć samodzielnie. Możemy dowiedzieć się o konkretnym błędzie w ograniczony sposób, wywołując `error_get_last()`, na przykład:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Nie można wysłać poczty:'
		. (@error_get_last()['wiadomość'] ?? '')
	);
}
```

> **TIP:** Zwróć uwagę, że nie podaliśmy adresu, z którego chcemy wysłać pocztę, ani kodowania, które ma zostać użyte.
>
> Wszystkie te ustawienia muszą być przekazywane za pomocą nagłówków.

Jeśli nadal musisz używać funkcji `mail()` (na przykład z powodu hostingu), polecam użycie pakietu `nette/mail` i usługi `SendmailMailer`, która dobrze radzi sobie z wysyłaniem poczty.

Serwer SMTP
-----------

Skrót SMTP oznacza `Simple Mail Transfer Protocol`, co (jak się wkrótce przekonasz) jest bardzo prawdziwe.

SMTP, w przeciwieństwie do `mail()`, jest bardziej zaawansowanym protokołem, posiadającym zaawansowane opcje konfiguracyjne nie tylko po stronie PHP, ale także bezpośrednio na serwerze pocztowym.

Obsługa protokołu SMTP na hostach jest w 2018 roku doskonała.

SMTP zasadniczo działa w ten sposób, że PHP najpierw nawiązuje połączenie z serwerem SMTP (wymaga to rozszerzenia `php_openssl.dll` w PHP, które prawdopodobnie masz już aktywne), uwierzytelnia się (weryfikuje poprawność danych logowania) podczas połączenia, a następnie możemy komunikować się z serwerem w podobny sposób jak z bazą danych - tzn. wysyłać pojedyncze żądania, ale cały czas utrzymywać jedno połączenie. Dużą zaletą protokołu SMTP jest bezpośrednia obsługa szyfrowania (znanego jako `TLS`).

Wysyłanie wiadomości e-mail z localhost - proste rozwiązanie
--------------------------------------------------

Często muszę wysyłać wiadomości e-mail z localhost, gdy testuję nowo napisaną aplikację.

> **Dla napiwku:**
>
> Na komputerach Mac sytuacja jest prosta, ponieważ serwer MAMP w jakiś "magiczny" sposób znajduje aktualnie zalogowane konto Apple Mail i wiadomości są zawsze wysyłane z tego konta.

Nie zawsze jednak można polegać na takim zachowaniu i warto opracować własne rozwiązanie. Jeśli masz połączenie z Internetem i konto Google, możesz w bardzo prosty sposób korzystać z konta Gmail, z którym możesz łączyć się bezpośrednio z PHP i wysyłać za jego pośrednictwem pocztę.

Jeśli używasz pakietu `nette/mail`, konfiguracja jest prosta:

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> Hasło nie jest hasłem logowania do konta (byłoby to niezabezpieczone i nie można by na przykład używać **dwuskładnikowego uwierzytelniania**).
>
> Musisz użyć tak zwanego "hasła aplikacji", co w praktyce oznacza, że <a href="https://myaccount.google.com/apppasswords">rejestrujesz swoją aplikację</a> bezpośrednio na swoim koncie Google, do którego przypisane jest jakieś losowo wygenerowane hasło, które wprowadzasz do PHP i przez które można ją wysłać.
>
> Szczegółowe instrukcje znajdują się <a href="https://support.google.com/accounts/answer/185833?hl=cs">w witrynie Google</a>.

Konfiguracja poczty na Wedosie
---------------------------

Za pośrednictwem hostingu Wedos można wysłać tylko 500 e-maili dziennie, a ja przez pewien czas zmagałem się z połączeniem SMTP.

Za pośrednictwem pakietu `nette/mail` można to zrobić w następujący sposób (rozwiązanie możliwe do zastosowania):

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

Parametr `host` jest inny dla każdego hostingu i można go znaleźć w e-mailu, który Wedos wysyła przy rejestracji hostingu.

Nazwa `username` reprezentuje skrzynkę pocztową, z której będą wysyłane maile. Skrzynka pocztowa musi istnieć. Wysyłając pocztę w PHP, musimy również ustawić wysyłkę na ten sam adres (w Nette za pomocą metody `->setFrom()`).

Jeśli nie wypełnimy konfiguracji dokładnie i poprawnie, zostaną wyświetlone różne komunikaty o błędach i nie będzie można wysyłać wiadomości pocztowych.

Po przekroczeniu liczby wysłanych wiadomości zostanie rzucony wyjątek informujący o przekroczeniu limitu.
