Sikker anvendelse
=================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 	da: sikker-anvendelse
> 
> publicationDate: '2019-10-01 14:19:04'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '74e52382b995b303550d8e4b783bbb84'

Hvis du mener det alvorligt med at udvikle webapplikationer, og hvis webstedet senere skal være tilgængeligt på internettet, er det meget vigtigt at tage hensyn til sikkerheden.

Realistisk set venter følgende trusler på udviklerne:

- **Applikationen har en intern fejl**, f.eks. fordi programmøren har lavet en fejl, som han eller hun ikke bemærkede, da han eller hun skrev koden, eller fordi den kun viser sig "nogle gange".
- **Serveren har en fejlkonfiguration** eller miljøet **har ændret sig**, fordi serveradministratoren har ændret serveradfærd, og webstederne ikke var tilpasset til det. Alternativt kan det være, at vi implementerer til en ny maskine og ikke kender den nøjagtige konfiguration.
- Nogen forsøger at angribe**, enten en ekstern angriber eller en tidligere ansat indefra systemet.

Alle situationer er yderst ubehagelige og kræver øjeblikkelig handling. Det er ikke teknisk muligt at designe en applikation, så den aldrig fejler.

Under udviklingen er det meget vigtigt at teste koden, når den er blevet skrevet, og hvis der opstår en fejl på serveren, er det vigtigt at informere udvikleren straks med en præcis beskrivelse af problemet.

For at opdage fejl og overvåge serverens tilstand anbefaler jeg værktøjet <a href="https://tracy.nette.org/">Tracy</a>, som logger alle fatale fejl, ubehandlede undtagelser m.m. til filer direkte på serverens disk. Du kan desuden indstille en e-mail, hvor meddelelser vil blive sendt.

Er du interesseret i en sikkerhedsrådgivning om din applikation?

Kontakt mig.
