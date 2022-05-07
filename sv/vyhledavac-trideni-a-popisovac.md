Algoritm för sökmotorer på Internet - sortering och beskrivare
==============================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	sv: algoritm-foer-soekmotorer-pa-internet---sortering-och-beskrivare
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

I den här lektionen om principerna för en sökmotor på Internet kommer vi att förstå hur en sökmotor sorterar, beskriver och utvärderar resultaten.

Sortering av resultat
----------------

Låt oss föreställa oss en färdig tunna som för närvarande är klar på sökservern. Vår första sökfråga kommer in från användaren och nu måste vi göra den första "grova" sorteringen, som kommer att förfinas ytterligare.

Vi har följande exempel på en inmatningsfråga:

```txt
[O (and) pejskovi (and) a (and) kočičce]
```

Ja, detta är den form i vilken sökservern tar emot den bearbetade frågan från användaren och väntar nu på att resultatet ska returneras. Totalt har vi mindre än 300 ms för att göra detta, så snabbt :)

I det första steget får vi tunnor fulla med framtida resultat-ID:n (även kallade kandidater) och operationer som ska utföras. Det hämtade fatet är i vissa fall inte fullständigt, utan innehåller endast de värden som servern lyckades läsa från disken under ett förutbestämt tidsintervall (även kallat **timeout**). Vi får till exempel följande nummerserier (som delvis är sorterade i förväg):

| Barrels | Dokument |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| hund | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| pussy | 9,19,42,57,58,62,68,83 |

Vi utför nu en grov skärning av alla uppsättningar och får en lista över dokument-ID:n som är desamma i alla tunnor. I praktiken går vi igenom den kortaste listan och söker i de andra.

De gemensamma ID-koderna för alla tunnor är "19, 42, 58", så den framtida resultatsidan innehåller tre objekt i en ännu okänd ordning. Vi betraktar fortfarande dokumenten som ID:n eftersom detta sparar en enorm mängd data. I sökresultaten vill vi dock i allmänhet inte ange dokumentnummer för användaren, utan snarare dokumentets titel (rubrik), beskrivning, URL och annan information. Detta tillhandahålls av komponenten "descriptor".

Beskrivare
---------

Från föregående kapitel har vi fått en lista över kandidater för framtida resultat. Vi hämtade dem i sorterad form enligt systemet, dvs. så som sökmotorn rangordnade dem i indexet. Vid denna punkt kan vi inte längre utföra en sekventiell läsning av stora data, utan måste i stället gå igenom någon typ av relationsdatabas för att hitta ytterligare information för en annan typ av sortering som är mer begriplig för användaren.

En databasdel kan till exempel se ut så här:


| ID | Titel | Etikett | URL | Rank | Rank |
|----|---------|---------|-----|------|
| 19 | Josef Čapek: En berättelse om en hund och en katt | Det var då hunden och katten fortfarande var jordbrukare tillsammans; de hade sitt lilla hus vid skogen och bodde där tillsammans och ville göra allting på samma sätt som de stora människorna gör det | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| 42 | Talking about the dog and the cat | "Okej", sa hunden, och katten tog tvål och en kruka med vatten, knäböjde ner på golvet, tog hunden som borste och skrubbade hela golvet med hunden. Golvet var blött | www.capek.narod.ru/povidani.htm | 86 |
| 58 | Hur hunden och katten gjorde en tårta till semestern| I morgon var det hundens semester och kattens födelsedag. Barnen visste detta och ville överraska hunden och katten på deras födelsedag. De funderade på vad de kunde göra för hunden och | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Beräkning av relevans
-----------------

Det sista steget är att beräkna svarets relevans. För de flesta resultat räcker det att ange relevansen som ett enda tal med en fast radie som resultaten sorteras efter. Resultaten sorteras återigen "grovt" i flera grupper (antalet grupper beror på variansen i rangerna) och dessa grupper bearbetas vidare.

De grupper som har högst rankning tas först, ofta räcker detta för att sortera den första resultatsidan och vi behöver inte ta itu med något annat. Men om flera olika dokument har samma rang måste vi göra en mer detaljerad analys och beräkna ytterligare extra rangordningsvärden för att hjälpa till att rangordna sidan. Ytterligare rangordning kan till exempel vara kvaliteten på länkarna, dokumentets ålder, titelns längd, URL:ns utseende och känsla och många andra faktorer.

Återlämnande av resultat till användaren
---------------------------

Hurra! Vi har resultaten och nu återstår bara att göra upp sidan för användaren. Resultatpaketet lämnas tillbaka till servern, som anpassar resultaten till en förbeställd mall, lägger in reklambanners runt sidan och skickar tillbaka sidan till användaren. Samtidigt kan den också utföra andra hjälpoperationer, t.ex. caching av resultaten. Om någon annan söker på samma fråga inom en snar framtid (t.ex. inom den senaste timmen) kommer resultaten inte att sökas igen, utan bara läsas från det tillfälliga minnet, vilket ofta bidrar till att förbättra sökmotorns prestanda.

Slutsats och inblick i semantiken
---------------------------

Syftet med den här artikeln var att beskriva de grundläggande principerna för hur sökmotorer fungerar internt och hur de lyckas hitta ett teoretiskt obegränsat antal dokument på en fortfarande rimlig tid. Detta är enorma matematiska optimeringar som har utvecklats under många år av de bästa programmeringsgrupperna.

En fullständig beskrivning av detaljerna ligger utanför ramen för detta dokument. Det hela kompliceras också av att de flesta andra steg inte beskrivs i detalj av någon av sökmotorerna, eftersom de är deras affärshemligheter.

Det är också viktigt att notera att sökmotorerna fortfarande inte förstår sidornas innehåll och resultatens innebörd, utan det är fortfarande bara en statistisk beräkning som bygger på rå datorkraft och människors förmåga att länka till kvalitetstext, och detta system är också ganska sårbart (ta exemplet med "Google-bomben", där en lömsk webmaster tvingar fram ett innehåll som är irrelevant för en sökfråga).

Detta problem bör delvis kunna lösas med hjälp av artificiell intelligens, som alla sökmotorer gradvis försöker integrera. Detta är dock fortfarande en avlägsen framtidsmusik, eftersom det inte är någon lätt uppgift och oftast bara handlar om att förbättra de statistiska beräkningsmetoderna. Låt Google Graph Search vara ett exempel, som försöker besvara frågan direkt (även om det i sin interna struktur fortfarande bara söker i en fördefinierad kunskapsbas).

Så nästa gång du ställer en fråga till din favoritsökmotor, kom ihåg hur mycket arbete sökmotorn har haft och hur många TB data den har läst från disken för att hitta de tio bästa dokumenten för din sökfråga.
