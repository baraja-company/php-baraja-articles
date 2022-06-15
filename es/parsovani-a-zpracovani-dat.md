Análisis y procesamiento de datos en PHP
========================================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	es: analisis-y-procesamiento-de-datos-en-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> Este artículo tratará sobre los métodos para procesar datos en PHP, pero aún no está terminado.

Así que, temporalmente, sólo un rápido esbozo de las posibilidades:

- **El rastreo carácter por carácter** es un método muy antiguo que ensucia el código, pero todos los demás métodos lo hacen internamente.
- <a href="/explode">Explotar</a>, dividir una cadena por un delimitador
- <a href="/regex">**Expresiones regulares**</a> son la mejor manera de manejar cadenas simples
- **Tokenizer**, divide una cadena compleja en trozos (tokens) según expresiones regulares, por ejemplo así es como se procesa PHP
