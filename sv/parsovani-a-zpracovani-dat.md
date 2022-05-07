Parsing och databehandling i PHP
================================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	sv: parsing-och-databehandling-i-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> Den här artikeln kommer att diskutera metoder för databehandling i PHP, men den är inte färdig ännu.

Så tillfälligt bara en snabb skiss av möjligheterna:

- **Crawl tecken för tecken** är en mycket gammal metod som gör koden rörig, men alla andra metoder gör det internt.
- <a href="/explode">Explode</a>, dela en sträng med en avgränsare
- <a href="/regex">**Regulära uttryck**</a> är det bästa sättet att hantera enkla strängar.
- **Tokenizer**, delar upp en komplex sträng i delar (tokens) enligt reguljära uttryck, till exempel så här behandlas PHP
