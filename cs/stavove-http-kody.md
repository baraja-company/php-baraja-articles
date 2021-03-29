Stavové HTTP kódy
=================

> id: "97800bb6-004c-4bdb-b126-f87e77cc5120"
> slugCS: stavove-http-kody
> publicationDate: "2021-03-29 22:17:03"
> mainCategoryId: "2a1ef8bc-14aa-438a-87e7-5b3f9643f325"

Při HTTP komunikaci se přenáší tzv. `stavové kódy`, což je informace o tom, jak přenos proběhl. Určitě víte, že kód `200` znamená úspěch a kód `404` neexistující stránku.

Stavové kódy se dělí do několika skupin podle jejich prefixu.

1xx Informační
--------------

| Kód   | Význam |
|-------|--------|
| `100` | Pokračovat |
| `101` | Přepnout protokol |

2xx Úspěch
----------

| Kód   | Význam |
|-------|--------|
| `200` | OK (všechno v pořádku) |
| `201` | Vytvořeno |
| `202` | Přijato |
| `203` | Neoprávněné informace |
| `204` | Žádný obsah |
| `205` | Obnovit obsah |
| `206` | Částečný obsah |

3xx Přesměrování
----------------

| Kód   | Význam |
|-------|--------|
| `300` | Více možností |
| `301` | Trvalé přesměrování |
| `302` | Nalezeno |
| `303` | Viz další |
| `304` | Beze změny |
| `305` | Použijte proxy |
| `306` | **zastaralé, ale rezervováno pro budoucí použití** |
| `307` | Dočasné přesměrování |

4xx Chyba klienta (uživatele)
-----------------------------

| Kód   | Význam |
|-------|--------|
| `400` | Špatný požadavek |
| `401` | Neoprávněné připojení |
| `402` | Požadovaná platba |
| `403` | Zakázáno |
| `404` | Nenalezeno |
| `405` | Metoda není povolena |
| `406` | Nepřijatelné |
| `407` | Vyžaduje se ověření proxy |
| `408` | Časový limit požadavku vypršel |
| `409` | Konflikt na síti |
| `410` | Data jsou pryč |
| `411` | Požadovaná délka neodpovídá |
| `412` | Předpoklad se nezdařil |
| `413` | Žádost o entitu je příliš velká |
| `414` | Žádost-URI je příliš dlouhý |
| `415` | Nepodporovaný typ média |
| `416` | Požadovaný rozsah není uspokojivý |
| `417` | Očekávání se nezdařilo |

5xx Chyba serveru
--------------

| Kód   | Význam |
|-------|--------|
| `500` | Interní chyba serveru |
| `501` | Není implementováno |
| `502` | Bad Gateway |
| `503` | Služba není k dispozici |
| `504` | Časový limit brány vypršel |
| `505` | Verze HTTP není podporována |
| `509` | Byl překročen limit šířky pásma |
