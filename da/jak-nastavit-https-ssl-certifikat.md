Sådan opretter du et HTTPS / SSL-certifikat - komplet vejledning
================================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	da: sadan-opretter-du-et-https-ssl-certifikat-komplet-vejledning
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

Når jeg implementerede `https`-protokollen på klientwebsteder, stødte jeg ofte på forskellige problemer, som skyldtes manglende forståelse af problemerne og for komplekse koncepter.

I denne vejledning beskriver jeg i detaljer de trin, der skal til for at opnå og implementere et gyldigt certifikat på en webserver.

**I hver underrubrik opsummerer jeg altid kort trinene for avancerede brugere, og nederst diskuterer jeg detaljerne for begyndere.**

> **Advarsel:** Hele processen med at implementere et certifikat kan tage over en time og er ofte uregelmæssig (webstedet kan være utilgængeligt).

Krav til input
-----------------

I vejledningen antages det, at vi har adgang til en Terminal-webserver, der kører på Linux og bruger Apache.

For Nginx gælder hele teorien på samme måde, blot er certifikatfilen anderledes forbundet med certifikatfilen.

## Oprettelse af forbindelse til webserveren

Vi opretter forbindelse til serveren via SSH.

- På Windows anbefaler jeg programmet **Putty**,
- På Mac eller Linux skal du blot bruge den indbyggede Terminal.

På Mac eller Linux skal du bruge kommandoen:

```htaccess
ssh uživatel@server
```

Jeg ønsker f.eks. at oprette forbindelse til brugeren `root` på webstedet `baraja.cz`:

```htaccess
ssh root@baraja.cz
```

Eller til brugeren `jan` til en bestemt IP-adresse:

```htaccess
ssh jan@127.0.0.1
```

Når du har indsendt din forespørgsel, vil forbindelsen enten blive oprettet direkte, eller du vil blive bedt om en adgangskode. Der vises ikke noget, når du indtaster adgangskoden, så bekræft adgangskoden med enter-tasten og vent på, at forbindelsen godkendes.

> **Varsling:** Hvis vi ikke har rettigheder til en handling, skal vi tildele dem. Enten skifter du direkte til brugeren `root` med kommandoen `sudo su`, eller du sætter ordet `root` i begyndelsen af den kommando, du vil udføre under root, f.eks. vil `root rm <name>` under root slette filen `<name>`. Når vi bruger kommandoen `sudo`, kan vi med jævne mellemrum blive bedt om at angive en adgangskode.

**Detaljer:**

SSH-adgang er oprettet af den pågældende hosting, hvor du har lejet en server.

- I tilfælde af VPS får du altid SSH-adgang.
- I tilfælde af hosting får du måske slet ikke SSH, og konfigurationen foregår på en anden måde (normalt via webgrænsefladen eller ved at kontakte support).

## Skift fra HTTP til HTTPS

Hvis du konverterer et eksisterende websted fra `http` til `https`, skal du sikre, at al trafik omdirigeres til den nye `https`-protokol.

I Apache kan dette nemt opnås ved at bruge en omdirigering i filen `.htaccess`:

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

Denne konfiguration sikrer, at alle anmodninger til `http` omdirigeres med `HTTP-kode 301` til `https`. Denne konfiguration er standardkonfigurationen for Nette-rammen, men gælder også for alle andre tilfælde.

**Detaljer:**

Filen `.htaccess` indeholder den specifikke webserverkonfiguration, der påvirker hver enkelt anmodning. Den er normalt placeret i samme mappe som `index.php` eller andre filer, der er tilgængelige fra internettet.

Indstillingen er kun gyldig for Apache-serveren og kan være deaktiveret eller begrænset for nogle værter. Du kan altid få mere detaljerede oplysninger ved at kontakte den hostingvirksomhed, hvor du er vært for dit websted.

-----

Nogle gange sker det, at `.htaccess` ikke vil samarbejde godt, og konfigurationen er ekstremt vanskelig (f.eks. for mange domæner, der hver især opfører sig forskelligt). I sådanne tilfælde kan viderestilling til HTTPS håndteres direkte i PHP i et snuptag:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'off') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Flyttet permanent');
	header('Beliggenhed:' . preg_replace('/^(https:\/\/(?:www\..)(.*)$/', '$1$2', $location));
	die;
}
```

Indsæt scriptet i `index.php`. Jeg anbefaler dog ikke denne løsning.

## Find filer med virtuelle værter

I Apaches tilfælde skal vi finde Virtual hosts-filen.

De ligger normalt på stien `/etc/apache2/sites-available`.

Liste indholdet af mappen med kommandoen `ls -al` og find den fil, hvor den virtuelle er oprettet for vores websted.

VirtualHosts er normalt placeret i filer med udvidelsen `.conf`. Standardindstillingen findes ofte i `000-default.conf`.

**Detaljer:**

- Mappen åbnes med kommandoen `cd`, f.eks. `cd /etc/apache2/sites-available`.
- Indholdet af filen skrives til terminalen med kommandoen `cat <name>` eller redigeres med kommandoerne `nano <name>` eller `vim <name>`.
- Du får normalt adgang til editoren ved at bruge tastaturgenvejen `CTRL + X` eller ved at trykke to gange på `ESC`.
- Filer kan delvist gennemses i GUI'en med Midnight commander (kommandoen `mc`), som i Ubuntu installeres ganske enkelt med `apt-get install mc` eller `sudo apt-get install mc`.

## Opsætning af en virtuel vært for port 443

I den virtuelle vært skal vi forberede en ny til port 443 (et almindeligt problem kan være blokering af port 443 på firewallen).

Indholdet af filen kan f.eks. se således ud:

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

På filen er det vigtigste område:

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

Det er helt afgørende at forstå denne konfiguration. Hvis du har brug for at google efter mere detaljerede oplysninger, kan du bruge ordene `SSLCertificateFile`, `SSLCertificateKeyFile` og `SSLCertificateChainFile`, som er fælles for alle Apache-servere.

> **Varsel:** SSL-problemet er relativt gammelt, og der bruges ofte forskellige navne for den samme ting for at bevare bagudkompatibilitet! Det er derfor vigtigt at forstå princippet.

**Detaljer:**

Det er vigtigt, at VirualHost indeholder:

- `<IfModule mod_ssl.c>` siger, at vi ønsker at bruge SSL
- `<VirtualHost *:443>`, at al kommunikation vil foregå på port 443
- `SSLEngine on`, at SSL er aktiveret for denne VirtualHost
- `SSLCertificateFile`, `SSLCertificateKeyFile` og `SSLCertificateChainFile` er filer med specifikke nøgler.

Der er ikke behov for andet.

## Forståelse af princippet i det, vi skal gøre, og hvordan certifikatet fungerer

I den endelige konfiguration af VirtualHost har vi i virkeligheden kun brug for 3 filer `SSLCertificateFile`, `SSLCertificateKeyFile` og `SSLCertificateChainFile`, som vi kan placere hvor som helst på serveren (navn og placering er ligegyldigt). Det er vigtigt at angive en arbejdssti, og at indholdet af filerne er gyldigt.

Den specifikke metode til fremskaffelse af certifikater kan variere fra CA til CA. Det vigtige er at forstå princippet og derefter anvende det på din sag.

| Fil | Betydning |
|---------------------------|-------------------------------------|
| `SSLCertificateFile` | Dette certifikat **er sendt af myndigheden** |
| `SSLCertificateKeyFile` | **Min genererede** private nøgle |
| `SSLCertificateChainFile` | Jeg downloadede fra nettet type **intermediate + root** |

## Hent den private nøgle og anmod om certifikatet

På serveren genererer vi først den private nøgle. Ordet **privat** er vigtigt, det betyder, at ingen andre end webserveren kender det. Ideelt set bør den aldrig forlade serveren og bør placeres et sikkert sted. Tab af denne nøgle betyder et tab af sikkerhed, da en angriber vil kunne udgive sig for at være en bestemt server.

Du kan generere nøglen ved at bruge kommandoen:

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

For at generere den skal vi have programmet `openssl` installeret på serveren, hvilket vi f.eks. kan få ved at køre `sudo apt install openssl`.

Der kan være flere slags nøgler, i dette tilfælde genererer vi en RSA-nøgle på 2048 bytes (`rsa:2048`).

Kommandoens output er 2 filer (som du navngiver efter dit domæne):

- `yourdomain.key` - dette er den private nøgle. Gem stien til denne nøgle i Apache VirtualHost til `SSLCertificateKeyFile`.
- `yourdomain.csr` - dette er en `certificate request`, eller en anmodning om at udstede et certifikat.

Indholdet af anmodningen skal altid forelægges for den kompetente myndighed til godkendelse. Dette gøres normalt via webgrænsefladen i administrationen på det websted, hvor certifikaterne sælges. Godkendelsen af anmodningen varierer afhængigt af certifikatets art. Den udføres oftest automatisk af en robot og tager mellem 5 minutter og 8 timer. I tilfælde af dyre certifikater, hvor det fysiske ejerskab af stedet og driftsselskabet også kontrolleres, foregår kontrollen manuelt og kan tage flere dage.

Hvis du har travlt, findes der et certificeringsagentur `Let's encrypt`, som automatisk godkender anmodninger med en gyldighed på 3 måneder.

> **Bemærk:** I nogle tilfælde tilbyder CA automatisk generering af anmodninger. Dette er ikke en anbefalet fremgangsmåde, da den kender den private nøgle. Hvis du gør dette, skal du altid få den private nøgle fra agenturet og placere den i en fil på serveren på samme måde, som hvis du genererede anmodningen.

## Fremskaffelse af en nøgle til en bestemt CA/sikkerhedsmyndighed

I mellemtiden, mens vi venter på, at anmodningen godkendes og certifikatet udstedes, sikrer vi den **offentlige nøgle** for CA'en. I Apache VirtualHost er dette repræsenteret af filen `SSLCertificateChainFile` (på engelsk hedder den `Intermediate and Root CA`). Denne nøgle er i princippet offentlig og fortæller webbrowseren, hvem CA'en er. Denne fil downloades normalt fra CA's websted eller leveres til os i en e-mail.

Du skal altid downloade den korrekte nøgle i henhold til den algoritme, du vælger. RapidSSL kan f.eks. downloades her: https://knowledge.digicert.com/generalinformation/INFO1548.html

## Hent certifikatet

På baggrund af anmodningen har certifikatudstederen udstedt et certifikat til os. I tilfælde af Apache VirtualHost repræsenteres den af filen `SSLCertificateFile`.

Det er vigtigt, at certifikatet har en begrænset gyldighed, og vi skal gentage denne proces (helst inden udløbet). Du kan nemt finde udløbsdatoen i Chrome ved at klikke på den grønne hængelås i browseren og se certifikatdetaljerne, hvis det er meddelt af CA.

Efter udløbsdatoen er certifikatet ugyldigt, og webstedet vil nægte forbindelser, og brugerne vil modtage en fejlmeddelelse om sikkerhedsbruddet.

## Kontroller og foretage ændringer

Korrektheden af Apache-indstillingerne kan delvist verificeres med kommandoen `apache2ctl -S`.

For at ændringerne kan træde i kraft, skal Apache genstartes, f.eks. med kommandoen:

```shell
sudo service apache2 restart
```

Hvis der ikke kommer nogen fejlmeddelelse, går vi straks over til at kontrollere funktionaliteten via en webbrowser (jeg anbefaler Google Chrome, som viser flest detaljer).

I tilfælde af en fejl kalder vi kommandoen `apache2ctl -S`, som kan vise stien til logfilerne, hvor vi kan se tilnærmelsesvis hvad der er galt. Det er vigtigt at udføre alle trin i denne vejledning i den rigtige rækkefølge og ikke at bytte om på nogen af tasterne.

## Fremtidig støtte

Husk at markere udløbsdatoen i din kalender, så du kan forny dit certifikat i tide. Nogle CA'er sender en e-mail med en meddelelse, men det er ikke altid pålideligt.

Du kan foretage fornyelsesprocessen og få et nyt certifikat sideløbende med, at det nuværende certifikat kører, og så kan du bare udskifte det på samme tid.

Til automatisk fornyelse anbefaler jeg [Certbot](https://certbot.eff.org), som automatisk kan overvåge gyldigheden og forny certifikater. Fornyelse sker f.eks. ved hjælp af en cron, der en gang om måneden om aftenen kalder en kommando til at udstede et nyt certifikat og distribuerer det med det samme.

Hvis du ikke forstår dig på certifikatstyring, er det en god idé at hoste hos et etableret firma, der leverer dig funktionel hosting, herunder et certifikat. Det sparer dig for en masse besvær.
