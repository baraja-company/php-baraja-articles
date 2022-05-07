Kódy stavu HTTP
===============

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	sk: kody-stavu-http
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

Pri komunikácii HTTP sa prenášajú tzv. stavové kódy, čo sú informácie o tom, ako sa prenos uskutočnil. Určite viete, že kód `200` znamená úspech a kód `404` znamená neexistujúcu stránku.

Stavové kódy sú rozdelené do niekoľkých skupín podľa ich prefixu.

1xx Informačné
--------------

| Kód | Význam |
|-------|--------|
| `100` | Pokračovať |
| `101` | Switch Protocol |

2xx Úspech
----------

| Kód | Význam |
|-------|--------|
| `200` | OK (všetko v poriadku) |
| `201` | Created |
| `202` | Prijaté |
| `203` | Neautorizované informácie |
| `204` | Žiadny obsah |
| `205` | Obnovenie obsahu |
| `206` | Čiastočný obsah |

Presmerovanie 3xx
----------------

| Kód | Význam |
|-------|--------|
| `300` | Viacnásobná voľba |
| `301` | Trvalé presmerovanie |
| `302` | Nájdené |
| `303` | Viac informácií
| `304` | Žiadna zmena |
| `305` | Použiť proxy server |
| `306` | **Staré, ale vyhradené pre budúce použitie** |
| `307` | Dočasné presmerovanie |

4xx Chyba klienta (používateľa)
-----------------------------

| Kód | Význam |
|-------|--------|
| `400` | Zlá požiadavka |
| `401` | Neautorizované pripojenie |
| `402` | Požadovaná platba |
| `403` | Zakázané |
| `404` | Nenájdené |
| `405` | Metóda nie je povolená |
| `406` | Neprípustné |
| `407` | Vyžaduje sa overenie proxy servera |
| `408` | Request timed out |
| `409` | Konflikt v sieti |
| `410` | Údaje zmizli |
| `411` | Požadovaná dĺžka sa nezhoduje |
| `412` | Predpoklad zlyhal |
| `413` | Požiadavka na entitu je príliš veľká |
| `414` | Request-URI je príliš dlhá |
| `415` | Nepodporovaný typ média |
| `416` | Požadovaný rozsah nie je vyhovujúci |
| `417` | Očakávanie zlyhalo |

5xx Chyba servera
--------------

| Kód | Význam |
|-------|--------|
| `500` | Interná chyba servera |
| `501` | Nie je implementované |
| `502` | Zlá brána |
| `503` | Service Unavailable (služba nie je dostupná)
| `504` | Časový limit brány vypršal |
| `505` | Verzia HTTP nie je podporovaná |
| `509` | Prekročenie limitu šírky pásma |
