Brådskande reparation av överbelastad server
============================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	sv: bradskande-reparation-av-oeverbelastad-server
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

Ett externt övervakningsverktyg rapporterar att den genomsnittliga svarstiden för de fem övervakade webbadresserna har fördubblats under de senaste 30 minuterna. Projektet körs på en enda fysisk server som du inte har hand om och som körs någonstans i ett datacenter. Du ansluter via SSH, startar htop och ser att CPU-belastningen är 95 % och att minnet är överfullt.

Enligt git vet du att de för ungefär en vecka sedan gjorde en databasmigrering till en ny tabellstruktur, och en kollega skriver i chatten att han var tvungen att köra migreringen över natten, eftersom omräkningen av kolumner och index tog ungefär 5 timmar, under vilka nästan hela databasen var låst och varken INSERT eller SELECT fungerade.

Så prestandaproblemen beror troligen på felaktigt utformade index, dåligt utformade SQL-förfrågningar eller stor anslutningspooling. Det finns ingen tid för en återgång, det finns 7 000 användare på webbplatsen enligt Google Analytics, och ett avbrott i fem timmar skulle innebära en risk för kundens rykte och en förlust av tiotals till hundratusentals kronor under den tiden (det är svårt att uppskatta, projektionisterna hittar på tillräckligt mycket). Du inser att det inte räcker med att bara testa funktionalitet i en testmiljö, utan du måste också genomföra ett belastningstest.

Eftersom detta är en viktig e-handelsbutik för din största kund och du räknar med att situationen kan förvärras har du 30 sekunder på dig att fatta ett beslut.

Hur går du vidare?

1. Du gör ingenting. Webbplatsen kommer att bli långsammare, men så länge servern klarar av det är det väl okej...
2. Du ska försöka förbereda en plan för att återställa DB-strukturen och köra den så snart som möjligt.
3. Du flyttar webbplatsen till en mer kraftfull server (men det innebär att du snabbt höjer priset för kunden och sedan väntar i kanske sex timmar på att migreringen ska vara klar, eftersom du har hundratals GB databastabeller).
4. Du kan ta reda på vilka SQL-förfrågningar och vilka sidor som tar mest tid och helt enkelt inaktivera dem.
5. Du kör Cloudflare och låter den statiskt kontrollera vad den kan.
6. Någon annan magi (jag tror inte att det finns mycket att komma på)...
