HTTP-statuskoder
================

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	sv: http-statuskoder
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

Vid HTTP-kommunikation överförs så kallade "state codes", vilket är information om hur överföringen genomfördes. Du vet säkert att en `200`-kod betyder framgång och en `404`-kod betyder att sidan inte finns.

Statuskoderna är indelade i flera grupper enligt deras prefix.

1xx Information
--------------

| Kod | Betydelse |
|-------|--------|
| `100` | Fortsätt |
| `101` | Växelprotokoll |

2xx Framgång
----------

| Kod | Betydelse |
|-------|--------|
| `200` | OK (allt är bra) |
| `201` | Skapad |
| `202` | Accepterad |
| `203` | Obehörig information |
| `204` | Inget innehåll |
| `205` | Återställ innehåll |
| `206` | Delvis innehåll |

3xx Omdirigering
----------------

| Kod | Betydelse |
|-------|--------|
| `300` | Flera valmöjligheter |
| `301` | Permanent omdirigering |
| `302` | Hittade |
| `303` | Se mer |
| `304` | Ingen förändring |
| `305` | Använd proxy |
| `306` | **Old men reserverad för framtida användning** |
| `307` | Temporär omdirigering |

4xx Fel i klienten (användaren)
-----------------------------

| Kod | Betydelse |
|-------|--------|
| `400` | Dålig förfrågan |
| `401` | Obehörig anslutning |
| `402` | Betalning begärd |
| `403` | Inaktiverad |
| `404` | Ej funnen |
| `405` | Metod inte tillåten |
| `406` | Oacceptabelt |
| `407` | Proxy-autentisering krävs |
| `408` | Förfrågan har gått ut i tid |
| `409` | Nätverkskonflikt |
| `410` | Uppgifterna är borta |
| `411` | Den begärda längden matchar inte |
| `412` | Antagandet misslyckades |
| `413` | Enhetsförfrågan är för stor |
| `414` | Request-URI är för lång |
| `415` | Medietyp som inte stöds |
| `416` | Begärd omfattning inte tillfredsställande |
| `417` | Förväntan misslyckades |

5xx Serverfel
--------------

| Kod | Betydelse |
|-------|--------|
| `500` | Internt serverfel |
| `501` | Inte implementerat |
| `502` | Dålig gateway |
| `503` | Tjänsten är otillgänglig |
| `504` | Timeout för gatewayen har gått ut |
| `505` | HTTP-versionen stöds inte |
| `509` | Bandbreddsgränsen överskrids |
