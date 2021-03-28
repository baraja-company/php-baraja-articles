Dědičnost a viditelnost v OOP
================================

> id: "71896a55-dcfd-4bb6-9f53-64f22b1514eb"
> slugCS: dedicnost-a-viditelnost
> publicationDate: "2020-02-16 22:17:05"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

Jedna ze základních vlastností Objektově orientovaného programování je **dědičnost** a <a href="/zapouzdreni">zapouzdření</a>. Díky těmto vlastnostem budete schopni snadno budovat složité aplikační logiky se zachováním dobré čitelnosti implementace.

Princip dědičnosti
-------------------

Dědičnost vyjadřuje, že implementace jedné třídy vychází z jiné. V terminologii OOP hovoříme o **potomkovi** (třída, která dědí) a **předkovi** (třída, kterou dědíme).

V obecné rovině dědičnost funguje tak, že potomek získá všechny vlastnosti předka, které buď přijme přesně tak, jak je měl předek, nebo je vlastním způsobem upraví, případně úplně přepíše a použije vlastní implementaci.

Použití tohoto přístupu je velmi široké a dědičnost využívá řada <a href="/navrhove-vzory">návrhových vzorů</a>.

Reálné použití dědičnosti - presentery v aplikaci
--------------------

Dědičnost se výborně hodí pro návrh tzv. **presenterů**, což je speciální druh třídy reprezentující propojovací logiku v návrhovém vzoru **MVC**.

Mějme například trojici stránek `Homepage` (hlavní strana), `Contact` (kontaktní informace) a `Login` (přihlášení).

Při implementaci jednotlivých stránek se bude velká část logiky opakovat (například přijmutí requestu, sestavení URL, vykreslení šablony a odeslání výsledného HTML). Je proto výhodné implementovat jednoho předkat s touto logikou a v potomcích ji pouze použít.

Začneme nejprve definici předka (na názvu třídy nezáleží, používám konvenci z Nette frameworku):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implementace metody pro sestavení URL
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // vykreslovací logika šablony
   }
}
```

Při definici třídy jsem použil nové klíčové slovo `abstract`, které říká, že třída `BasePresenter` je abstraktní. To znamená, že nepůjde vytvořit instance a musíme ji použít jenom tak, že ji bude jiná třída dědit a implementovat. Abstrakce má ještě další užitečné výhody, o kterých si povíme později. Abyste třídu mohli dědit, tak nemusí být abstraktní - jde jen o jedno z možných nastavení.

Nyní již můžeme implementovat druhou třídu, například `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // vykreslovací logika
       $this->renderTemplate('homepage', [
          'contactLink' => $this->link('Contact:default'),
       ]);
   }
}
```

Nyní máte funkční třídu `HomepagePresenteru`. Všimněte si, že je třída `final`, což znamená, že již dál nepůjde dědit a zaručíme si tím, že metody budou použity přesně tak, jak jsme je uvedli.

Při implementaci třídy jsme vytvořili novou metodu `run()`, kterou umí obsluhovat pouze `HomepagePresenter`. Uvnitř metody voláme metodu `renderTemplate()` a `link()`, kterou třída neobsahuje. To ale ničemu nevadí, protože klíčovým slovem `extends` říkáme, odkud se mají metody podědit a proto se použijí ty.

Díky dědičnosti jsme dokázali docílit znovupoužitelnosti kódu, protože jednou napsané metody můžeme použít na více místech.

Přepsání implementace konkrétní metody
------------

Velmi často se může hodit přepsat chování konkrétní metody při dědičnosti. Pokud bychom chtěli například změnit chování metody `link()` z předchozího příkladu v `ContactPresenteru`, bude to vypadat takto:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // vykreslovací logika
      echo $this->link('Homepage:default', []);
   }
   
   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Implementaci přepíšeme jednoduše tak, že metodu definujeme v potomkovi znovu a přepíšeme tělo metody. Důležité je splnit rozhraní a implementovat stejné vstupní argumenty.

Viditelnost při dědičnosti
--------------------------

Někdy by se nám hodilo při dědičnosti některé metody ukrýt a použít jen interně. Případně umožnit použít jen při dědičnosti a ne jako veřejné rozhraní.

Obecně proto existuje několik jednoduchých pravidel viditelnosti. Metody označíme slovem `public`, `protected` nebo `private`, přičemž pravidla viditelnosti jsou následující:

- `public` metody může kdokoli zavolat odkudkoli, tedy při vytvoření instance předka i potomka.
- `protected` metody může zavolat jen předek nebo potomek, nelze je ale zavolat z veřejného rozhraní při vytvoření instance. Jde o interní metody pro dosažení dědičnosti (vhodné použití je například na metodu `link()` v předchozím příkladu).
- `private` metody může zavolat jen aktuální třída bez ohledu na nastavení dědičnosti a veřejného rozhraní.

Změna viditelnosti za běhu
----------------------------

V hodně specifických případech se může hodit změnit viditelnost metody za běhu a tu následně zavolat. Toto používají například různé knihovny typu Doctrine.

Pro změnu viditelnosti pak použijeme nativní třídu **ReflectionClass**, kterou implementuje samotné PHP.
