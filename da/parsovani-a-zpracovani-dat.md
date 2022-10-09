Parsing og databehandling i PHP
===============================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	da: parsing-og-databehandling-i-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> Denne artikel omhandler metoder til behandling af data i PHP, men den er ikke færdig endnu.

Så foreløbig blot en hurtig skitse af mulighederne:

- **Crawl tegn for tegn** er en meget gammel metode, som gør koden rodet, men alle andre metoder gør det internt.
- <a href="/explode">Explode</a>, opdeling af en streng med en afgrænser
- <a href="/regex">**Regulære udtryk**</a> er den bedste måde at håndtere enkle strenge på
- **Tokenizer**, opdeler en kompleks streng i stykker (tokens) i henhold til regulære udtryk, f.eks. er det sådan her, PHP bliver behandlet
