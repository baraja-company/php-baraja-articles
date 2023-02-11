Hastende reparation af overbelastet server
==========================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	da: hastende-reparation-af-overbelastet-server
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

Et eksternt overvågningsværktøj vil rapportere til dig, at den gennemsnitlige svartid for de 5 overvågede URL'er er fordoblet i løbet af de sidste 30 minutter. Projektet kører på en enkelt fysisk server, som ikke er under din ledelse, og som kører et sted i et datacenter. Du opretter forbindelse via SSH, starter htop og kan se, at CPU-belastningen er 95 %, og at hukommelsen længe har været overfyldt.

Ifølge git ved du, at de for ca. en uge siden foretog en databasemigrering til en ny tabelstruktur, og en kollega skriver i chatten, at han var nødt til at køre migreringen natten over, fordi genberegningen af kolonner og indekser tog ca. 5 timer, hvor næsten hele databasen var låst, og hverken INSERT eller SELECT virkede.

Så ydelsesproblemerne skyldes sandsynligvis uhensigtsmæssigt udformede indekser, dårligt omdesignede SQL-forespørgsler eller stor connection pooling. Der er ikke tid til at lave en omlægning, der er 7.000 brugere på webstedet ifølge Google Analytics, og en afbrydelse i 5 timer ville betyde en omdømmerisiko for kunden og et tab af titusindvis til hundredtusindvis af kroner i den periode (det er svært at vurdere, projektionisterne finder på nok). Du er klar over, at det ikke er nok kun at teste funktionaliteten i et testmiljø, men at du også skal gennemføre en belastningstest.

Da der er tale om en vigtig e-handelsbutik for din største kunde, og du forventer, at situationen kan blive værre, har du 30 sekunder til at træffe en beslutning.

Hvordan går du videre?

1. Du gør ingenting. Siden vil være langsommere, men så længe serveren kan klare det, er det vel i orden...
2. Du vil forsøge at udarbejde en plan for at ændre DB-strukturen og køre den så hurtigt som muligt.
3. Du migrerer webstedet til en mere kraftfuld server (men det betyder, at du hurtigt hæver prisen over for kunden og derefter venter måske 6 timer på, at migreringen er færdig, fordi du har hundredvis af GB databastabeller).
4. Du kan finde ud af, hvilke SQL-forespørgsler og hvilke sider der tager mest tid, og så kan du simpelthen deaktivere dem.
5. Du kører Cloudflare og lader den statisk tjekke ind, hvad den kan.
6. Noget andet magi (jeg tror ikke, der er meget at komme med)...
