Varför och hur man använder ramverk och bibliotek
=================================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	sv: varfoer-och-hur-man-anvaender-ramverk-och-bibliotek
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

Det finns ett berömt skämt om att programmerare börjar använda ramverk först när de skriver sina egna ramverk och upptäcker att de inte leder någonstans. Det roliga med detta är att det är sant. Jag har själv upplevt det. Två gånger till och med.

På <a href="https://nette.org">Nette</a>s huvudsida står det dock:

> Riktiga programmerare **använder inte ramverk**. De skriver webbprogram via kommandoraden direkt till servern. Detta är en hyllning till dem. För oss andra kommer Nette att göra vårt arbete mycket enklare och roligare.

Ramverkens roll i programutveckling
-----------------------------------

Det vet du säkert själv:

Du får en idé om ett fantastiskt projekt och börjar skriva ny kod direkt i ren PHP ... efter några timmar upptäcker du att mycket av arbetet fortfarande är repetitivt och att du löser grundläggande systemproblem. Till exempel hur man ansluter till en databas, hur man skapar ett formulär, hur man validerar data, hur man skickar ett e-postmeddelande och mycket mer.

Du kommer förmodligen att skriva egna funktioner som du anropar för dessa uppgifter. Om du skriver flera projekt samtidigt börjar du dela dessa funktioner mellan projekten i en stor, rörig fil och du ändrar dem kontinuerligt när du behöver dem.

Och när funktionerna har nått ett sådant utvecklingsstadium att man börjar bygga ett nytt projekt på dem varje gång och utvecklas direkt, då kan man tala om att man har skrivit sitt eget ramverk, eller åtminstone ett bibliotek.

Vad är och vad innebär en ram?
-------------------------

Som vi redan har visat med ett praktiskt exempel från livet är ett ramverk en samling av många funktioner (men helst klasser och objekt) som sparar programmerarens arbete. När han utvecklar behöver han inte tänka på hur han ska ansluta till en databas, utan han vet att det redan är programmerat någonstans och att "det bara fungerar".

Färdiga ramverk är alltså det samlade kunnandet från hundratals människor som har utvecklat tusentals program under en lång tidsperiod för att felsöka en lösning och ett ekosystem för att ta itu med nya projekt.

Med ramverk får du också en uppsättning **best practices**, som är sätt att lösa problem utan att behöva tänka på det. Om du stöter på ett problem har någon förmodligen löst det före dig, och det är alltid bättre att hitta en lösning i dokumentationen än att behöva lösa det på ett komplicerat sätt själv.

Abstrakt programmering och kapslingsprincipen
---------------------------------------------

Väl utformade ramverk som Nette gör det möjligt att programmera på en **mycket hög abstraktionsnivå**.

På så sätt behöver programmeraren (ramanvändaren) inte förstå vad som händer internt och exakt hur komponenterna fungerar internt, vilket sparar mycket tid och arbete. Han kan fokusera på att lösa de verkliga problemen i sin tillämpning och få resultat mycket snabbt.

David Grudl själv (författaren till Nette-ramverket) sa en gång att han utformade Nette för att kunna programmera en webbplats åt en kund på en kväll, när han kommer hem sent från puben och måste lansera webbplatsen på morgonen. Detta beror främst på att utvecklaren inte har något med teknik att göra.

För att detta ska fungera och för att utvecklaren ska kunna använda det färdiga ramverket som helhet är det mycket viktigt att använda <a href="/kapsling">kapslingsprincipen</a> korrekt.
