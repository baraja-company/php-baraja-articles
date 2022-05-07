Hur en programmerare lever i ett internt utvecklingsteam
========================================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	sv: hur-en-programmerare-lever-i-ett-internt-utvecklingsteam
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

Det finns ett högt pris att betala för ärlighet.

Den här webbplatsen har alltid varit en beskrivning av den verklighet som människor inom IT-branschen upplever, så jag skulle vilja titta på min erfarenhet av att arbeta i utvecklingsteam. Följande är allmänna erfarenheter som jag har fått från olika företag. Ingen erfarenhet är förknippad med ett visst företag och är inte nödvändigtvis en kritik.

Företag tenderar att inte vilja ha hårt arbetande och proaktiva personer.
----------------------------------------------

Har du massor av idéer? Vill du förnya dig? Tycker du om att hitta eleganta lösningar på komplexa problem som ditt team löser och som plågar halva företaget? Inser du vikten av säkerhet, programvarudesign och att hitta flaskhalsar i projekt?

Du kommer förmodligen att vara olycklig i utvecklingsteamet under lång tid.

Lagarbete är något som jag har kämpat mycket med på sistone - jag menar, jag simmar i det och försöker förstå de helt ointuitiva principer som jag ska följa.

- Teamarbete innebär att utveckla en lösning som hela teamet förstår. Det är ofta inte det bästa. Det är nästan aldrig elegant. Det är faktiskt alltid ineffektivt. Men den har den stora fördelen att hela teamet förstår den och att den kan hanteras på lång sikt. Detta är en mycket viktig idé. När det gäller stora projekt finns det inte så mycket press på att vara effektiv eftersom man som företag kan skala med hjälp av personal och har tillräckligt med pengar. Det är mycket viktigare att leverera saker i tid, även om de delvis är trasiga och har tidigare okända begränsningar, men i tid. Det har att göra med tanken att den lösning du levererar i morgon (även om den är bristfällig) har mycket större värde för kunden än en lösning som är 100 % felsökt och som inte kommer att finnas kvar på ett år.
- Det är snarare fel att förnya sig och att driva på saker och ting. Det har att göra med att det finns riktiga människor som arbetar i företag, som har familj och som bara vill arbeta under arbetstid. Det är en nödvändighet att människor har begränsad tid för att lära sig och prova nya saker. I princip måste företagen på arbetstiden få in utbildning och utveckling som kan användas för framtida arbete. En intressant konsekvens av detta är att beslut och inlärning av nya saker skjuts upp till den tid som behövs. För min del förstår jag detta tillvägagångssätt och det är mycket logiskt för mig. Om jag hade anställda skulle jag också föredra lugna människor som kommer att stanna i företaget i till exempel 15 år framför människor som har en bra allmän överblick och vill gå vidare, men det kommer att ske på bekostnad av teamet. Det är sant att den kollektiva lösningen aldrig kommer att vara densamma som den individuella, men det betyder inte att den blir sämre.

Personer som jag är en potentiell källa till problem och konflikter. Det är häftigt att tänka på det på det sättet.

När du gör en kodgranskning ska du bara leta efter objektiva fel.
----------------------------------------

Varje företag har sina regler för kodstil. Tyvärr. Det irriterade mig så mycket i början.

När du använder ett av de mer utvecklade språken, som .NET, tenderar kodstilreglerna att bli mer lika. Det är först då du upptäcker att det fortfarande finns PHP och JavaScript i världen. Särskilt PHP-grejen är lite frustrerande ibland. PHP är ett mycket moget språk med många fantastiska funktioner, men enligt min erfarenhet används det i företag med ungefär en tredjedel av sin fulla potential. Orsakerna varierar, men oftast är det tröghet.

I detta avseende har jag länge letat efter en procedurlösning för kodgranskning. Jag har lekt med PhpStan länge och följt idéerna kring det. Den grundläggande idén bakom PhpStan är att den endast visar på objektiva fel som med säkerhet kommer att inträffa någon gång. Ju mer jag utforskar PhpStan och tar det till nästa nivå, desto mer kan jag internt bekräfta hur sant det är.

Det vore trevligt om PhpStan skulle införas i åtminstone hälften av de tjeckiska företagen. Det skulle vara ännu bättre om det var minst nivå 6. I praktiken har jag sett att de bästa företagen har nivå 4 eller 5.

Häromdagen undrade jag om det kan finnas en riktig produktionsapplikation som har ett eget utvecklingsteam och som inte ens klarar nivå 0 - det är den nivå som kontrollerar de saker som du förväntar dig i en applikation. Något i stil med att se till att alla klasser och funktioner finns, att filerna inte har parsefel och så vidare. Och ja, det finns sådana tillämpningar. Men situationen förbättras på lång sikt. Förhoppningsvis.

Jag följer ett liknande tillvägagångssätt med codereview. Jag rapporterar bara om sådant som objektivt sett alltid är fel. Jag stöter ofta på felbedömningar av graden - något är fel, men det är inte ett så stort problem att det måste åtgärdas. Många problem löses genom konventioner eller genom att förklara att risken är liten och att det därför inte är någon idé att behandla den.

Jag har alltid ansett att saknade datatyper (även sammansatta sådana) är ett kritiskt fel. Då förstod jag att det finns test och dump.

Jag tror inte att jag har någon konkret slutsats om den här delen. Jag har bara en massa subjektiva kommentarer om den, som snarare kommer att leda till ömsesidiga missförstånd, så jag kommer inte att publicera dem. På lång sikt är jag benägen att dra slutsatsen att jag inte kan göra kodgranskning - antingen är jag för sträng eller för välvillig. Jag kan inte säga på en allmän nivå vad som verkligen är viktigt och vad som inte är så viktigt. Jag skulle vara intresserad av att veta hur andra gör det. Ingen har heller kunnat ge mig ett svar som jag kan använda.

Det finns inga pengar i anpassad utveckling.
---------------------------------

Har du en byrå och gör skräddarsydd webbutveckling? Om du inte är tillräckligt stor och gör stora projekt för andra företag kommer du förmodligen inte att existera en dag.

Anledningen till detta är att det inte finns tillräckligt med finansiering för anpassade projekt. Det har förmodligen att göra med avtalens karaktär, som är mer lönsamma för köparna. I själva verket är de flesta byråer brutalt underfinansierade och ger endast ägarna en verklig vinst.

När du internt utvecklar ett projekt som ett företag för dig själv spelar en försening av leveransen med en månad förmodligen inte så stor roll. Om du som byrå inte levererar ett jobb inom en månad är det garanterat att du sover långsamt på jobbet den månaden, att du ständigt förstör din hälsa, att kunden svär i telefonen och böter, att du alltid frågar om det kan gå snabbare, och som belöning får du fortfarande betala ett avtalsvite. Om du inte gör det kommer kunden att betrakta dig som en opålitlig entreprenör och kanske överväga att gå någon annanstans, där det ändå inte blir bättre.

Men detta är spelreglerna, som jag har lärt känna i praktiken och som har gjort hål i min budget.

Entreprenadverksamhet är förmodligen den mest komplexa affärsformen inom IT. Du vill inte göra entreprenader. Att ingå avtal kommer att förstöra dig. De ringer dig på helgen, på natten, på julen. Hälften av det arbete du gör får du inte betalt. Du kommer att be om ursäkt för misstag som du inte kan göra. Du kommer att titta på Instagram och se hur dina vänner berättar om sin tredje semester i år medan du sitter på kontoret klockan tre på lördagen och avslutar ett kundprojekt som du kommer att bli utskälld för på måndag eftersom kontraktet innehöll mer än vad du kunde få gjort. Folk tar semester och sjukdagar och du arbetar i deras ställe. Annars blir du uppsagd. Och då är du glad att du kan betala hyran.

Men å andra sidan lär man sig en hel del. Att vara under press sätter stor press på dig att lära dig och vara effektiv så att du nästa gång kan få lite mer gjort på samma tid. Det dåliga är att du inte riktigt kan tillämpa den kunskapen och erfarenheten någon annanstans eftersom företagen helt enkelt inte är intresserade (som jag förklarade ovan).

Lyckligtvis är detta en berättelse från 2019 och det är långt borta.

Det kommer aldrig att finnas en idealisk värld
-------------------------

Om jag gör det jag tycker om? Gör du det? :D

Jag antar att allt är en fråga om proportioner. Du kommer förmodligen aldrig att göra ett arbete som är 100 procent tillfredsställande. Du kommer förmodligen alltid att ha någon som förklarar för dig att du inte kan göra det som du har lärt dig med kraft i tio år och som du internt vet att du kan göra.

Å andra sidan får du utrymme att ha något mer om du förnekar din egen personlighet. Till exempel helgtid, bättre hälsa, mer tid för dig själv, en stabil miljö osv. Att det inte kan upprätthållas i längden är också uppenbart.

Jag gillar att kunna prova olika saker för att uppleva hur andra har det. Att arbeta i ett utvecklingsteam är en enorm tråkig sak jämfört med ett skräddarsytt jobb.

Det handlar inte längre om att hitta den bästa och snabbaste lösningen, utan om samarbete. Det handlar om att förbättra kommunikation, känslor och framför allt att lära sig att vara människa. Och det är också ett stort värde.
