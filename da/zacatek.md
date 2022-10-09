PHP onlinekursus for begyndere
==============================

> id: '10ecc1cc-8d49-4882-8ecf-114ad74902df'
> slug:
> 	cs: zacatek
> 	da: php-onlinekursus-for-begyndere
> 
> perex:
> 	- 'Výuka PHP online, kurz pro začátečníky. Naučíte se programovat v PHP přímo od senior vývojáře.'
> 	- 'Online PHP tutorial, kursus for begyndere. Lær at programmere i PHP direkte fra en erfaren udvikler.'
> 
> publicationDate: '2020-02-09 14:27:47'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: f4f8a8d74e09eb298963f7ab8ff00959

PHP er et scriptingsprog på serversiden, der er udviklet til moderne webapplikationer.

PHP-sproget har en meget hurtig indlæringskurve, dvs. på meget kort tid (i størrelsesordenen uger) vil du være i stand til at forstå de fleste af principperne i sproget, så du vil være i stand til at oprette næsten enhver simpel webapplikation med formularer, brugerkonti, database og meget mere.

En anden fordel ved PHP er dets massive udbredelse på næsten alle servere (til hosting) og den konstante udvikling, hvilket gør dig sikker på, at dit program/web vil kunne køre overalt.

Hvordan kommer man i gang?!?
------------

Sørg for, at du har følgende ting på plads, inden du går i gang:

- Hjerne, det handler meget om at tænke,
- En computer (eller server), hvor du kan køre dine scripts,
- Det er nyttigt at have kendskab til matematik eller et andet teknisk område,
- Passende studiematerialer (som f.eks. dette websted og <a href="https://www.php.net">den officielle vejledning</a>),
- Grundlæggende kendskab til HTML og CSS,
- Det er nyttigt at have et grundlæggende kendskab til engelsk (de fleste materialer er kun på engelsk, f.eks. den officielle manual og webfora),
- Kendskab til et andet programmeringssprog er en fordel (meget lig C/C++, som PHP er baseret på),
- Jeg anbefaler stærkt et grundlæggende kendskab til HTML og CSS, da det er meget vanskeligt at forstå PHP uden dette.
- En grundlæggende softwarebaggrund (varierer fra system til system, og de bedste programmer **er ikke gratis**).

Grundlæggende software
-----------------

`Windows-computer:`
- Enhver moderne **webbrowser**, der tilbyder fejlfindingstilstand. Personligt bruger jeg <a href="https://www.google.com/chrome">Google Chrome</a>.
- Til at starte med er det tilstrækkeligt med en bedre **teksteditor** med syntaksmarkering. Verdens bedste er nok <a href="https://www.sublimetext.com">Sublime Text</a> (som tilbyder avanceret arbejde med enhver tekst i mange formater, arbejde med flere markører, regulære udtryk og generelt er et værktøj til flere formål end blot programmering). Tidligere brugte jeg den tjekkiske editor <a href="https://www.pspad.com/cz/">PSpad</a> (som jeg i øjeblikket anser for at være meget forældet og utilstrækkelig til moderne websteder), nogle bruger også <a href="https://www.slunecnice.cz/sw/notepad/">Notepad++</a>.
- Hvis du er seriøs med udvikling, vil jeg hellere bruge det fulde udviklingsmiljø. På arbejdet bruger jeg <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, som jeg betragter som den bedste editor til at skrive kode, der nogensinde er blevet kodet.
- En **webserver**, der kan håndtere PHP, MySql-database og giver dig mulighed for at konfigurere dine indstillinger. I øjeblikket anser jeg <a href="https://www.apachefriends.org/download.html">Xampp</a>, som er en færdigpakket pakke, for at være det bedste valg til Windows.

`Linux (især webserver):`
- En hvilken som helst browser, f.eks. <a href="https://www.google.com/chrome">Google Chrome</a> eller Firefox.
- I Ubuntu bruger jeg <a href="https://www.sublimetext.com">Sublime Text</a>, begge er tilstrækkelige til at komme i gang.
- Det er en større udfordring at installere en **webserver** end at installere Windows. I Ubuntu er der f.eks. et <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">Tasksel</a> program til dette formål, som styres af Terminal.
- Hvis du skal installere en Linux-server, er det også værd at overveje <a href="https://www.nginx.com/resources/wiki/">Ngnix</a>.

`Mac:`
- Mac'en er fantastisk at programmere på, den er tilpasset brugeren.
- Til udvikling på en MacBook Pro bruger jeg <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, som jeg finder er det bedste udviklingsmiljø, og til redigering af almindelige tekstfiler bruger jeg <a href="https://www.sublimetext.com">Sublime Text</a>, som håndterer store filer meget godt.
- Jeg installerede selv serveren via **Terminal**, hvilket kan være en udfordring for begyndere, men der findes et værktøj kaldet **Mamp**, som lader dig klikke på alle ting med musen.

`Senior Recommendations:`

Fra 2020 er det begyndt at blive tydeligt, at alle problemerne med at køre PHP og hele applikationer nemt kan løses ved hjælp af Docker-containere. Hvis du lærer at arbejde med Docker, kan du spare hundredvis af timer i fremtiden og nemt integrere nybegyndere i et eksisterende projekt.

Dele af serien
------------

For at få en komplet grundbog i PHP har jeg skrevet flere artikler, der hjælper dig med at overvinde begynderbarrieren og lære det grundlæggende i PHP:

- <a href="/introduktion">Introduktion til at lære PHP</a>
- <a href="/first-script">Første script</a>
- <a href="/principper-for-variabel-skrivning</a>
- <a href="/cykler">Cykler</a>
- <a href="/hvor man finder ud af">Sådan finder du rundt i koden</a>
- <a href="/methods-sending-data">Metoder til dataafsendelse</a>
- <a href="/include-file">Include (sammensætning af sider fra chunks)</a>
- <a href="/betingelser">Betingelser og forgrening</a>
- <a href="/safe-application">safe-app</a>

Senere er webudvikling dog allerede ret kompliceret, og man har virkelig brug for en masse viden (eller i det mindste en mistanke om, at der findes noget sådant). Da hele konceptet omkring sproget og webudvikling er ret komplekst, har jeg i det mindste udarbejdet <a href="/viden">en grundlæggende oversigt over viden</a>, som jeg gradvist supplerer og skriver artikler om.

Til udvikling af komplekse applikationer anbefaler jeg at starte med <a href="/oop">objektorienteret programmering</a>.

Licens
-------

Jeg stiller disse materialer gratis til rådighed via hjemmesiden `php.baraja.cz`, så de må ikke bruges i andre betalte kurser. Teksterne kan indeholde fejl og unøjagtigheder. Dette er ikke en officiel oversættelse af manualen.

Jeg forbeholder mig alle rettigheder til teksterne (virkelig), og derfor er kopiering **forbudt**. Du må bruge URL'en til dette websted (linket her) og kildekoden uden yderligere begrænsninger.

Kontakt
-------

Jeg er glad for at snakke med dig om webudvikling, jeg er glad for at give dig generelle råd, men **mere komplekst arbejde betragtes som et betalt job**.

- E-mail: jan@barasek.com
- Personlig <a href="https://www.facebook.com/janbarasek">Facebook</a>

<a href="https://baraja.cz/kontakt">Alle kontakter</a>
