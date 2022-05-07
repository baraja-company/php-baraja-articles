Dedičnosť a viditeľnosť v OOP
=============================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	sk: dedicnost-a-viditelnost-v-oop
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

Jednou zo základných vlastností objektovo orientovaného programovania je **dedičnosť** a <a href="/encapsulation">uzavretie</a>. Vďaka týmto funkciám budete môcť ľahko vytvárať zložitú aplikačnú logiku pri zachovaní dobrej čitateľnosti implementácie.

Princíp dedenia
-------------------

Dedičnosť vyjadruje, že implementácia jednej triedy je založená na inej triede. V terminológii OOP hovoríme o **descendentovi** (triede, ktorá dedí) a **ancedentovi** (triede, ktorú dedíme).

Vo všeobecnosti dedičnosť funguje tak, že potomok získa všetky vlastnosti predka, buď ich prevezme presne tak, ako ich mal predok, alebo ich upraví vlastným spôsobom, prípadne ich úplne prepíše a použije vlastnú implementáciu.

Využitie tohto prístupu je veľmi široké a dedičnosť sa používa v mnohých <a href="/design-patterns">návodoch na návrh</a>.

Skutočné použitie dedičnosti - prezentácie v aplikácii
--------------------

Dedičnosť je vhodná na navrhovanie takzvaných **prezentátorov**, čo je špeciálny druh triedy reprezentujúci logiku prepojenia v návrhovom vzore **MVC**.

Majme napríklad trojicu stránok `Homepage`, `Contact` a `Login`.

Pri implementácii každej stránky sa veľká časť logiky opakuje (napríklad prijatie požiadavky, vytvorenie adresy URL, vykreslenie šablóny a odoslanie výsledného HTML). Preto je vhodné implementovať jedného predka s touto logikou a použiť ju len v potomkoch.

Najprv začneme definovaním predka (na názve triedy nezáleží, používam konvenciu z rámca Nette):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implementácia metódy na vytvorenie adresy URL
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // logika vykresľovania šablóny
   }
}
```

Pri definovaní triedy som použil nové kľúčové slovo `abstract`, ktoré hovorí, že trieda `BasePresenter` je abstraktná. To znamená, že nemôžeme vytvoriť jej inštanciu a musíme ju použiť len tak, aby ju zdedila iná trieda a implementovala ju. Abstrakcia má aj ďalšie užitočné výhody, ktoré si rozoberieme neskôr. Trieda nemusí byť abstraktná, aby bola dedičná - je to len jedno z možných nastavení.

Teraz môžeme implementovať druhú triedu, napríklad `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // logika vykresľovania
       $this->renderTemplate('domovská stránka', [
          'kontaktLinka' => $this->link('Kontakt:predvolený'),
       ]);
   }
}
```

Teraz máte funkčnú triedu `HomepagePresenter`. Všimnite si, že trieda je `finálna`, čo znamená, že už nemôže byť dedená, čo zaručuje, že metódy budú použité presne tak, ako sme ich špecifikovali.

Pri implementácii triedy sme vytvorili novú metódu `run()`, ktorú môže obsluhovať iba `HomepagePresenter`. Vo vnútri metódy voláme metódu `renderTemplate()` a `link()`, ktoré trieda neobsahuje. Na tom však nezáleží, pretože kľúčové slovo `extends` nám hovorí, odkiaľ sa majú metódy dediť, takže sa použijú tieto metódy.

Vďaka dedičnosti sa nám podarilo dosiahnuť opätovnú použiteľnosť kódu, pretože raz napísané metódy sa dajú použiť na viacerých miestach.

Prevzatie implementácie konkrétnej metódy
------------

Veľmi často môže byť užitočné prepísať správanie konkrétnej metódy počas dedenia. Ak by sme napríklad chceli zmeniť správanie metódy `link()` z predchádzajúceho príkladu v `ContactPresenter`, vyzeralo by to takto:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // logika vykresľovania
      echo $this->link('Úvodná stránka:default', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Ak chcete prepísať implementáciu, jednoducho definujte metódu v podriadenom objekte a prepíšte telo metódy. Dôležité je splniť rozhranie a implementovať rovnaké vstupné argumenty.

Viditeľnosť dedičnosti
--------------------------

Niekedy by sme chceli niektoré metódy pri dedení skryť a používať len interne. Prípadne ich povoľte používať len pri dedení a nie ako verejné rozhranie.

Vo všeobecnosti preto existuje niekoľko jednoduchých pravidiel viditeľnosti. Metódy označujeme ako `public`, `protected` alebo `private` a pravidlá viditeľnosti sú nasledovné:

- Metódy `public` môže ktokoľvek zavolať odkiaľkoľvek, teda pri vytváraní inštancie predka aj potomka.
- Metódy `protected` môže volať iba predok alebo potomok, ale pri vytváraní inštancie ich nemožno volať z verejného rozhrania. Ide o interné metódy na dosiahnutie dedičnosti (vhodná aplikácia je napríklad na metódu `link()` v predchádzajúcom príklade).
- Iba aktuálna trieda môže volať metódy `private` bez ohľadu na nastavenie dedičnosti a verejného rozhrania.

Zmena viditeľnosti za behu
----------------------------

Vo veľmi špecifických prípadoch môže byť užitočné zmeniť viditeľnosť metódy za behu a potom ju zavolať. Používajú ho napríklad rôzne knižnice Doctrine.

Na zmenu viditeľnosti potom použijeme natívnu triedu **ReflectionClass** implementovanú samotným PHP.
