Hur du konfigurerar ett HTTPS-/SSL-certifikat - fullständig guide
=================================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	sv: hur-du-konfigurerar-ett-https--ssl-certifikat---fullstaendig-guide
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

När jag använde `https`-protokollet på klienters webbplatser stötte jag ofta på olika svårigheter som berodde på bristande förståelse för problemen och alltför komplexa begrepp.

I den här handledningen beskriver jag i detalj stegen för att skaffa och distribuera ett giltigt certifikat på en webbserver.

**I varje underrubrik sammanfattar jag alltid kortfattat steget för avancerade användare, och längst ner diskuterar jag detaljerna för nybörjare.**

> **Varning:** Hela processen för att distribuera ett certifikat kan ta över en timme och är ofta intermittent (webbplatsen kan vara otillgänglig).

Ingångskrav
-----------------

I instruktionerna förutsätts att vi har tillgång till en Terminal-webbserver som körs på Linux och som använder Apache.

För Nginx gäller hela teorin på samma sätt, bara kopplingen av certifikatfilen är annorlunda.

## Anslutning till webbservern

Vi ansluter till servern via SSH.

- På Windows rekommenderar jag programmet **Putty**,
- På Mac eller Linux använder du bara den inbyggda terminalen.

På Mac eller Linux använder du kommandot:

```htaccess
ssh uživatel@server
```

Jag vill till exempel ansluta till användaren `root` på webbplatsen `baraja.cz`:

```htaccess
ssh root@baraja.cz
```

Eller till användaren `jan` till en specifik IP-adress:

```htaccess
ssh jan@127.0.0.1
```

När du har skickat in din fråga kommer anslutningen att ske direkt eller så kommer du att bli ombedd att ange ett lösenord. Inget visas när du skriver in lösenordet, så bekräfta lösenordet med enter-tangenten och vänta på att anslutningen ska godkännas.

> **Varning:** Om vi inte har rättigheter till en åtgärd måste vi tilldela dem. Antingen byter du direkt till användaren `root` med kommandot `sudo su`, eller så föregår du kommandot som du vill utföra under root med ordet `root` i början, till exempel `root rm <name>` under root kommer att radera filen `<name>`. När du använder kommandot `sudo` kan du regelbundet bli ombedd att ange ett lösenord.

**Detaljer:**

SSH-åtkomst konfigureras av det särskilda webbhotell där du har hyrt en server.

- När det gäller VPS får du alltid SSH-åtkomst.
- När det gäller webbhotell kanske du inte får SSH alls, och konfigurationen görs på annat sätt (vanligtvis via webbgränssnittet eller genom att kontakta supporten).

## Byte från HTTP till HTTPS

Om du konverterar en befintlig webbplats från `http` till `https` måste du se till att all trafik omdirigeras till det nya `https`-protokollet.

När det gäller Apache kan detta enkelt uppnås genom att använda en omdirigering i filen `.htaccess`:

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

Den här konfigurationen säkerställer att alla förfrågningar till `http` omdirigeras med `HTTP code 301` till `https`. Den här konfigurationen är standardkonfigurationen för Nette-ramverket, men gäller även i alla andra fall.

**Detaljer:**

Filen `.htaccess` innehåller den specifika webbserverkonfiguration som påverkar varje begäran. Den placeras vanligtvis i samma katalog som `index.php` eller andra filer som är tillgängliga från Internet.

Inställningen gäller endast för Apache-servern och kan vara inaktiverad eller begränsad för vissa värdar. Om du vill ha mer detaljerad information bör du alltid kontakta det webbhotell där du har din webbplats.

-----

Ibland händer det att `.htaccess` inte vill samarbeta väl och att konfigurationen är extremt svår (till exempel för många domäner som alla beter sig olika). I sådana fall kan omdirigering till HTTPS hanteras direkt i PHP:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'off') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Flyttas permanent');
	header('Plats:' . preg_replace('/^(https:\/\/(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

Lägg skriptet i `index.php`. Jag rekommenderar dock inte denna lösning.

## Hitta filer med virtuella värdar

När det gäller Apache måste vi hitta filen Virtual hosts.

De finns vanligtvis i sökvägen `/etc/apache2/sites-available`.

Lista innehållet i katalogen med kommandot `ls -al` och hitta filen där den virtuella webbplatsen är konfigurerad för vår webbplats.

VirtualHosts finns vanligtvis i filer med tillägget `.conf`. Standardvärdet finns ofta i `000-default.conf`.

**Detaljer:**

- Katalogen öppnas med kommandot `cd`, till exempel `cd /etc/apache2/sites-available`.
- Filens innehåll skrivs till terminalen med kommandot `cat <name>` eller redigeras med kommandona `nano <name>` eller `vim <name>`.
- Redigeraren nås vanligtvis genom att använda tangentbordsgenvägen `CTRL + X` eller genom att trycka på `ESC` två gånger.
- Filer kan delvis bläddras i GUI med Midnight commander (kommandot `mc`), som i Ubuntu installeras helt enkelt med `apt-get install mc` eller `sudo apt-get install mc`.

## Konfigurera en virtuell värd för port 443

I den virtuella värden måste vi förbereda en ny för port 443 (ett vanligt problem kan vara att port 443 blockeras av brandväggen).

Innehållet i filen kan till exempel se ut så här:

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

Det viktigaste området är filen:

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

Det är absolut nödvändigt att förstå denna konfiguration. Om du behöver googla efter mer detaljerad information kan du använda orden `SSLCertificateFile`, `SSLCertificateKeyFile` och `SSLCertificateChainFile` som är gemensamma för alla Apache-servrar.

> **Varning:** SSL-problemet är relativt gammalt och olika namn används ofta för samma sak för att upprätthålla bakåtkompatibilitet! Det är därför viktigt att förstå principen.

**Detaljer:**

Det är viktigt att VirualHost innehåller:

- `<IfModule mod_ssl.c>` säger att vi vill använda SSL
- `<VirtualHost *:443>` för att all kommunikation ska ske på port 443.
- `SSLEngine on` att SSL är aktiverat för detta VirtualHost.
- `SSLCertificateFile`, `SSLCertificateKeyFile` och `SSLCertificateChainFile` är filer med specifika nycklar.

Inget annat behövs.

## Förstå principen om vad vi ska göra och hur certifikatet fungerar.

I den slutliga konfigurationen av VirtualHost behöver vi egentligen bara tre filer: `SSLCertificateFile`, `SSLCertificateKeyFile` och `SSLCertificateChainFile`, som vi kan placera var som helst på servern (namn och plats spelar ingen roll). Det är viktigt att ange en arbetssökväg och att filernas innehåll är giltigt.

Den specifika metoden för att erhålla certifikaten kan variera från en certifikatutfärdare till en annan. Det viktiga är att förstå principen och sedan tillämpa den på ditt fall.

| Fil | Betydelse |
|---------------------------|-------------------------------------|
| `SSLCertificateFile` | Detta certifikat **skickas av myndigheten** |
| `SSLCertificateKeyFile` | **Min genererade** privata nyckel |
| `SSLCertificateChainFile` | Jag laddade ner från webbtypen **intermediate + root** |

## Hämta den privata nyckeln och begära certifikatet

På servern genererar vi först den privata nyckeln. Ordet **privat** är viktigt, det betyder att ingen annan än webbservern känner till det. Helst ska den aldrig lämna servern och placeras på en säker plats. Att förlora denna nyckel innebär en förlust av säkerhet, eftersom en angripare kan utge sig för att vara en specifik server.

Använd kommandot för att generera nyckeln:

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

För att generera den måste vi ha programmet `openssl` installerat på servern, vilket vi till exempel kan få genom att köra `sudo apt install openssl`.

Det kan finnas flera olika typer av nycklar, i det här fallet genererar vi en RSA-nyckel som är 2048 byte lång (`rsa:2048`).

Resultatet av kommandot är två filer (som du namnger enligt din domän):

- `yourdomain.key` - detta är den privata nyckeln. Spara sökvägen till den här nyckeln i Apache VirtualHost till `SSLCertificateKeyFile`.
- `yourdomain.csr` - detta är en `certificate request`, eller en begäran om att utfärda ett certifikat.

Innehållet i begäran måste alltid lämnas till den behöriga myndigheten för godkännande. Detta görs vanligtvis via webbgränssnittet i administrationen på den webbplats där certifikaten säljs. Godkännandet av begäran varierar beroende på vilken typ av certifikat det rör sig om. Det görs oftast automatiskt av en robot och tar mellan 5 minuter och 8 timmar. När det gäller kostsamma certifikat, där man också kontrollerar den fysiska äganderätten till anläggningen och det företag som driver anläggningen, sker kontrollen manuellt och kan ta flera dagar.

Om du har bråttom finns det en certifieringsbyrå `Let's encrypt` som godkänner förfrågningar automatiskt med en giltighetstid på tre månader.

> **Varning:** I vissa fall erbjuder certifikatutfärdaren automatisk generering av begäran. Detta rekommenderas inte eftersom den känner till den privata nyckeln. Om du gör detta måste du alltid få den privata nyckeln från byrån och placera den i en fil på servern på samma sätt som om du genererade begäran.

## Upphämtning av en nyckel för en specifik CA/ säkerhetsmyndighet

Under tiden, medan vi väntar på att begäran ska godkännas och certifikatet utfärdas, säkrar vi certifikatutfärdarens **offentliga nyckel**. I Apache VirtualHost representeras detta av filen `SSLCertificateChainFile` (på engelska heter den `Intermediate and Root CA`). Denna nyckel är i princip offentlig och talar om för webbläsaren vem CA är. Filen laddas vanligtvis ner från den behöriga myndighetens webbplats eller levereras till oss i ett e-postmeddelande.

Du bör alltid ladda ner rätt nyckel enligt den algoritm du väljer. RapidSSL kan till exempel laddas ner här: https://knowledge.digicert.com/generalinformation/INFO1548.html

## Hämta certifikatet

Certifikatmyndigheten har utfärdat ett certifikat till oss på grundval av begäran. När det gäller Apache VirtualHost representeras den av filen `SSLCertificateFile`.

Det är viktigt att certifikatet har en begränsad giltighetstid och vi måste upprepa processen (helst innan den löper ut). Du kan enkelt hitta utgångsdatumet i Chrome genom att klicka på det gröna hänglåset i webbläsaren och visa certifikatinformationen, om den meddelas av certifikatutfärdaren.

Efter utgångsdatumet är certifikatet ogiltigt och webbplatsen vägrar att ansluta och användarna får ett felmeddelande om säkerhetsöverträdelsen.

## Kontrollera och gör ändringar

Att Apache-inställningarna är korrekta kan delvis kontrolleras med kommandot `apache2ctl -S`.

För att ändringarna ska träda i kraft måste Apache startas om, till exempel med kommandot:

```shell
sudo service apache2 restart
```

Om inget felmeddelande visas går vi genast över till att verifiera funktionaliteten via en webbläsare (jag rekommenderar Google Chrome, som visar mest detaljer).

Om ett fel uppstår kallar vi kommandot `apache2ctl -S`, vilket kan visa sökvägen till loggarna, där vi kan se ungefär vad som är fel. Det är viktigt att alla steg i den här handboken utförs i rätt ordning och att inga nycklar byts ut.

## Framtida stöd

Glöm inte att markera utgångsdatumet i kalendern så att du kan förnya ditt certifikat i tid. Vissa certifikatutfärdare skickar ett meddelande via e-post, men detta är inte alltid tillförlitligt.

Du kan göra förnyelseprocessen och skaffa ett nytt certifikat parallellt med att det nuvarande certifikatet är igång och sedan byta ut det samtidigt.

För automatisk förnyelse rekommenderar jag [Certbot] (https://certbot.eff.org), som automatiskt kan övervaka giltigheten och förnya certifikat. Förnyelsen görs till exempel av en cron som en gång i månaden på kvällen anropar ett kommando för att utfärda ett nytt certifikat och distribuerar det omedelbart.

Om du inte förstår dig på certifikathantering är det en bra idé att ha ett värdskap hos ett etablerat företag som tillhandahåller ett fungerande värdskap, inklusive ett certifikat. Det sparar dig en hel del besvär.
