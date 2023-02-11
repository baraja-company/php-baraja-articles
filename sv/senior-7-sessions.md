Hur man hanterar plötsliga krascher i PHP-skriptet
==================================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	sv: hur-man-hanterar-ploetsliga-krascher-i-php-skriptet
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

En historia från slutet av 2016, då jag bokstavligen räddades av en kollega: i en PHP-applikation väljer du att checka in bilder via ett proxyscript, som bland annat kan justera deras dimensioner och andra parametrar beroende på den inkommande begäran. Som en del av optimeringen sparar du också de genererade varianterna fysiskt på disk.

Men i produktionsdrift börjar du plötsligt se en enorm belastning och tusentals förfrågningar som står i kö. Bilderna läses in i tur och ordning en efter en för varje användare. Uppdatering av sidan och klick på länkar fungerar inte. Programmet verkar helt fruset. Det fungerar bara att vänta på att allt ska behandlas.

Vad kan vara problemet? Jag har listat tre viktiga ledtrådar i texten för att du snabbt ska kunna söka efter problemet. Hotfix har en trivial lösning.
