Säker tillämpning
=================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 	sv: saeker-tillaempning
> 
> publicationDate: '2019-10-01 14:19:04'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '74e52382b995b303550d8e4b783bbb84'

Om du menar allvar med att utveckla webbapplikationer och webbplatsen senare kommer att finnas tillgänglig på Internet, är det mycket viktigt att ta hänsyn till säkerheten.

Realistiskt sett väntar följande hot på utvecklarna:

- **Programmet har ett internt fel**, t.ex. för att programmeraren har gjort ett misstag som han eller hon inte märkte när han eller hon skrev koden, eller för att felet bara uppträder "ibland".
- **Servern har en felaktig konfiguration** eller miljön **har ändrats** eftersom serveradministratören har ändrat serverbeteendet och webbplatserna inte var anpassade för det. Alternativt kan vi distribuera till en ny maskin och inte veta den exakta konfigurationen.
- Någon försöker attackera**, antingen en extern angripare eller en tidigare anställd inom systemet.

Alla situationer är extremt obehagliga och kräver omedelbara åtgärder. Att utforma ett program så att det aldrig misslyckas är inte tekniskt möjligt.

Under utvecklingen är det mycket viktigt att testa koden när den har skrivits, och om ett fel uppstår på servern är det viktigt att informera utvecklaren omedelbart med en noggrann beskrivning av problemet.

För att upptäcka fel och övervaka serverns hälsa rekommenderar jag verktyget <a href="https://tracy.nette.org/">Tracy</a>, som loggar alla dödliga fel, obehandlade undantag med mera i filer direkt på serverns disk. Dessutom kan du ange en e-postadress till vilken meddelanden ska skickas.

Är du intresserad av en säkerhetskonsultation för din applikation?

Kontakta mig.
