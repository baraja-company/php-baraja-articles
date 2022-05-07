Jak skonfigurować certyfikat HTTPS / SSL - kompletny przewodnik
===============================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	pl: jak-skonfigurowac-certyfikat-https-ssl---kompletny-przewodnik
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

Wdrażając protokół `https` na stronach klientów, często napotykałem na różne trudności, które wynikały z braku zrozumienia zagadnień i zbytniego skomplikowania pojęć.

W tym poradniku szczegółowo opisuję kroki, które należy wykonać, aby uzyskać i wdrożyć ważny certyfikat na serwerze WWW.

**W każdym podtytule zawsze krótko podsumowuję krok dla zaawansowanych użytkowników, a na dole omawiam szczegóły dla początkujących.

> **Ostrzeżenie:** Cały proces wdrażania certyfikatu może trwać ponad godzinę i często jest przerywany (witryna może być niedostępna).

Wymagania wejściowe
-----------------

W instrukcjach założono, że mamy dostęp do serwera WWW Terminal działającego pod kontrolą systemu Linux i wykorzystującego Apache.

W przypadku Nginx cała teoria ma takie samo zastosowanie, tylko łączenie plików z certyfikatami jest inne.

## Łączenie się z serwerem WWW

Łączymy się z serwerem za pomocą protokołu SSH.

- W systemie Windows polecam program **Putty**,
- Na komputerach Mac lub Linux wystarczy użyć wbudowanego Terminala.

W systemie Mac lub Linux należy wywołać polecenie:

```htaccess
ssh uživatel@server
```

Na przykład, chcę się połączyć z użytkownikiem `root` na stronie `baraja.cz`:

```htaccess
ssh root@baraja.cz
```

Lub do użytkownika `jan` na określony adres IP:

```htaccess
ssh jan@127.0.0.1
```

Po wysłaniu zapytania połączenie zostanie nawiązane bezpośrednio lub zostaniesz poproszony o podanie hasła. Podczas wpisywania hasła nic się nie wyświetla, dlatego należy potwierdzić hasło klawiszem Enter i poczekać na autoryzację połączenia.

> **Ostrzeżenie:** Jeśli nie mamy praw do działania, musimy je przypisać. Albo przełączamy się bezpośrednio na użytkownika `root` komendą `sudo su`, albo poprzedzamy komendę, którą chcemy wykonać pod rootem, słowem `root` na początku, na przykład `root rm <name>` pod rootem usunie plik `<name>`. Podczas korzystania z polecenia `sudo` może się zdarzyć, że zostaniemy poproszeni o podanie hasła.

**Szczegóły:**

Dostęp SSH jest konfigurowany przez hosting, w którym dzierżawisz serwer.

- W przypadku VPS zawsze będziesz mieć dostęp do SSH.
- W przypadku hostingu możesz w ogóle nie mieć dostępu do SSH, a konfiguracja odbywa się w inny sposób (zwykle za pomocą interfejsu WWW lub poprzez kontakt z działem pomocy technicznej).

## Przejście z protokołu HTTP na HTTPS

Jeśli konwertujesz istniejącą stronę z `http` na `https`, musisz zagwarantować, że cały ruch zostanie przekierowany do nowego protokołu `https`.

W przypadku Apache'a można to łatwo osiągnąć, stosując przekierowanie w pliku `.htaccess`:

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

Ta konfiguracja zapewni, że wszystkie żądania do `http` będą przekierowywane z kodem `HTTP 301` do `https`. Ta konfiguracja jest domyślna dla struktury Nette, ale ma zastosowanie także we wszystkich innych przypadkach.

**Szczegóły:**

Plik `.htaccess` zawiera specyficzną konfigurację serwera WWW, która ma wpływ na każde żądanie. Zwykle umieszcza się go w tym samym katalogu co `index.php` lub inne pliki dostępne z Internetu.

Jego ustawienie jest ważne tylko dla serwera Apache i może być wyłączone lub ograniczone dla niektórych hostów. Aby uzyskać bardziej szczegółowe informacje, zawsze należy skontaktować się z firmą hostingową, w której znajduje się witryna.

-----

Czasami zdarza się, że `.htaccess` nie chce dobrze współpracować i konfiguracja jest bardzo trudna (np. dla wielu domen, z których każda zachowuje się inaczej). W takim przypadku przekierowanie na HTTPS może być w razie potrzeby obsługiwane bezpośrednio w PHP:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'poza') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Przeniesiony na stałe');
	header('Lokalizacja:' . preg_replace('/^(https:/(?:www.)(.*)$/.', '$1$2', $location));
	die;
}
```

Umieść skrypt w pliku `index.php`. Nie polecam jednak tego rozwiązania.

## Znajdź pliki z wirtualnymi hostami

W przypadku Apache'a musimy znaleźć plik Virtual hosts.

Zazwyczaj znajdują się one w ścieżce `/etc/apache2/sites-available`.

Wylistuj zawartość katalogu za pomocą polecenia `ls -al` i znajdź plik, w którym znajduje się wirtualna konfiguracja dla naszej strony.

VirtualHosts są zwykle umieszczane w plikach z rozszerzeniem `.conf`. Wartość domyślna jest często podana w pliku `000-default.conf`.

**Szczegóły:**

- Katalog otwierany jest poleceniem `cd`, na przykład `cd /etc/apache2/sites-available`.
- Zawartość pliku jest zapisywana do Terminala za pomocą polecenia `cat <nazwa>` lub edytowana za pomocą poleceń `nano <nazwa>` lub `vim <nazwa>`.
- Edytor jest zwykle dostępny za pomocą skrótu klawiaturowego `CTRL + X` lub przez dwukrotne naciśnięcie klawisza `ESC`.
- Pliki można częściowo przeglądać w GUI za pomocą Midnight commander (komenda `mc`), który w przypadku Ubuntu instaluje się po prostu za pomocą `apt-get install mc` lub `sudo apt-get install mc`.

## Skonfiguruj wirtualnego hosta dla portu 443

W ramach hosta wirtualnego musimy przygotować nowy port dla portu `443` (częstym problemem może być blokowanie portu 443 przez zaporę sieciową).

Zawartość pliku może wyglądać na przykład tak:

```htaccess
<IfModule mod_ssl.c>
     <VirtualHost *:443>
         ServerName baraja.cz

         ServerAdmin jan@barasek.com
         DocumentRoot /var/www/baraja.cz/www
         Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
         ErrorLog ${APACHE_LOG_DIR}/baraja.cz.ssl.error.log
         CustomLog ${APACHE_LOG_DIR}/baraja.cz.ssl.access.log combined
         SSLEngine on
         SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
         SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
         SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
         SSLProtocol             all -SSLv3
         SSLCipherSuite          ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS
         SSLHonorCipherOrder     on
         SSLCompression          off
         <FilesMatch "\.(cgi|shtml|phtml|php)$">
                 SSLOptions +StdEnvVars
         </FilesMatch>
         <Directory /usr/lib/cgi-bin>
                 SSLOptions +StdEnvVars
         </Directory>
		 BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
         # MSIE 7 and newer should be able to use keepalive
         BrowserMatch "MSIE [17-9]" \
         ssl-unclean-shutdown
     </VirtualHost>
</IfModule>
```

Najważniejszym obszarem jest teczka:

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

Zrozumienie tej konfiguracji jest absolutnie niezbędne. Jeśli trzeba wyszukać bardziej szczegółowe informacje, należy użyć słów `SSLCertificateFile`, `SSLCertificateKeyFile` i `SSLCertificateChainFile`, które są wspólne dla wszystkich serwerów Apache.

> **Ostrzeżenie:** Problem SSL jest stosunkowo stary i często używa się różnych nazw dla tej samej rzeczy, aby zachować kompatybilność wsteczną! Dlatego ważne jest, aby zrozumieć tę zasadę.

**Szczegóły:**

Ważne jest, aby VirualHost zawierał:

- `<IfModule mod_ssl.c>` mówi, że chcemy używać SSL
- `<VirtualHost *:443>`, że cała komunikacja będzie odbywać się na porcie 443
- `SSLEngine on`, że SSL jest włączony dla tego VirtualHost
- Pliki `SSLCertificateFile`, `SSLCertificateKeyFile` i `SSLCertificateChainFile` są plikami z określonymi kluczami.

Nic więcej nie jest potrzebne.

## Zrozumienie zasady tego, co będziemy robić i jak działa certyfikat

W ostatecznej konfiguracji VirtualHost tak naprawdę będziemy potrzebować tylko 3 plików `SSLCertificateFile`, `SSLCertificateKeyFile` i `SSLCertificateChainFile`, które możemy umieścić w dowolnym miejscu na serwerze (nazwa i lokalizacja nie mają znaczenia). Ważne jest, aby podać ścieżkę roboczą i upewnić się, że zawartość plików jest prawidłowa.

Konkretna metoda uzyskiwania certyfikatów może być różna dla poszczególnych urzędów certyfikacji. Ważne jest, aby zrozumieć tę zasadę, a następnie zastosować ją w swoim przypadku.

| Plik | Znaczenie |
|---------------------------|-------------------------------------|
| Ten certyfikat **jest wysyłany przez organ** |.
| `SSLCertificateKeyFile` | **Mój wygenerowany** klucz prywatny |
| Pobrałem z sieci typ **intermediate + root** |.

## Uzyskanie klucza prywatnego i żądanie certyfikatu

Na serwerze najpierw generujemy klucz prywatny. Ważne jest słowo **prywatny**, które oznacza, że nie zna go nikt poza serwerem WWW. Najlepiej, aby nigdy nie opuszczał serwera i był umieszczony w bezpiecznym miejscu. Utrata tego klucza oznacza utratę bezpieczeństwa, ponieważ osoba atakująca będzie mogła podszyć się pod konkretny serwer.

Aby wygenerować klucz, użyj polecenia:

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

Aby go wygenerować, musimy mieć zainstalowany na serwerze program `openssl`, który możemy uzyskać, uruchamiając na przykład `sudo apt install openssl`.

Może istnieć kilka rodzajów kluczy, w tym przypadku generujemy klucz RSA o długości 2048 bajtów (`rsa:2048`).

Wynikiem działania tego polecenia są 2 pliki (które można nazwać zgodnie z domeną):

- `yourdomain.key` - jest to klucz prywatny. Zapisz ścieżkę do tego klucza w Apache VirtualHost do `SSLCertificateKeyFile`.
- `yourdomain.csr` - jest to `zapytanie o certyfikat`, czyli żądanie wydania certyfikatu.

Treść wniosku musi być zawsze przedstawiona CA do zatwierdzenia. Zazwyczaj odbywa się to za pomocą interfejsu internetowego w administracji na stronie, na której sprzedawane są certyfikaty. Zatwierdzenie wniosku różni się w zależności od rodzaju świadectwa. Najczęściej jest ona wykonywana automatycznie przez robota i trwa od 5 minut do 8 godzin. W przypadku drogich certyfikatów, gdzie weryfikowana jest także fizyczna własność obiektu i firmy obsługującej, weryfikacja odbywa się ręcznie i może trwać kilka dni.

Jeśli się spieszysz, istnieje agencja certyfikacyjna `Let's encrypt`, która automatycznie autoryzuje wnioski z 3-miesięcznym okresem ważności.

> **Ostrzeżenie:** W niektórych przypadkach urząd certyfikacji oferuje automatyczne generowanie wniosków. Nie jest to zalecana praktyka, ponieważ zna ona klucz prywatny. W takim przypadku należy zawsze uzyskać klucz prywatny od agencji i umieścić go w pliku na serwerze w taki sam sposób, jak w przypadku generowania żądania.

## Uzyskiwanie klucza dla określonego urzędu certyfikacji/bezpieczeństwa

W międzyczasie, w oczekiwaniu na zatwierdzenie wniosku i wydanie certyfikatu, zabezpieczamy **klucz publiczny** urzędu certyfikacji. W Apache VirtualHost jest to reprezentowane przez plik `SSLCertificateChainFile` (w języku angielskim nazywa się to `Intermediate and Root CA`). Klucz ten jest z zasady publiczny i informuje przeglądarkę internetową, kto jest urzędem certyfikacji. Plik ten jest zwykle pobierany ze strony internetowej urzędu certyfikacji lub dostarczany pocztą elektroniczną.

Należy zawsze pobierać prawidłowy klucz zgodny z wybranym algorytmem. Na przykład RapidSSL można pobrać tutaj: https://knowledge.digicert.com/generalinformation/INFO1548.html.

## Uzyskanie certyfikatu

Na podstawie tego żądania urząd certyfikacji wystawił nam certyfikat. W przypadku Apache VirtualHost jest on reprezentowany przez plik `SSLCertificateFile`.

Co ważne, certyfikat ma ograniczoną ważność i należy powtórzyć ten proces (najlepiej przed wygaśnięciem ważności). Datę wygaśnięcia można łatwo znaleźć w przeglądarce Chrome, klikając zieloną kłódkę w przeglądarce i przeglądając szczegóły certyfikatu, jeśli jest on podany przez urząd certyfikacji.

Po upływie daty ważności certyfikat jest nieważny i witryna odmawia połączenia, a użytkownicy otrzymują komunikat o błędzie o naruszeniu bezpieczeństwa.

## Sprawdź i wprowadź zmiany

Poprawność ustawień Apache'a można częściowo zweryfikować za pomocą polecenia `apache2ctl -S`.

Aby zmiany zaczęły obowiązywać, Apache musi zostać ponownie uruchomiony, na przykład za pomocą polecenia:

```shell
sudo service apache2 restart
```

Jeśli nie zostanie wyświetlony żaden komunikat o błędzie, od razu przechodzimy do sprawdzenia funkcjonalności za pomocą przeglądarki internetowej (polecam Google Chrome, który pokazuje najwięcej szczegółów).

W przypadku wystąpienia błędu, wywołujemy polecenie `apache2ctl -S`, które może pokazać ścieżkę do logów, gdzie możemy zobaczyć, co w przybliżeniu jest nie tak. Ważne jest, aby wszystkie czynności opisane w niniejszej instrukcji wykonać w prawidłowej kolejności i nie zamieniać miejscami żadnego z kluczy.

## Wsparcie w przyszłości

Nie zapomnij zaznaczyć w kalendarzu daty wygaśnięcia certyfikatu, aby móc na czas odnowić jego ważność. Niektóre urzędy certyfikacji wysyłają wiadomość e-mail z powiadomieniem, ale nie zawsze jest to niezawodne.

Proces odnawiania i uzyskiwania nowego certyfikatu można przeprowadzić równolegle, gdy działa już obecny, a następnie po prostu go wymienić.

Do automatycznego odnawiania polecam program [Certbot](https://certbot.eff.org), który może automatycznie monitorować ważność i odnawiać certyfikaty. Odnawianie odbywa się na przykład za pomocą crona, który raz w miesiącu w nocy wywołuje polecenie wydania nowego certyfikatu i natychmiast go wdraża.

Jeśli nie rozumiesz zasad zarządzania certyfikatami, warto korzystać z usług sprawdzonej firmy, która dostarczy Ci funkcjonalny hosting wraz z certyfikatem. Dzięki temu zaoszczędzisz sobie wielu kłopotów.
