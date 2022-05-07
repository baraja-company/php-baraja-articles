Parsovanie a spracovanie údajov v jazyku PHP
============================================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	sk: parsovanie-a-spracovanie-udajov-v-jazyku-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> Tento článok sa zaoberá metódami spracovania údajov v jazyku PHP, ale ešte nie je dokončený.

Takže dočasne len stručný náčrt možností:

- **Prehľadávanie znak po znaku** je veľmi stará metóda, ktorá robí kód neprehľadným, ale všetky ostatné metódy to robia interne.
- <a href="/explode">Explode</a>, rozdelenie reťazca oddeľovačom
- <a href="/regex">**Pravidelné výrazy**</a> sú najlepším spôsobom spracovania jednoduchých reťazcov
- **Tokenizer**, rozdelí komplexný reťazec na časti (tokeny) podľa regulárnych výrazov, napríklad takto sa spracováva PHP
