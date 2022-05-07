Viacročná forma
===============

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	sk: viacrocna-forma
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

Niekedy musíme formulár rozdeliť na niekoľko častí (stránok), spracovať ich samostatne a potom ich zložiť do jedného výsledku.

V tomto článku sú opísané metódy a návrhové vzory na tento účel.

> **Poznámka:**
>
> Problematika rozdelenia formulára do viacerých krokov je veľmi zložitá, najmä ak ju chcete urobiť dobre. Počas svojho života som sa stretol s mnohými prístupmi, o ktorých tu budem hovoriť. Niektoré prístupy vyzerajú lákavo, ale sú naivné a fungujú len v určitých prípadoch. Pri každom prístupe opíšem, kedy má zmysel a ktoré prístupy nemajú zmysel.

Návrh teoretického riešenia
-------------------------

Zvyčajne je cieľom získať základné údaje z prvého formulára na prvej stránke, overiť ich, potom ich "niekde" uložiť a zobraziť ďalšiu stránku.

Keď sa používateľ dostane na poslednú stránku, je potrebné odoslať celý formulár a spracovať vstupy.

V každom kroku je dôležité vždy starostlivo overiť všetky údaje a umožniť používateľovi ľubovoľne prechádzať stránky späť, aby mohol údaje opraviť, keď narazí na chybu. Okrem toho, ak sa má formulár vykresliť podmienečne na základe už získaných údajov, je to veľmi náročný proces.

Implementácia samotných formulárov
--------------------------------

Jednotlivé formuláre môžeme buď sami implementovať v čistom HTML a potom spracovať v PHP, alebo použiť hotové riešenia, ako napríklad <a href="https://doc.nette.org/cs/3.0/forms">Nette formuláre</a>.

> **Príklad zo života:**
>
> Veľmi často mi začínajúci programátori posielajú e-maily a kladú zdanlivo jednoduché otázky, na ktoré existuje hotové riešenie. Napríklad konkrétne o spracovaní formulárov v PHP.
>
> Vždy odporúčam úplne vynechať ručné spracovanie a namiesto toho použiť hotové riešenie. V skutočnosti je veľmi komplikované správne implementovať napríklad overenie zadaného e-mailu a to, že sa zhodujú 2 heslá v rámci 2 polí, pričom chceme používateľa presmerovať späť na predvyplnený formulár podľa jeho údajov a s chybovými správami v prípade chyby.
>
> Pretože ľudia **nevedia, že nevedia, že nevedia**, a tak namiesto toho, aby investovali 1 hodinu času do naučenia sa hotového riešenia 99,99 % problémov, radšej si zvolia vlastné riešenie, ktorého ladením strávia desiatky hodín, a stále sa vyskytujú prípady, keď formuláre nefungujú, hádžu chyby, majú bezpečnostné chyby a nechránia zadávané údaje.

Cieľom tohto kroku je teda implementovať niekoľko stránok na rôznych adresách URL, ktoré budú obsahovať prázdne formuláre.

Odporúčam implementovať každý formulár nezávisle od ostatných (atomicky) a odovzdávanie stavu riešiť na inej aplikačnej vrstve. Dôvodom je, že každý formulár bude v praxi inak spracovávať validáciu údajov, inak zapisovať svoje výstupy, inak spracovávať chyby a pravdepodobne ho budeme chcieť časom rozšíriť alebo zmeniť, takže nepotrebujeme poznať kontext celého procesu a meniť kvôli tomu desiatky stránok.

Prevod štátu
---------------

Pri spracovaní prvého formulára chceme najprv overiť prijaté údaje, a ak sú správne, potom používateľa presmerovať na druhý krok. Je dobré spracovať presmerovanie ako presmerovanie HTTP, pretože sa môže ľahko stať, že údaje nie sú platné, a v takom prípade chceme používateľa vrátiť na prvý formulár a nie na ďalší krok.

Stavy môžeme ukladať v podstate 4 spôsobmi:

**Neodporúča sa:**

- **Nikde neukladať** a kompletne prenášať v adrese URL. To má nevýhodu, že používateľ môže raz zmeniť údaje už odoslané v adrese URL a podvrhnúť tak vstup. Prípadne môžeme v adrese URL zverejniť citlivé informácie, napríklad heslá.
- **Plynulé pridávanie do <a href="/sessions">sessions</a>**, t. j. postupné vkladanie novo získaných údajov podľa kľúča do poľa. To má tú nevýhodu, že ak aplikácia urobí chybu, používateľ uviazne v reláciách a nemôže chybu nijako vyriešiť (okrem vymazania súborov cookie, čo je pre väčšinu ľudí veľmi ťažké), navyše pri nedokončenom formulári existuje riziko, že údaje zostanú predvyplnené a môže ich vidieť niekto iný. Oveľa horší prípad však nastane, ak má relácia veľmi krátku platnosť (napríklad 5 minút) a používateľ pri vypĺňaní posledného kroku stratí údaje z prvého kroku... to môže byť dosť nepríjemné.

**Doporučujeme:**

- **Ukladanie do databázy a odovzdanie identifikátora**. Pri prvom odoslaní formulára uložíme všetky zozbierané údaje do databázovej tabuľky a vygenerujeme náhodný identifikátor (napríklad 10 znakov dlhý reťazec), ktorý sa odovzdá medzi stránkami ako parameter. Výhodou je, že pri spracovaní akéhokoľvek formulára môžeme novo načítané a overené údaje zapísať priamo do tabuľky a v prípade poruchy máme fyzické zálohy analyzovaných formulárov a môžeme s nimi pracovať. Ak napríklad objednávka nie je dokončená, môžeme používateľovi poslať e-mail, že ju nedokončil, a zvýšiť tak šancu na predaj.
- Funkcia **Uložiť do aktuálne prihláseného účtu** funguje presne rovnako ako presmerovanie cez ID, len namiesto náhodného identifikátora (tokenu) použijeme reláciu na ID aktuálne prihláseného používateľa (ak existuje). Výhodou je, že predvyplnené údaje môžeme používateľovi v budúcnosti ľubovoľne zobraziť.

Záver
-----

Žiadne z týchto riešení nie je dokonalé ani jediné správne. Ja sám pri práci na viacstupňových riešeniach kombinujem viacero prístupov. Zvyčajne napríklad riešim nákupný košík ako databázovú tabuľku, do ktorej priradím už zozbierané údaje a priradím ju buď k používateľovi (ak je prihlásený), alebo k relácii (ak nie je prihlásený a ešte sa nepoznáme).
