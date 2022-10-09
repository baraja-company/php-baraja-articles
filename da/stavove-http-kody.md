HTTP-statuskoder
================

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	da: http-statuskoder
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

I HTTP-kommunikation overføres såkaldte "tilstandskoder", som er oplysninger om, hvordan overførslen blev foretaget. Jeg er sikker på, at du ved, at en `200`-kode betyder succes, og en `404`-kode betyder en ikke-eksisterende side.

Statuskoder er opdelt i flere grupper efter deres præfiks.

1xx Informationel
--------------

| Kode | Betydning |
|-------|--------|
| `100` | Fortsæt |
| `101` | Switch Protocol |

2xx Succes
----------

| Kode | Betydning |
|-------|--------|
| `200` | OK (alt i orden) |
| `201` | Oprettet |
| `202` | Accepteret |
| `203` | Uautoriserede oplysninger |
| `204` | Intet indhold |
| `205` | Gendan indhold |
| `206` | Delvist indhold |

3xx-omdirigering
----------------

| Kode | Betydning |
|-------|--------|
| `300` | Multiple Choice |
| `301` | Permanent omdirigering |
| `302` | Fundet |
| `303` | Se mere |
| `304` | Ingen ændring |
| `305` | Brug proxy |
| `306` | **Gammel, men reserveret til fremtidig brug** |
| `307` | Midlertidig omdirigering |

4xx Client (bruger) fejl
-----------------------------

| Kode | Betydning |
|-------|--------|
| `400` | Dårlig anmodning |
| `401` | Uautoriseret forbindelse |
| `402` | Betaling ønskes |
| `403` | Deaktiveret |
| `404` | Ikke fundet |
| `405` | Metode ikke tilladt |
| `406` | Uacceptabelt |
| `407` | Proxy-godkendelse påkrævet |
| `408` | Anmodning er gået ud i tid |
| `409` | Netværkskonflikt |
| `410` | Data væk |
| `411` | Den ønskede længde svarer ikke til |
| `412` | Antagelse mislykkedes |
| `413` | Enhedsanmodning er for stor |
| `414` | Request-URI er for lang |
| `415` | Ikke understøttet medietype |
| `416` | Anmodet omfang ikke tilfredsstillende |
| `417` | Forventning mislykkedes |

5xx Serverfejl
--------------

| Kode | Betydning |
|-------|--------|
| `500` | Intern serverfejl |
| `501` | Ikke implementeret |
| `502` | Dårlig gateway |
| `503` | Tjenesten er ikke tilgængelig |
| `504` | Gateway timeout udløbet |
| `505` | HTTP-version understøttes ikke |
| `509` | Grænse for båndbredde overskredet |
