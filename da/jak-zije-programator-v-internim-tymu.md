Hvordan en programmør lever i et internt udviklingsteam
=======================================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	da: hvordan-en-programmor-lever-i-et-internt-udviklingsteam
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

Der er en høj pris at betale for ærlighed.

Dette websted har altid været en beskrivelse af den virkelighed, som folk inden for IT oplever, så jeg vil gerne se på min erfaring med at arbejde på udviklingsteams. Følgende er generelle erfaringer, som jeg har haft på tværs af virksomheder. Ingen erfaring er forbundet med en bestemt virksomhed og er ikke nødvendigvis en kritik.

Virksomhederne har en tendens til ikke at ville have hårdtarbejdende og proaktive mennesker
----------------------------------------------

Har du masser af idéer? Ønsker du at innovere? Kan du lide at finde elegante løsninger på de komplekse problemer, som dit team løser, og som plager halvdelen af virksomheden? Er du klar over vigtigheden af sikkerhed, softwaredesign og at finde flaskehalse i projekter?

Du vil sandsynligvis være ulykkelig på udviklingsholdet i lang tid.

Teamwork er noget, som jeg har kæmpet meget med på det seneste - jeg mener, jeg svømmer specifikt i det og forsøger at forstå de fuldstændig uintuitive principper, som jeg skal overholde.

- Teamwork betyder at udvikle en løsning, som hele teamet forstår. Det er ofte ikke det bedste. Det er næsten aldrig elegant. Faktisk er det altid ineffektivt. Men det har den fede fordel, at hele holdet forstår det, og at det kan styres på lang sigt. Det er en meget vigtig idé. For med store projekter er der ikke så stort et pres for at være effektiv, fordi man som virksomhed kan skalere gennem folk, og man har penge nok. Det er meget vigtigere at levere tingene til tiden, selv om de er delvist ødelagte og med hidtil ukendte begrænsninger, men til tiden. Det har at gøre med tanken om, at den løsning, du leverer i morgen (selv om den er ufuldkommen), har langt større værdi for kunden end en 100 % fejlfinding, der ikke vil være tilgængelig om et år.
- At innovere og skubbe tingene ud er ret forkert. Det har at gøre med, at der er rigtige mennesker, der arbejder i virksomheder, som har familie og kun ønsker at arbejde i arbejdstiden. Det følger nødvendigvis heraf, at folk har begrænset tid til at lære og prøve nye ting. Virksomhederne skal i det væsentlige presse uddannelse og udvikling ind i folks arbejdstid, som kan bruges til fremtidigt arbejde. En interessant konsekvens heraf er, at beslutninger og indlæring af nye ting udskydes til den nødvendige tid. For mit vedkommende forstår jeg denne fremgangsmåde, og den giver meget god mening for mig. Hvis jeg havde medarbejdere, ville jeg også foretrække rolige mennesker, der vil holde i virksomheden i f.eks. 15 år, frem for folk, der har et godt overblik og gerne vil videre, men det vil ske på bekostning af holdet. Det er rigtigt, at den kollektive løsning aldrig vil være den samme som den individuelle løsning, men det betyder ikke, at den vil være værre.

Folk som mig er en potentiel kilde til problemer og konflikter. Det er fedt at tænke på det på den måde.

Når du foretager codereview, skal du blot kigge efter objektive fejl
----------------------------------------

Alle virksomheder har deres egne regler for kodeks. Desværre. Det irriterede mig så meget i starten.

Når du bruger et af de mere modne sprog, som f.eks. .NET, er reglerne for kodningsstil mere ens. Først da finder du ud af, at der stadig findes PHP og JavaScript i verden. Især det med PHP er en smule frustrerende til tider. PHP er et meget modent sprog med mange gode funktioner, men min erfaring er, at det bruges i virksomheder med omkring en tredjedel af dets fulde potentiale. Årsagerne er forskellige, men oftest er der tale om inerti.

I den forbindelse har jeg længe ledt efter en proceduremæssig løsning til kodegennemgang. Jeg har leget med PhpStan i lang tid og har fulgt ideerne omkring det. Den grundlæggende idé bag PhpStan er, at den kun fremhæver objektive fejl, som med sikkerhed vil opstå på et tidspunkt. Jo mere jeg udforsker PhpStan og tager det til det næste niveau, jo mere kan jeg internt bekræfte, hvor sandt det er.

Det ville være rart, hvis PhpStan blev implementeret i mindst halvdelen af de tjekkiske virksomheder. Det ville være endnu bedre, hvis det i det mindste var på niveau 6. I praksis har jeg set de bedste virksomheder have niveau 4 eller 5.

Forleden dag tænkte jeg på, om der kan være en rigtig produktionsapplikation, der kører, og som har sit eget udviklingsteam, og som ikke engang klarer niveau 0 - det er det niveau, der styrer de ting, som du forventer i enhver applikation. Noget i retning af at sikre, at alle klasser og funktioner findes, at filer ikke har parsefejl osv. Og ja, der findes sådanne applikationer. Men situationen er ved at blive bedre på lang sigt. Forhåbentlig.

Så jeg følger en lignende fremgangsmåde med codereview. Jeg rapporterer kun om ting, der objektivt set altid er forkerte. Jeg støder ofte på fejlvurderinger af graden - der er noget galt, men igen er det ikke så stort et problem, at der skal gøres noget ved det. Mange problemer løses ved konventioner eller ved at erklære, at risikoen er lille, og at der derfor ikke er nogen grund til at behandle den.

Jeg har altid betragtet manglende datatyper (selv sammensatte typer) som en kritisk fejl. Så forstod jeg, at der er test og dump.

Jeg tror ikke, at jeg har en konkret konklusion på denne del. Jeg har bare en masse subjektive kommentarer til det, som sandsynligvis vil give anledning til misforståelser, så jeg vil ikke skrive dem. På lang sigt er jeg tilbøjelig til at konkludere, at jeg ikke kan lave codereview - enten er jeg for streng eller for velvillig. Jeg kan ikke på et generelt niveau sige, hvad der virkelig betyder noget, og hvad der ikke er så vigtigt. Jeg ville være meget interesseret i at vide, hvordan andre mennesker gør det. Ingen har endnu kunnet give mig et svar, som jeg også kan bruge.

Der er ingen penge i brugerdefineret udvikling.
---------------------------------

Har du et bureau og laver brugerdefineret webudvikling? Hvis du ikke er stor nok og laver store projekter for andre virksomheder, vil du sandsynligvis ikke eksistere en dag.

Grunden til, at dette sker, er den ringe finansiering af brugerdefinerede projekter. Det har sandsynligvis noget at gøre med kontrakternes art, som er mere rentable for køberne. Faktisk er de fleste agenturer brutalt underfinansierede og giver kun ejerne et reelt overskud.

Når du internt udvikler et projekt som en virksomhed for dig selv, betyder en forsinkelse af leveringen med en måned sandsynligvis ikke så meget. Hvis du som bureau ikke leverer et job inden en måned, er du garanteret at sove langsomt på arbejdet i den pågældende måned, at du konstant ødelægger dit helbred, at kunden bander i telefonen og bøder, at du altid spørger, om det kan gå hurtigere, og som belønning betaler du stadig en kontraktmæssig bod. Og hvis du ikke gør det, vil kunden betegne dig som en upålidelig entreprenør og måske overveje at gå et andet sted hen, hvor det alligevel ikke bliver bedre.

Men det er de spilleregler, som jeg har lært at kende i praksis, og som har givet mig et hul i budgettet.

Kontraktering er nok den mest komplekse form for forretning inden for IT. Du ønsker ikke at indgå kontrakter. Kontraktering vil ødelægge dig. De ringer til dig i weekenden, om natten og til jul. Halvdelen af det arbejde, du udfører, bliver du ikke betalt. Du vil undskylde for fejl, du ikke kan begå. Du vil kigge på Instagram på storytelling af venner, der er taget på deres tredje ferie i år, mens du sidder på dit kontor kl. 3 om natten om lørdagen og afslutter et kundeprojekt, som du alligevel vil få skældud for mandag, fordi kontrakten indeholdt mere, end du kunne nå at få gjort. Folk vil tage ferie og sygedage, og du vil arbejde i deres sted. Eller du bliver opsagt. Og så vil du være glad for at betale huslejen.

Men så lærer man også en masse. Fordi man er under pres og har et stort pres for at lære og være effektiv, så man næste gang kan få lidt mere gjort på den samme tid. Det dårlige er, at du ikke rigtig kan anvende denne viden og erfaring andre steder, fordi virksomhederne ikke er interesserede (som jeg forklarede ovenfor).

Heldigvis er det en 2019-historie, og den er langt væk.

Der vil aldrig findes en ideel verden
-------------------------

Hvis jeg gør det, jeg kan lide? Gør du det? :D

Det hele er vel et spørgsmål om proportioner. Du vil sandsynligvis aldrig få et arbejde, der er 100 % tilfredsstillende. Du vil sandsynligvis altid få nogen til at forklare dig, at du ikke kan gøre det, du har lært med magt i 10 år, og du ved internt, at du kan.

På den anden side får du plads til at have noget mere, hvis du fornægter din egen personlighed i et vist omfang. Som weekendtid, bedre helbred, mere tid til dig selv, et stabilt miljø osv. Det er også indlysende, at det ikke kan opretholdes i længden.

Jeg kan godt lide at kunne prøve forskellige ting for at opleve på egen hånd, hvordan andre har det. At arbejde på et udviklingsteam er enormt kedeligt sammenlignet med et specialarbejde.

Det handler ikke længere om at finde den bedste og hurtigste løsning, det handler om samarbejde. Det handler om at forbedre kommunikationen, følelserne og vigtigst af alt om at lære at være menneske. Og det er også en stor værdi.
