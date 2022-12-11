Kommunikation via SSH och RSA2-nyckel
=====================================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	sv: kommunikation-via-ssh-och-rsa2-nyckel
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH är ett nätverksprotokoll för krypterad fil- och terminalöverföring. SSH används oftast för fjärrstyrning av en webbserver och säker filöverföring. Till skillnad från FTP erbjuder den en inhemsk krypterad anslutning. SSH kommunicerar via standardporten 22. Anslutningen initieras med målserverns användarnamn och adress. Ett lösenord (rekommenderas inte) eller en SSH RSA2-nyckel (rekommenderas) kan användas för autentisering.

Att få (generera) nyckeln
--------------------------

Innan vi ansluter till servern måste vi skaffa (eller generera) vår första SSH RSA2-nyckel. Det är viktigt att det är en RSA2-algoritm. Detta beror på att det finns ett antal nycklar och att alla inte kan användas.

I Linux används verktyget `ssh-keygen` för att generera den, till vilket vi anger nyckelns komplexitet (4096 i det här fallet) och e-postadressen för den auktoriserade användaren:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Efter att ha kört kommandot kommer vi att bli ombedda att ange sökvägen för att lagra nyckeln och ett eventuellt lösenord (auktoriseringslösenord). Ange inget som sökväg (standardplatsen väljs automatiskt) och ange lösenfrasen som alternativ (om du gör det måste du ange samma lösenord varje gång innan du använder nyckeln).

Den genererade nyckeln sparas automatiskt på standardplatsen `~/.ssh`, dvs. i katalogen `.ssh` i den aktuella användarens hemkatalog.

Generera en SSH-nyckel i Windows
-------------------------------

Tyvärr har Windows ingen standardsökväg för SSH-nyckeln. Det är därför idealiskt att installera till exempel verktyget `Putty` och `PuttyGen` för att generera nyckeln. Välj alltid algoritmen `RSA2`. Återigen kommer ett par nycklar att genereras, så förvara dem säkert någonstans. Innan du använder SSH-nyckeln i `Putty` måste du välja den diskbana från vilken nyckeln ska hämtas. I Linux behövs inte detta, eftersom det finns en standarddiskväg.

Nyckelsäkerhet
---------------

När nycklar genereras, genereras två nycklar. En offentlig nyckel (den som du ger till den andra parten för att möjliggöra kommunikation) och en privat nyckel (den som är din egen, som du aldrig berättar för någon och som används för att dekryptera kommunikation).

Det är viktigt att du aldrig förlorar den privata nyckeln. Att förlora den innebär att hela kommunikationen bryts!

Det rekommenderas i allmänhet att generera ett unikt offentligt/privat nyckelpar för varje enhet och användare för att minska sannolikheten för läckage. Men om du vill överföra nyckeln mellan olika enheter kan du göra det. SSH-nyckeln är på lösenordsnivå, så när du flyttar den till en annan maskin fungerar anslutningen direkt.

Vissa servrar kommer ihåg vilken enhet de senast kommunicerade med via SSH. Det är därför möjligt att anslutningen inte fungerar när du flyttar nyckeln till den nya datorn. I det här fallet måste du rensa nyckelcachen på servern.

Auktorisera nyckeln för att ansluta till servern
--------------------------------------

Kommandot `ssh` används för att ansluta till servern. Ange bara användaren och domänen:

```shell
ssh root@baraja.cz
```

Därefter försöker den upprätta en SSH-anslutning. Om du har en fungerande och korrekt konfigurerad SSH-nyckel kommer anslutningen att ske automatiskt. Om inte måste du ange ett lösenord.

Om du vill autentisera dig mot din server med en SSH-nyckel i stället för ett lösenord måste du överföra din **offentliga nyckel** till servern.

Visa den helt enkelt med kommandot:

```shell
cat ~/.ssh/id_rsa.pub
```

Kopiera hela innehållet till målservern på platsen `~/.ssh/authorized_keys`. Om du har mer än en nyckel, var och en på en separat linje.
