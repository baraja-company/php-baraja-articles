Komunikacja poprzez SSH i klucz RSA2
====================================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	pl: komunikacja-poprzez-ssh-i-klucz-rsa2
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH to protokół sieciowy służący do szyfrowanego transferu plików i terminala. SSH jest najczęściej używany do zdalnego sterowania serwerem internetowym i bezpiecznego przesyłania plików. W przeciwieństwie do FTP, oferuje natywne połączenie szyfrowane. SSH komunikuje się na domyślnym porcie `22`. Połączenie jest inicjowane z nazwą użytkownika i adresem serwera docelowego. Do uwierzytelnienia można użyć hasła (niezalecane) lub klucza SSH RSA2 (zalecane).

Uzyskanie (wygenerowanie) klucza
--------------------------

Zanim połączymy się z serwerem, musimy uzyskać (lub wygenerować) nasz pierwszy klucz SSH RSA2. Ważne jest, aby był to algorytm `RSA2`. Dzieje się tak dlatego, że istnieje wiele kluczy i nie wszystkie można wykorzystać.

W systemie Linux do jego wygenerowania służy narzędzie `ssh-keygen`, do którego podajemy złożoność klucza (w tym przypadku 4096) oraz e-mail autoryzowanego użytkownika:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Po uruchomieniu polecenia zostaniemy zapytani o ścieżkę do przechowywania klucza oraz ewentualne `hasło` (hasło autoryzacyjne). Nie wpisuj nic jako ścieżkę (domyślna lokalizacja jest wybierana automatycznie) i opcjonalnie wprowadź hasło (jeśli to zrobisz, będziesz musiał wprowadzić to samo hasło za każdym razem przed użyciem klucza).

Wygenerowany klucz jest automatycznie zapisywany w domyślnej lokalizacji `~/.ssh`, czyli w katalogu `.ssh` w katalogu domowym bieżącego użytkownika.

Generowanie klucza SSH w systemie Windows
-------------------------------

Niestety, system Windows nie ma domyślnej ścieżki dla klucza SSH. Dlatego idealnym rozwiązaniem jest zainstalowanie np. narzędzia `Putty` i `PuttyGen` do generowania klucza. Zawsze wybieraj algorytm `RSA2`. Ponownie zostanie wygenerowana para kluczy, więc przechowuj je bezpiecznie gdzieś. Przed użyciem klucza SSH w `Putty`, należy wybrać ścieżkę dysku, z którego ma być pobrany klucz. W Linuksie nie jest to potrzebne, istnieje domyślna ścieżka do dysku.

Bezpieczeństwo kluczy
---------------

Gdy generowane są klucze, w rzeczywistości generowane są dwa. Jeden klucz publiczny (ten, który dajesz drugiej stronie, aby umożliwić komunikację) i klucz prywatny (ten jest tylko twój, nigdy nikomu nie mów, i służy do odszyfrowania komunikacji).

Istotne jest, aby nigdy nie zgubić klucza prywatnego. Jego utrata oznacza zerwanie całej komunikacji!

Ogólnie zaleca się generowanie unikalnej pary kluczy publicznych/prywatnych dla każdego urządzenia i użytkownika, aby zmniejszyć prawdopodobieństwo wycieku. Jeśli jednak chcesz przenieść klucz między urządzeniami, możesz. Klucz SSH jest na poziomie hasła, więc po przeniesieniu go na inną maszynę od razu połączenie działa.

Niektóre serwery pamiętają, z którym urządzeniem ostatnio komunikowały się przez SSH. Dlatego możliwe jest, że po przeniesieniu klucza na nowy komputer połączenie nie będzie działać. W takim przypadku należy wyczyścić pamięć podręczną kluczy na serwerze.

Upoważnienie klucza do połączenia z serwerem
--------------------------------------

Do połączenia z serwerem służy komenda `ssh`. Wystarczy podać użytkownika i domenę:

```shell
ssh root@baraja.cz
```

Następnie spróbuje nawiązać połączenie SSH. Jeśli masz działający i poprawnie skonfigurowany klucz SSH, połączenie zostanie nawiązane automatycznie. Jeśli nie, musisz wprowadzić hasło.

Jeśli chcesz uwierzytelniać się na serwerze za pomocą klucza SSH zamiast hasła, musisz przekazać swój **klucz publiczny** na serwer.

Wystarczy wyświetlić go za pomocą polecenia:

```shell
cat ~/.ssh/id_rsa.pub
```

I skopiuj całą jego zawartość na serwer docelowy w lokalizacji `~/.ssh/authorized_keys`. Jeśli masz więcej niż jeden klucz, każdy na osobnej linii.
