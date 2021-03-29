Jak nastavit HTTPS / SSL certifikát - kompletní příručka
========================================================

> id: "0346a091-2b2c-494f-b2fb-573796d4fb46"
> slugCS: jak-nastavit-https-ssl-certifikat
> publicationDate: "2019-09-16 09:01:35"
> mainCategoryId: "02d93aa8-ed83-460a-ab97-4de21118f019"

Při nasazování `https` protokolu na weby klientů jsem často narážel na různé potíže, které pramenily z nepochopení problematiky a přílišné složitosti pojmů.

V tomto návodu podrobně popisuji jednotlivé kroky, jak validní certifikát získat a nasadit na webový server.

**V každém podnadpisu vždy stručně shrnu krok pro pokročilé a v spodní části rozeberu podrobně detaily pro začátečníky.**

> **Pozor:** Celý proces nasazení certifikátu může zabrat i více než hodinu času a je často výpadkový (web může být nedostupný).

Vstupní požadavky
-----------------

Návod předpokládá, že máme přístup k Terminálu webového serveru běžícího na Linuxu, který používá Apache.

Pro Nginx je platná celá teorie stejně, jenom se liší napojení souborů s certifikáty.

## Připojení k webovému serveru

Připojíme se k serveru prostřednictvím SSH.

- Na Windows doporučuji program **Putty**,
- na Macu nebo Linuxu stačí použít vestavěný Terminál.

Na Macu nebo Linuxu zavolejte příkaz:

```htaccess
ssh uživatel@server
```

Například se chci připojit na uživatele `root` na webu `baraja.cz`:

```htaccess
ssh root@baraja.cz
```

Nebo na uživatele `jan` na konkrétní IP adresu:

```htaccess
ssh jan@127.0.0.1
```

Po odeslání dotazu se buď spojení provede přímo, nebo budete požádáni o heslo. Při psaní hesla se nic nezobrazuje, proto heslo potvrdťe enterem a vyčkejte na autorizaci připojení.

> **Pozor:** Pokud nebudeme mít na nějakou akci práva, je potřeba si je přidělit. Buď se přepneme rovnou na uživatele `root` příkazem `sudo su`, nebo před příkazem, který chceme provést pod rootem uvedeme na začátku slovo `root`, například `root rm <název>` pod rootem smaže soubor `<název>`. Při použití příkazu `sudo` můžeme být pravidelně vyzváni zadat heslo.

**Podrobnosti:**

Přístup přes SSH vám zřídí konkrétní hosting, kde máte pronajatý server.

- V případě VPS dostanete SSH přístup vždy.
- V případě hostingu nemusíte SSH dostat vůbec a konfigurace se provádí jinak (obvykle přes webové rozhraní, nebo kontaktujte podporu).

## Přechod z HTTP na HTTPS

Pokud převádíme již existující web z `http` na `https`, je potřeba zaručit přesměrování veškerého provozu na nový `https` protokol.

V případě Apache to lze jednoduše docílit pomocí přesměrování v `.htaccess` souboru:

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

Tato konfigurace zařídí, že všechny requesty na `http` budou přesměrovány s `HTTP kódem 301` na `https`. Tato konfigurace je výchozí pro Nette framework, ale platí i pro všechny ostatní případy.

**Podrobnosti:**

Soubor `.htaccess` obsahuje konkrétní konfiguraci webového serveru, která ovlivňuje jednotlivé requesty. Zpravidla se umisťuje do stejného adresáře, kde je umístěn i `index.php`, nebo ostatní soubory, které jsou dostupné z internetu.

Jeho nastavení je platné jen pro server Apache a může být u některých hostingů zakázáno nebo omezeno. Pro podrobnější informace kontaktujte vždy hosting, kde web provozujete.

-----

Někdy se stává, že `.htaccess` nechce dobře spolupracovat a konfigurace je extrémně náročná (například pro mnoho domén, přičemž se každá chová jinak). V takovém případně lze v nouzi přesměrování na HTTPS vyřešit i přímo v PHP:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'off') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Moved Permanently');
	header('Location: ' . preg_replace('/^(https:\/\/)(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

Script vložte do `index.php`. Toto řešení však nedoporučuji.

## Najdeme soubory s virtual hosty

V případě Apache potřebujeme najít soubor s Virtual hosty.

Obvykle jsou umístěny na cestě `/etc/apache2/sites-available`.

Obsah adresáře vypíšeme příkazem `ls -al` a najdeme soubor, kde je nastaven virtuál pro náš web.

VirtualHosty jsou obvykle umístěny v souborech s příponou `.conf`. Výchozí je často v `000-default.conf`.

**Podrobnosti:**

- Adresář se otevírá příkazem `cd`, například `cd /etc/apache2/sites-available`.
- Obsah souboru vypíšeme do Terminálu příkazem `cat <název>`, případně editujeme příkazem `nano <název>` nebo `vim <název>`
- Z editoru se obvykle dostaneme klávesovou zkratkou `CTRL + X` nebo dvojím stisknutím `ESC`
- Soubory lze částečně procházet v grafickém rozhraní programem Midnight commander (příkaz `mc`), který v případě Ubuntu nainstalujeme jednoduše příkazem `apt-get install mc`, případně `sudo apt-get install mc`.

## Nastavíme virtual host pro port 443

V rámci Virtual hostu je potřeba připravit nový, pro port `443` (častá potíž může být v blokaci portu 443 na firewallu).

Obsah souboru může vypadat například takto:

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

Na souboru je nejdůležitější oblast:

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

Pochopení této konfigurace ja naprosto kritické. Pokud potřebuje googlit podrobnější informace, použijte slova `SSLCertificateFile`, `SSLCertificateKeyFile` a `SSLCertificateChainFile`, která jsou společná pro všechny servery Apache.

> **Pozor:** Problematika SSL je relativně stará a z důvodu zachování zpětné kompatibility se pro stejnou věc používají často různé názvy! Důležité je proto pochopení principu.

**Podrobnosti:**

Důležité je, aby VirualHost obsahoval:

- `<IfModule mod_ssl.c>` říká, že chceme použít SSL
- `<VirtualHost *:443>`, že celá komunikace poběží na portu 443
- `SSLEngine on` že je SSL povoleno pro tento VirtualHost
- `SSLCertificateFile`, `SSLCertificateKeyFile` a `SSLCertificateChainFile` jsou soubory s konkrétními klíčy.

Nic dalšího už není potřeba.

## Pochopení principu toho, co budeme dělat a jak certifikát funguje

Ve finální konfiguraci VirtualHostu budeme potřebovat skutečně jen 3 soubory `SSLCertificateFile`, `SSLCertificateKeyFile` a `SSLCertificateChainFile`, které můžeme umístit kamkoli na serveru (na názvu a umístění nezáleží). Důležité je uvést funkční cestu a aby byl obsah souborů validní.

Konkrétní způsob získání certifikátů se může lišit u jednotlivých certifikačních autorit. Důležité je pochopit princip a ten poté použít pro váš případ.

| Soubor                    | Význam                              |
|---------------------------|-------------------------------------|
| `SSLCertificateFile`      | Tento certifikát **pošle autorita** |
| `SSLCertificateKeyFile`   | **Můj vygenerovaný** privátní klíč  |
| `SSLCertificateChainFile` | Stáhl jsem si na webu typu **intermediate + root** |

## Získání privátního klíče a žádosti o certifikát

Na serveru nejprve vygenerujeme privátní klíč. Důležité je to slovo **privátní**, to znamená, že ho nezná nikdo jiný, kromě webového serveru. V ideálním případě by neměl nikdy opustit server a měl by být umístěn na bezpečném místě. Ztráta tohoto klíče znamená ztrátu bezpečnosti, protože se útočník bude moci vydávat za konkrétní server.

Klíč vygenerujeme příkazem:

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

Pro generování musíme mít na serveru nainstalovaný program `openssl`, který získáme například příkazem `sudo apt install openssl`.

Klíčů může být více druhů, v tomto případě generujeme RSA klíč dlouhý 2048 bajtů (`rsa:2048`).

Výstupem příkazu jsou 2 soubory (které si pojmenujte podle domény):

- `yourdomain.key` - jde o privátní klíč. Cestu k tomuto klíči uložte do Apache VirtualHostu k položce `SSLCertificateKeyFile`
- `yourdomain.csr` - jde o tzv. `certificate request`, nebo-li o žádost o vystavení certifikátu.

Obsah requestu je potřeba vždy předat certifikační autoritě na schválení. Obvykle se to dělá přes webové rozhraní v administraci na webu, kde se certifikáty prodávají. Schválení requestu (žádosti) liší podle typu certifikátu. Nejčastěji se provádí automaticky robotem a trvá od 5 minut do 8 hodin. V případě drahých certifikátů, kde se ověřuje i fyzické vlastnictví webu a provozující firmy se ověření provádí ručně a může trvat i několik dnů.

Pokud spěcháte, existuje certifikační agentura `Let's encrypt`, která requesty autorizuje automaticky s platností 3 měsíce.

> **Pozor:** V některých případech nabízí certifikační autorita automatické generování requestu (žádosti). Toto není doporučovaný postup, protože zná privátní klíč. Pokud na tento způsob přistoupíte, vždy musíte od agentury získat i privátní klíč a ten umístit do souboru na serveru stejným způsobem, jako kdybychom ho generovali.

## Získání klíče pro konkrétní certifikační/bezpečnostní autoritu

V mezičase při čekání na schválení requestu a vystavení certifikátu zajistíme **veřejný klíč** certifikační autority. V Apache VirtualHostu je reprezentován souborem `SSLCertificateChainFile` (anglicky se to jmenuje `Intermediate and Root CA`). Tento klíč je principiálně veřejný a říká webovému prohlížeči, kdo je certifikační autorita. Tento soubor si obvykle stáhneme na stránkách autority nebo nám ho doručí do e-mailu.

Je potřeba vždy stáhnout správný klíč podle zvoleného algoritmu. V případě RapidSSL lze stáhnout například zde: https://knowledge.digicert.com/generalinformation/INFO1548.html

## Získání certifikátu

Na základě requestu nám certifikační autorita vystavila certifikát. V případě Apache VirtualHostu je reprezentován souborem `SSLCertificateFile`.

Důležité je, že certifikát má omezenou platnost a je potřeba tento proces (ideálně před expirací) opakovat. Datum skončení platnosti zjistíte snadno v Chromu po kliknutí na zelený zámeček v prohlížeči a po zobrazení podrobností o certifikátu, případě ho sdělí certifikační autorita.

Po skončení platnosti je certifikát neplatný a web bude odmítat spojení a uživatelům bude zobrazena chybová hláška o porušení zabezpečení.

## Kontrola a provedení změn

Správnost nastavení Apache lze částečně ověřit příkazem `apache2ctl -S`.

Pro projevení změn je potřeba Apache restartovat, například příkazem:

```shell
sudo service apache2 restart
```

Pokud nebude vyhozena žádná chybová hláška, okamžitě jdeme ověřit funkčnost přes webový prohlížeč (doporučuji Google Chrome, který ukazuje nejvíce podrobností).

V případě chyby zavoláme příkaz `apache2ctl -S`, který umí ukázat cestu k logům, kde lze přibližně vyčíst, co je špatně. Důležité je všechny kroky v tomto manuálu provést ve správném pořadí a neprohodit některý z klíčů.

## Support do budoucna

Nezapomeňte si do kalendáře poznamenat datum konce certifikátu, abyste ho stihli včas obnovit. Některé certifikační autority posílají notifikační e-mail, ale ne vždy je na to spoleh.

Proces obnovy a získání nového certifikátu můžete provádět paralelně při běhu toho současného a pak ho jen v jeden okamžik vyměnit.

Pro automatizovanou obnovu doporučuji nástroj [Certbot](https://certbot.eff.org), který umí automaticky hlídat platnosti a certifikáty obnovovat. Obnovení se provádí například cronem, který jednou za měsíc v noci zavolá příkaz pro vystavení nového certifikátu a hned ho nasadí.

Pokud správě certifikátů nerozumíte, je dobrý nápad hostovat u zavedené společnosti, která vám dodá funkční hosting včetně certifikátu. Ušetříte si s tím spoustu starostí.
