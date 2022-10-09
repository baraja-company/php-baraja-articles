Sådan optimerer du effektivt sidens indlæsningshastighed
========================================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	da: sadan-optimerer-du-effektivt-sidens-indlaesningshastighed
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

Når jeg rejser, støder jeg ofte på dårlige internetforbindelser, hvilket som webdesigner får mig til at tænke over designprincipper til at løse webhastigheden selv på en dårlig forbindelse.

Jeg har fundet frem til et par nyttige tricks fra praksis:

Vigtige landingssider er ofte enkle, og det kan betale sig at bruge brugerdefineret CSS-styling
-----------------------------------------------------------------------------------

F.eks. er login-siden til CMS ofte meget enkel. Er det virkelig nødvendigt at downloade hele Bootstrap og andre CSS/JS-frameworks til en simpel formular? Vigtige sider er værd at optimere og skrive native kode.

Når siden er fuldt renderet, får vi et par sekunder, hvor brugeren udfylder sine oplysninger, og vi kan downloade og cache de resterende stilarter i browseren i baggrunden. Når brugeren logger ind, har han/hun sandsynligvis allerede downloadet Bootstrap (og hvis han/hun bruger Edge, har han/hun ikke...).

I stedet for ikoner kan vi ofte bruge emoji
-----------------------------------

Virkelig! Emoji har mange praktiske fordele:

- Det fylder kun 4 bytes, så det overtrumfer ethvert billede
- Det mislykkes aldrig at downloade, så brugeren ser altid i det mindste et ikon i stedet for et tomt felt.
- Viser i brugerens visuelle stil
- Har i øjeblikket allerede en bred understøttelse af enheden
- Sparer serveranmodningen, og vi behøver ikke at beskæftige os med hvor vi skal hente billedet fra (dette kan optimeres via CDN, men hvorfor, ikke sandt)
- Der er mange ikoner, som du sandsynligvis ikke ønsker at beskæftige dig med. Hvor kan du f.eks. finde flagikoner for alle de lande, du ønsker at vise i administrationen? Brug emoji: https://github.com/bara.../country/blob/main/flag-emoji.json

Brug kritiske stilarter direkte i HTML'en, når webstedet indlæses
------------------------------------------------------

Jeg sætter nogle gange de vigtige CSS-stilarter, der definerer sidens layout og grundlæggende layout, direkte ind i HTML'en. Jeg sætter en grænse på 1 KB, som jeg forsøger at få så meget som muligt ind i som muligt. Ulempen ved denne fremgangsmåde er, at stilarter skal overføres ved hver forespørgsel og ikke kan lagres i cachen, men på den anden side downloader de uforligneligt meget hurtigere end et billede.

Jeg er ikke ekspert i indlæsningshastighed, det kan [Martin Michalek](https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) fortælle dig bedre.

Brug ajax!
--------------

Jeg bruger altid ajax til at hente uvæsentligt eller langsomt indhold. Det øger de teknologiske krav til applikationen en smule, men jeg har råd til det.

Et eksempel på et godt sted at bruge ajax er næsten alt i administrationen. Meget ofte er der en masse data, der skal hentes, men brugeren er ikke interesseret i alt. Når brugeren kun har en tynd javascript-klient downloadet, og alle dataene hentes via Vue og json, downloades kun den mindste mængde data, og svarene er øjeblikkelige.

Sådan gør du det i Vue: https://vue.baraja.cz/api-a-ajax-ve-vue-js

På backend bruger jeg biblioteket til Nette: https://github.com/baraja-core/structured-api

Brug et passende CDN
---------------------

Til distribution af statisk indhold anbefaler jeg, at du bruger et CDN til alle typer websteder. Hvis du ikke ved, hvordan du konfigurerer et CDN, kan du i det mindste bruge Cloudflare i proxy-tilstand. Den cacher automatisk statisk indhold til sig selv baseret på HTTP-headers. Det er ikke alle hostingudbydere, der har Pops godt opsat, og for lange ruter vil dette spare dig hundredvis af millisekunder i hver forespørgsel.

Egnet billedformat
---------------------

En af mine juniorer lagde for nylig et PNG-billede på webstedets forside, som indeholdt et foto og fyldte 3 MB. Han sagde, at det var i orden, fordi siden blev indlæst hurtigt på hans forbindelse (det gør den også på hans optiske forbindelse derhjemme, ikke sandt?), og han sagde, at vi overfører mange data i dag, f.eks. video.

Jeg er gammeldags og optimerer, hvad jeg kan.

Billeder til JPG, eller bedre WEBP. Men det betyder ikke noget at gemme et billede til JPG, du skal køre det gennem en optimeringstjeneste: https://tinyjpg.com

Hvis du har billeder i Git, kan dette værktøj automatisk komprimere dem og sende en pull request: https://imgbot.net. Når der tilføjes et nyt billede, sendes PR igen.

Hvis du har brug for at komprimere tusindvis af billeder (f.eks. et helt galleri med produkter på et websted), bruger jeg et desktop-program til Mac til at gøre det: https://imageoptim.com/mac

Ved at komprimere billederne korrekt kan du normalt spare mest data. Nogle gange endda 50 %.
