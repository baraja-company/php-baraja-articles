Parsing ed elaborazione dei dati in PHP
=======================================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	it: parsing-ed-elaborazione-dei-dati-in-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> Questo articolo discuterà i metodi per elaborare i dati in PHP, ma non è ancora finito.

Quindi temporaneamente solo un rapido schizzo delle possibilità:

- **Crawl carattere per carattere** è un metodo molto vecchio che rende il codice disordinato, ma tutti gli altri metodi lo fanno internamente.
- <a href="/explode">Explode</a>, dividendo una stringa con un delimitatore
- <a href="/regex">**Le espressioni regolari**</a> sono il modo migliore per gestire stringhe semplici
- **Tokenizer**, divide una stringa complessa in pezzi (token) secondo le espressioni regolari, per esempio questo è il modo in cui PHP viene processato
