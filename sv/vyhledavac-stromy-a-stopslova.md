Algoritmen för sökmotorer på Internet - Träd och StopLead
=========================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	sv: algoritmen-foer-soekmotorer-pa-internet---traed-och-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

Varje sekund läggs 5 miljoner nya sidor till på Internet, och denna takt ökar hela tiden. För att ge ordning åt detta enorma hav av information och för att hitta något i det finns det sökmotorer. Följande arbete syftar till att introducera frågan om sökning och förklara hela processen från skapandet av en ny sida till att den hittas i en sökmotor.

Det är inte lätt att hitta och sortera miljarder dokument. Enbart Google behöver 300 000 webbservrar för att hantera denna uppgift på bara några timmar. Faktum är att sökningen efter din fråga sker långt innan du ens ställer den. Google har redan lagrat de sökresultat som du kommer att söka efter de kommande dagarna i sitt minne.

Arkitektur för sökmotorer
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Gemensam sökmotorarkitektur för upp till 50 miljoner dokument" class="w-100 mb-3">

En korrekt utformad sökmotor innehåller många komponenter, varav det finns flera hundra. Detta diagram beskriver de mest grundläggande elementen som en sökmotor måste innehålla för att fungera. Detta är en gammal arkitektur som inte längre används och som fungerade fram till omkring 2005, då det bara fanns några få miljoner dokument på webben. Denna arkitektur tillåter inte en "oändlig" mängd innehåll och inte heller att sökservrarna (bassökning) kan vara ur funktion. Om en enskild komponent skulle vara ur funktion, skulle hela systemet upphöra att fungera och sökmotorn skulle vara otillgänglig.

En fullständig beskrivning av aktuella trender i utformningen av nya arkitekturer ligger utanför ramen för denna artikel, eftersom dessa vanligtvis är affärshemligheter för stora företag. Syftet med detta dokument är att förklara hur denna grundläggande arkitektur fungerar. De enskilda komponenterna förklaras i detalj i de följande kapitlen.

Inmatning av förfrågningar
------------

Den mest synliga komponenten, även för vanliga användare, är inmatningen av frågor, som för närvarande oftast representeras som en textruta. Inmatningsfältet finns på en särskild sida som vanligtvis endast är avsedd för inmatning.

I nästa steg skriver användaren in söksträngen och skickar formuläret till sökmotorns servrar, vilket inleder kommunikationen med dispenseringskomponenten, ibland även kallad huvudsökning. Dispenseringsservrar är byggda för att hantera stora belastningar från användare och måste kunna hantera alla sökare samtidigt.

Arkitekturen för en sändningsserver är vanligtvis en enkel Apache-server med en grundläggande installation och utmärkt nätverksbandbredd. När en fråga kommer in till servern behandlas den genom regressionsbeslutsträd och en "frågekarta" skapas som fångar den fullständiga semantiken kring de andra betydelserna för att göra det tydligt vad användaren faktiskt letar efter. Metoder för omskrivning av frågor ligger för långt utanför den här uppsatsen, så vi kommer bara att visa allmänna beskrivningar av hur omskrivning kan se ut och inte kan se ut.

Tänk på följande fråga:

```txt
"O pejskovi a kočičce"
```

Denna fråga omvandlas till ett binärt träd som fångar dess semantiska essens:

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Symboliskt diagram för parseträd" class="w-100 mb-3">

Hela poängen med ett parse tree är att fånga upp hur en sökmotor ser på sökresultaten. Trädet skickas tillsammans med sökfrågan till de extra sökservrarna (i detta dokument benämnda Base search), där enskilda nyckelord söks efter (indexläsningar) och sedan sorteras enligt regler (genom operationer).

| Operatör | Sortering |
|----------|------------------
| AND | Korsning |
| OR | Summa |
| NOT | Komplement |

Det finns ingen egentlig sortering av sökresultaten, utan den här komponenten utför bara snabba binära operationer på stora mängder data (ofta miljontals poster) och jämför i princip bara ID:erna för enskilda dokument.

Hur själva sorteringen av sökresultaten på resultatsidan fungerar diskuteras i följande avsnitt. Utdataservern används helt enkelt för att kombinera en mängd data från olika sökservrar för att öka den totala prestandan och sprida belastningen, och matar sedan in de upptäckta värdena på resultatsidan (detta kallas en "webbsida"), som innehåller sammansatt information från dussintals servrar.

Omskrivning till ett binärt träd
-----------------------

Som vi nämnde i föregående kapitel bygger hela sökningen på binära träd som talar om hur resultaten kommer att se ut och vad du ska leta efter. Hela sökmotorns "logik" och "intelligens" är direkt beroende av kvaliteten på det program som skapar trädet - nämligen deskriptorn.

Ett korrekt konstruerat träd bör innehålla all nödvändig metainformation om sökfrågan i en sådan form att det lätt kan bearbetas även med hjälp av sekventiell diskläsning och bör eliminera slumpmässiga åtkomster till minnet, så det innehåller vanligtvis de enskilda sökorden (från användaren), deras transkriptioner (och stavade former) och sist men inte minst länkarna mellan orden, dvs. deras relativa avstånd i texten.

Metoder för transkription av förfrågningar
---------------------

Det mest grundläggande sättet att transkribera en sökning är att dela upp den i enskilda ord (enligt mellanslag), som kommer att bearbetas vidare. Det andra steget är att söka efter stoppord och ersätta dem med en variabelpekare (eftersom det, som vi kommer att visa senare, inte är någon idé att söka efter stoppord överhuvudtaget).

Låt oss ha följande fråga: "Om en hund och en katt", vars transkription i sin enklaste form ser ut så här:

```txt
"O (and) pejskovi (and) a (and) kočičce"
```

Det andra steget är att eliminera ord som inte har någon relevans för sökningen. Naturligtvis är till exempel ordet "about" eller "and" meningsfullt för oss människor, men inte för en databassökning, eftersom det kan ersättas med ett annat ord och resultaten listas. Målet bör inte vara att hitta en bokstavsmässig matchning av sökfrågan mot indexet, eftersom det skulle leda till dåliga resultat, eftersom ord i text på Internet ofta har olika stavningar och denna metod försöker hitta resultat i andra former än de som vi fick från användaren. Detta är alltså det första tecknet på en "intelligent" sökmotor.

Dessa meningslösa ord kallas ofta "stoppord", och varje sökmotor bör ha ett index över sådana ord och detta index bör uppdateras ofta (manuellt).

```txt
["pejskovi (and) * (and) kočičce"]
```

Vi letar nu efter två olika ord ("hund" och "katt") med ett annat ord emellan (internt markerar vi det med en asterisk).

Syftet med den här ändringen är inte att öka kvaliteten på sökningen utan att öka prestandan. Detta beror på att när vi söker efter ett ord som är för vanligt förekommande sker en snabb belastning, och den här ändringen underlättar processen - särskilt genom att inte söka efter ett ord som ändå finns på den positionen i de flesta dokument.

Det är också viktigt att notera att vi inte alltid kan göra denna justering (utelämna ett ord), så varje sökmotor bör ha ett separat index över fraser som hittas på Internet för att kontrollera vilken justering som leder till bra resultat och vilken som inte gör det. Ibland gör dock en sökmotor denna justering även om den inte har frasen i sitt index - särskilt när en användare söker efter ett betydelsefullt ord som förväntas ha betydelsefulla sidor på de första platserna, som åsidosätter detta "fel" eftersom användarna vanligtvis frågar efter dem ändå.

Det sista steget är att arbeta med det engelska språket och "rensa" sökningen från onödiga tecken. Om användaren till exempel frågar efter ordet "tvättmaskin" förväntar sig användaren vanligtvis resultat endast för ordet "tvättmaskin" och vi kan bortse från kommatecknet.

De exakta metoderna för hur detta system fungerar är inte kända och varje sökmotor har sina egna. Man tror att det oftast bara är en statistisk modell som gör dessa justeringar baserat på kunskap om miljarder texter i databasen.

Engelska transkriptioner är också en vetenskap i sig, så även här ges endast en grundläggande beskrivning. I de flesta fall räcker det med att lägga till de böjda formerna (enligt ordboken) till varje ord och söka efter dem tillsammans med ordet. På så sätt kan sökmotorn hitta dokument där orden inte bara förekommer i sin grundform (som användaren anger), utan den kan också hitta de böjda versionerna - en mycket användbar funktion. Problemet med detta tillvägagångssätt ligger i böjningen av obskyra och problematiska ord, och även i böjningen av korta frågor (och därför är det inte möjligt att utifrån sammanhanget avgöra hur ordet ska böjas). Låt ordet "games" vara ett exempel på en problematisk böjning.

Med en engelsk fråga, "I love her", kommer en dåligt utformad böjningsapparat att böja ordet "games" till exempel som "games, gaming, gaming room, ...", så det är också nödvändigt att indexera de omgivande orden som fraser och endast böja dem i välkända situationer, eller använda ett annat förfarande (det är här som fonetiska algoritmer ofta kommer in).

Det sista steget är att skicka det färdiga trädet till basens sökservrar, där den egentliga sökningen i tunnor kommer att äga rum - detta är ämnet för nästa kapitel.
