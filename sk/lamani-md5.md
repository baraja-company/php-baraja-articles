Ako prelomiť funkciu md5
========================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	sk: ako-prelomit-funkciu-md5
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 je veľmi často používaná funkcia na výpočet hashov.

Začiatočníci ho často používajú na <a href="/hashovani">heslovanie hesla</a>, čo nie je dobrý nápad, pretože existuje mnoho spôsobov, ako získať pôvodné heslo.

V tomto článku sú opísané konkrétne metódy, ako to dosiahnuť.

Časová zložitosť
----------------

Celé zabezpečenie je postavené na tom, že vyskúšanie všetkých hesiel trvá neúmerne dlho. No, malo by to tak byť. Problémom algoritmu `md5()` je najmä to, že je to veľmi rýchla funkcia. Na bežnom počítači nie je problém vypočítať viac ako milión hashov za sekundu.

Ak heslo prelomíme postupným skúšaním kombinácií, ide o **útok hrubou silou**.

Metódy krakovania
----------------

Existuje niekoľko stratégií:

- Postupné testovanie metódou pokus-omyl (útok hrubou silou)
- Testovanie slovníkových hesiel
- Dúhové tabuľky (vopred vypočítaná hash databáza)
- Vyhľadávanie v službe Google
- Zásah kolízií v algoritme

Metód je oveľa viac, tento článok opisuje len tie najbežnejšie.

Stratégie prelomenia hrubou silou
-----------------------------

Všetky kombinácie písmen, číslic a iných znakov sa skúšajú postupne.

Vygenerované pokusy sa postupne hashujú a porovnávajú s pôvodným hashom.

Tak napríklad:

```php
aaaaaaabaaacaaadaaaeaaaf...
```

Problém tohto útoku je v samotnom algoritme `md5()`, ak by sme skúšali len malé písmená anglickej abecedy a čísla, vyskúšanie všetkých kombinácií na bežne dostupnom počítači by trvalo maximálne desiatky minút.

Preto je dôležité vyberať dlhé heslá, najlepšie náhodné a so špeciálnymi znakmi.

Stratégia slovníkového útoku
----------------------------

Ľudia si zvyčajne vyberajú slabé heslá, ktoré existujú v slovníku.

Ak využijeme túto skutočnosť, môžeme rýchlo vyradiť nepravdepodobné varianty ako `6w1SCq5cs` a namiesto toho hádať existujúce slová.

Okrem toho z predchádzajúcich únikov hesiel veľkých spoločností vieme, že používatelia si na začiatku hesla volia veľké písmeno a na konci číslo. Pozrime sa - má to aj vaše heslo? :)

Dúhové tabuľky - predpočítaná databáza
--------------------------------------

Keďže jednému heslu vždy zodpovedá rovnaký hash, je ľahké prepočítať obrovskú databázu, v ktorej sa budú heslá hľadať ako prvé.

V skutočnosti je vyhľadávanie vždy rádovo rýchlejšie ako opakované prehľadávanie hashov.

Pri väčších únikoch údajov možno navyše heslá týmto spôsobom hashovať paralelne a rýchlo tak získať napríklad 10 % všetkých používateľských hesiel.

Dobrou databázou hesiel je napríklad <a href="https://crackstation.net/">Crack Station</a>.

Vyhľadávanie Google
-------------------

Mnohé jednoduché heslá sú známe priamo spoločnosti Google, pretože indexuje stránky, ktoré obsahujú heslá.

Ako prvú možnosť vždy používam Google. :)

Vyhľadávanie kolízií v algoritme
--------------------------

<a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Dirichletov princíp</a> popisuje, že ak máme súbor hesiel, ktoré majú vždy 32 znakov, potom existujú aspoň 2 rôzne heslá s 33 znakmi (jedno dlhšie), ktoré generujú rovnaký hash.

V praxi nemá zmysel hľadať kolízie, ale niekedy si autor aplikácie sám uľahčí hádanie prepočítaním kolízií.

Napríklad:

```php
$password = 'heslo';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x zahashované cez md5()
```

V tomto prípade má zmysel hádať skôr kolíziu ako pôvodný hash.

Na zdravie pri pečení!
