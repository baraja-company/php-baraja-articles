Komunikace přes SSH a RSA2 klíč
===============================

> id: "09ec87dd-810d-4618-b76b-fd30f63be30a"
> slug:
> 	cs: ssh-klic
> 
> publicationDate: "2022-11-12 15:00:00"
> mainCategoryId: "02d93aa8-ed83-460a-ab97-4de21118f019"

SSH je síťový protokol pro šifrovaný přenos souborů a Terminálu. SSH se nejčastěji používá pro vzdálené ovládání webového serveru a bezpečný přenos souborů. Na rozdíl od FTP nabízí nativní šifrované spojení. SSH komunikuje přes výchozí port `22`. Spojení se inicializuje uživatelským jménem a adresou cílového serveru. Pro ověření je možno použít heslo (nedoporučuji) nebo SSH RSA2 klíč (doporučuji).

Získání (generování) klíče
--------------------------

Ještě než se k serveru připojíme, je potřeba získat (resp. vygenerovat) náš první SSH RSA2 klíč. Důležité je, aby šlo o algoritmus `RSA2`. Klíčů totiž existuje celá řada, a ne všechny lze použít.

V Linuxu se pro generování používá nástroj `ssh-keygen`, kterému zadáme složitost klíče (v tomto případě 4096) a e-mail autorizovaného uživatele:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Po spuštění příkazu budeme dotázáni na cestu pro uložení klíče a případný `passphrase` (autorizační heslo). Jako cestu nezadávejte nic (automaticky se zvolí výchozí umístění) a passphrase zadejte volitelně (pokud ho zadáte, bude potřeba toto stejné heslo zadávat vždy před použitím klíče).

Vygenerovaný klíč se automaticky uloží na výchozí umístění `~/.ssh`, tedy do adresáře `.ssh` v domovském adresáři aktuálního uživatele.

Generování SSH klíče ve Windows
-------------------------------

Windows bohužel nedisponuje výchozí cestou pro SSH klíč. Ideální je proto nainstalovat například nástroj `Putty` a k tomu `PuttyGen`, přes který se klíč vygeneruje. Vždy zvolte algoritmus `RSA2`. Opět se vygeneruje dvojice klíčů, tak je kamkoli bezpečně uložte. Před použitím SSH klíče v nástroji `Putty` je nutné zvolit diskovou cestu, odkud se má klíč načítat. V Linuxu toto potřeba není, tam existuje výchozí disková cesta.

Bezpečení klíče
---------------

Při generování se klíče generují ve skutečnosti dva. Jeden veřejný klíč (ten předáte druhé straně pro možnost komunikace) a privátní klíč (ten je jen váš, nikomu ho nikdy neříkejte, a slouží pro rozšifrování komunikace).

Zásadní je, abyste nikdy nepřišli o privátní klíč. Jeho ztráta znamená prolomení celé komunikace!

Obecně se doporučuje pro každé zařízení a uživatele generovat unikátní dvojici veřejného a privátního klíče, aby se snižovala pravděpodobnost úniku. Pokud si však chcete klíč mezi zařízeními přenášet, můžete. SSH klíč je na úrovni hesla, takže po přesunutí do jiného počítače rovnou spojení funguje.

Některé servery si pamatují, s kterým zařízením přes SSH komunikovaly naposledy. Je tedy možné, že po přesunutí klíče do nového počítače nebude spojení fungovat. V takovém případě je potřeba vymazat cache klíčů na serveru.

Autorizace klíče pro spojení k serveru
--------------------------------------

Pro spojení k serveru slouží příkaz `ssh`. Stačí jednoduše zadat uživatele a doménu:

```shell
ssh root@baraja.cz
```

Poté se zkusí navázat SSH spojení. Pokud máte funkční a správně nakonfigurovaný SSH klíč, dojde k automatickému spojení. Pokud ne, musíte zadat heslo.

Pokud se chcete vůči vašemu serveru autorizovat přes SSH klíč namísto hesla, je potřeba přenést váš **veřejný klíč** na server.

Jednoduše ho zobrazte příkazem:

```shell
cat ~/.ssh/id_rsa.pub 
```

A zkopírujte celý jeho obsah na cílový server do umístění `~/.ssh/authorized_keys`. Pokud máte více klíčů, tak každý na samostatný řádek.
