Oversigt over viden om webudvikling
===================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	da: oversigt-over-viden-om-webudvikling
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

At vide, hvordan man opretter et websted og derefter tager sig grundigt af det, er ikke kun et spørgsmål om at lave det. Der er mange forhindringer på vejen, og det er godt at have i det mindste en grundlæggende idé om hver enkelt ting. Da jeg startede, vidste jeg ikke rigtig, hvad der var at lære. Denne side tjener som en vejviser gennem de emner, jeg måtte studere efterhånden for at kunne forstå webudvikling og håndtere de mest almindelige situationer.

Serveradministration
--------------

> En webserver er en computer, der kører internettet. Når en bruger ser en side, sender webserveren siden til brugeren.
>
> I øjeblikket (2021) giver det ikke længere mening at få gratis hosting, hvis man er seriøs med nettet. En server kan **lejes fra 50 kroner om måneden**.

- Serverinstallation (forskellig på Windows og Linux)
- Serverkonfiguration
	- Apache-indstillinger
	- Opsætning af PHP
	- <a href="/info">hentning af den aktuelle PHP-konfiguration</a> (funktion `phpinfo()`)
	- Ngnix-routing (forbedrer serverens ydeevne)

Internet og webbrowser
--------------------------------

- Webbrowsere
- Princippet om anmodning og svar
	- URL-adresse
	- Ajax og Ajaj
	- Generering af HTML-kode (templating-systemer)

Strenge
-----------------

- Læsning, skrivning og sammenkædning af strenge, især grundlæggende arbejde med strenge
- Behandling af strenge
	- Gennemse tegn for tegn
	- <a href="/if">sammenligning af strenge</a>
	- Lighed mellem strenge (funktionerne `levenshtein()`, `similar_text()` og `soundex()`)
	- <a href="/explode">Explode</a>, opdeling med separator
	- **Regulære udtryk** opdeler strenge i henhold til en universelt konfigurerbar maske
	- **Tokenizer** opdeler strenge i små dele (tokens)
- Serialisering af data til en streng
	- <a href="/json">Json</a>, et javascript-objekt gemt i en streng
