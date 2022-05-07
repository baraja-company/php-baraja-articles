Ako nastaviť certifikát HTTPS / SSL - kompletný sprievodca
==========================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	sk: ako-nastavit-certifikat-https-ssl---kompletny-sprievodca
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

Pri nasadzovaní protokolu `https` na klientských stránkach som sa často stretával s rôznymi ťažkosťami, ktoré pramenili z nepochopenia problematiky a prílišnej zložitosti konceptov.

V tomto návode podrobne opíšem kroky na získanie a nasadenie platného certifikátu na webový server.

**V každej podkapitole vždy stručne zhrniem krok pre pokročilých používateľov a v spodnej časti rozoberiem podrobnosti pre začiatočníkov.**

> **Upozornenie:** Celý proces nasadenia certifikátu môže trvať viac ako hodinu a často je prerušovaný (stránka môže byť nedostupná).

Vstupné požiadavky
-----------------

Pokyny predpokladajú, že máme prístup k webovému serveru Terminal, ktorý beží v systéme Linux a používa Apache.

Pre Nginx platí celá teória rovnako, len prepojenie súborov s certifikátmi je iné.

## Pripojenie k webovému serveru

K serveru sa pripájame prostredníctvom SSH.

- V systéme Windows odporúčam program **Putty**,
- V systéme Mac alebo Linux stačí použiť zabudovaný terminál.

V systéme Mac alebo Linux zavolajte príkaz:

ssh uživatel@server

Napríklad sa chcem pripojiť k používateľovi `root` na webovej stránke `baraja.cz`:

ssh root@baraja.cz

Alebo používateľovi `jan` na konkrétnu IP adresu:

ssh jan@127.0.0.1

Po odoslaní dotazu sa spojenie vytvorí priamo alebo budete požiadaní o zadanie hesla. Pri zadávaní hesla sa nič nezobrazí, preto potvrďte heslo klávesom Enter a počkajte na autorizáciu pripojenia.

> **Upozornenie:** Ak nemáme práva na akciu, musíme ich priradiť. Buď sa prepnite priamo na používateľa `root` príkazom `sudo su`, alebo pred príkaz, ktorý chceme vykonať pod rootom, dajte na začiatok slovo `root`, napríklad `root rm <názov>` pod rootom vymaže súbor `<názov>`. Pri používaní príkazu `sudo` môžeme byť pravidelne vyzvaní na zadanie hesla.

**Detaily:**

Prístup SSH nastavuje konkrétny hosting, na ktorom ste si prenajali server.

- V prípade VPS budete mať vždy prístup SSH.
- V prípade hostingu nemusíte mať SSH vôbec k dispozícii a konfigurácia sa vykonáva inak (zvyčajne cez webové rozhranie alebo kontaktujte podporu).

## Prechod z HTTP na HTTPS

Ak konvertujete existujúcu stránku z `http` na `https`, musíte zaručiť, že všetka prevádzka bude presmerovaná na nový protokol `https`.

V prípade Apache to možno ľahko dosiahnuť pomocou presmerovania v súbore `.htaccess`:

<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>

Táto konfigurácia zabezpečí, že všetky požiadavky na `http` budú presmerované s kódom `HTTP 301` na `https`. Táto konfigurácia je predvolená pre rámec Nette, ale platí aj pre všetky ostatné prípady.

**Detaily:**

Súbor `.htaccess` obsahuje špecifickú konfiguráciu webového servera, ktorá ovplyvňuje každú požiadavku. Zvyčajne je umiestnený v rovnakom adresári ako `index.php` alebo iné súbory, ktoré sú prístupné z internetu.

Jeho nastavenie je platné len pre server Apache a môže byť pre niektorých hostiteľov vypnuté alebo obmedzené. Podrobnejšie informácie vždy získate od hostingovej spoločnosti, kde je vaša stránka umiestnená.

-----

Niekedy sa stáva, že súbor `.htaccess` nechce dobre spolupracovať a konfigurácia je veľmi náročná (napríklad pre mnoho domén, z ktorých každá sa správa inak). V takomto prípade je možné presmerovanie na HTTPS zvládnuť priamo v PHP:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'mimo') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Presunuté natrvalo');
	header('Umiestnenie:' . preg_replace('/^(https:\/\/(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

Skript umiestnite do súboru `index.php`. Toto riešenie však neodporúčam.

## Nájsť súbory s virtuálnymi hostiteľmi

V prípade Apache musíme nájsť súbor Virtual hosts.

Zvyčajne sa nachádzajú na ceste `/etc/apache2/sites-available`.

Pomocou príkazu `ls -al` vypíšte obsah adresára a nájdite súbor, v ktorom je nastavený virtuál pre našu stránku.

VirtualHosts sa zvyčajne nachádzajú v súboroch s príponou `.conf`. Predvolené nastavenie je často v súbore `000-default.conf`.

**Detaily:**

- Adresár sa otvorí príkazom `cd`, napríklad `cd /etc/apache2/sites-available`.
- Obsah súboru sa zapíše do terminálu pomocou príkazu `cat <názov>` alebo sa upraví pomocou príkazov `nano <názov>` alebo `vim <názov>`.
- Do editora sa zvyčajne dostanete pomocou klávesovej skratky `CTRL + X` alebo dvojitým stlačením `ESC`.
- Súbory možno čiastočne prechádzať v grafickom rozhraní pomocou príkazu Midnight commander (`mc`), ktorý sa v prípade Ubuntu inštaluje jednoducho pomocou príkazu `apt-get install mc` alebo `sudo apt-get install mc`.

## Nastavenie virtuálneho hostiteľa pre port 443

V rámci virtuálneho hostiteľa musíme pripraviť nový port `443` (častým problémom môže byť blokovanie portu 443 na firewalle).

Obsah súboru môže vyzerať napríklad takto:

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

Na súbore je najdôležitejšia oblasť:

SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt

Pochopenie tejto konfigurácie je absolútne nevyhnutné. Ak potrebujete vyhľadať podrobnejšie informácie, použite slová `SSLCertificateFile`, `SSLCertificateKeyFile` a `SSLCertificateChainFile`, ktoré sú spoločné pre všetky servery Apache.

> **Upozornenie:** Problematika SSL je relatívne stará a pre tú istú vec sa často používajú rôzne názvy, aby sa zachovala spätná kompatibilita! Preto je dôležité pochopiť tento princíp.

**Detaily:**

Je dôležité, aby VirualHost obsahoval:

- `<IfModule mod_ssl.c>` hovorí, že chceme používať SSL
- `<VirtualHost *:443>`, že všetka komunikácia bude prebiehať na porte 443
- `SSLEngine on`, že pre tento VirtualHost je povolené SSL
- Súbory `SSLCertificateFile`, `SSLCertificateKeyFile` a `SSLCertificateChainFile` sú súbory so špecifickými kľúčmi.

Nič iné nie je potrebné.

## Pochopenie princípu toho, čo budeme robiť, a fungovania certifikátu

V konečnej konfigurácii VirtualHostu budeme skutočne potrebovať len 3 súbory `SSLCertificateFile`, `SSLCertificateKeyFile` a `SSLCertificateChainFile`, ktoré môžeme umiestniť kdekoľvek na serveri (na názve a umiestnení nezáleží). Je dôležité, aby ste uviedli pracovnú cestu a aby bol obsah súborov platný.

Konkrétny spôsob získavania certifikátov sa môže u jednotlivých CA líšiť. Dôležité je pochopiť princíp a potom ho uplatniť na svoj prípad.

| Súbor | Význam |
|---------------------------|-------------------------------------|
| `SSLCertificateFile` | Tento certifikát **posiela autorita** |
| `SSLCertificateKeyFile` | **Môj vygenerovaný** súkromný kľúč |
| `SSLCertificateChainFile` | Stiahol som z webu typu **intermediate + root** |

## Získanie súkromného kľúča a vyžiadanie certifikátu

Na serveri najprv vygenerujeme súkromný kľúč. Dôležité je slovo **súkromný**, ktoré znamená, že ho okrem webového servera nikto iný nepozná. V ideálnom prípade by nikdy nemal opustiť server a mal by byť umiestnený na bezpečnom mieste. Strata tohto kľúča znamená stratu bezpečnosti, pretože útočník sa bude môcť vydávať za konkrétny server.

Ak chcete vygenerovať kľúč, použite príkaz:

openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr

Na jeho vygenerovanie potrebujeme mať na serveri nainštalovaný program `openssl`, ktorý získame napríklad spustením príkazu `sudo apt install openssl`.

Môže existovať niekoľko druhov kľúčov, v tomto prípade generujeme kľúč RSA dlhý 2048 bajtov (`rsa:2048`).

Výstupom príkazu sú 2 súbory (ktoré pomenujete podľa svojej domény):

- `yourdomain.key` - toto je súkromný kľúč. Uložte cestu k tomuto kľúču v Apache VirtualHost do súboru `SSLCertificateKeyFile`.
- `yourdomain.csr` - toto je `žiadosť o certifikát` alebo žiadosť o vydanie certifikátu.

Obsah žiadosti sa musí vždy predložiť na schválenie príslušnému orgánu. Zvyčajne sa to vykonáva prostredníctvom webového rozhrania v administrácii na stránke, kde sa certifikáty predávajú. Schválenie žiadosti sa líši v závislosti od typu certifikátu. Najčastejšie sa vykonáva automaticky pomocou robota a trvá od 5 minút do 8 hodín. V prípade drahých certifikátov, pri ktorých sa overuje aj fyzické vlastníctvo lokality a prevádzkovej spoločnosti, sa overovanie vykonáva manuálne a môže trvať niekoľko dní.

Ak sa ponáhľate, existuje certifikačná agentúra `Let's encrypt`, ktorá automaticky autorizuje žiadosti s platnosťou 3 mesiace.

> **Upozornenie:** V niektorých prípadoch CA ponúka automatické generovanie požiadaviek. Tento postup sa neodporúča, pretože pozná súkromný kľúč. Ak tak urobíte, musíte vždy získať súkromný kľúč od agentúry a umiestniť ho do súboru na serveri rovnakým spôsobom, ako keby ste žiadosť generovali.

## Získanie kľúča pre konkrétnu CA/bezpečnostnú autoritu

Medzitým, kým sa čaká na schválenie žiadosti a vydanie certifikátu, zabezpečíme **verejný kľúč** certifikačnej autority. V Apache VirtualHost to predstavuje súbor `SSLCertificateChainFile` (v angličtine sa nazýva `Intermediate and Root CA`). Tento kľúč je v zásade verejný a informuje webový prehliadač o tom, kto je CA. Tento súbor sa zvyčajne stiahne z webovej lokality príslušného orgánu alebo sa nám doručí e-mailom.

Vždy by ste mali stiahnuť správny kľúč podľa zvoleného algoritmu. V prípade RapidSSL si ho môžete stiahnuť napríklad tu: https://knowledge.digicert.com/generalinformation/INFO1548.html

## Získanie certifikátu

Na základe tejto žiadosti nám certifikačná autorita vydala certifikát. V prípade Apache VirtualHost ho predstavuje súbor `SSLCertificateFile`.

Dôležité je, že certifikát má obmedzenú platnosť a tento proces musíme zopakovať (ideálne pred vypršaním platnosti). Dátum vypršania platnosti môžete v prehliadači Chrome ľahko zistiť kliknutím na zelený zámok v prehliadači a zobrazením podrobností o certifikáte, ak ho certifikačná autorita oznámila.

Po uplynutí platnosti je certifikát neplatný a stránka odmietne pripojenie a používateľom sa zobrazí chybové hlásenie o narušení bezpečnosti.

## Skontrolujte a vykonajte zmeny

Správnosť nastavení Apache možno čiastočne overiť príkazom `apache2ctl -S`.

Aby sa zmeny prejavili, je potrebné Apache reštartovať, napríklad príkazom:

sudo service apache2 restart

Ak sa nevyhodí žiadna chybová správa, okamžite prejdeme na overenie funkčnosti prostredníctvom webového prehliadača (odporúčam prehliadač Google Chrome, ktorý zobrazuje najviac podrobností).

V prípade chyby zavoláme príkaz `apache2ctl -S`, ktorý nám ukáže cestu k logom, kde približne vidíme, čo sa stalo. Je dôležité vykonať všetky kroky uvedené v tomto návode v správnom poradí a nezameniť žiadny z kľúčov.

## Budúca podpora

Nezabudnite si v kalendári poznačiť dátum skončenia platnosti, aby ste si mohli certifikát včas obnoviť. Niektoré certifikačné autority posielajú notifikačný e-mail, ale nie vždy je to spoľahlivé.

Proces obnovy a získanie nového certifikátu môžete vykonať paralelne počas prevádzky súčasného certifikátu a potom ho len vymeniť.

Na automatické obnovenie odporúčam [Certbot](https://certbot.eff.org), ktorý dokáže automaticky sledovať platnosť a obnovovať certifikáty. Obnovenie sa vykonáva napríklad pomocou cronu, ktorý raz za mesiac v noci zavolá príkaz na vydanie nového certifikátu a okamžite ho nasadí.

Ak nerozumiete správe certifikátov, je dobré hosťovať u zavedenej spoločnosti, ktorá vám dodá funkčný hosting vrátane certifikátu. Ušetríte si tak veľa starostí.
