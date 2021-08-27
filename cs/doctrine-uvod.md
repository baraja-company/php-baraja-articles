Seriál o Doctrine - úvod
========================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 
> publicationDate: "2021-08-27 11:40:00"
> mainCategoryId: "818d311a-0f58-4df7-a9a4-da7d21489dd6"

Doctrine je vyspělá PHP knihovna pro objektovou práci s databází. Hlavním smyslem a cílem Doctrine je popsat databázové schéma pomocí datových entit a s daty manipulovat plně objektovým způsobem.

Tomuto paradigmatu se říká ORM (Object–relational mapping), což je [návrhový vzor](/navrhove-vzory) pro převod (obalení) dat uložených v relační databázi na objekt, který lze použít v objektově orientovaném jazyce. Pro pochopení a použití Doctrine tedy musíte umět aspoň základy [Objektově orientovaného programování](/oop).

Proč se Doctrine naučit?
------------------------

Důvodů je opravdu hodně:

- Doctrine je nejpoužívanější ORM databáze, používá ji většina pokročilé PHP komunity
- Zásadním způsobem zjednodušíte návrh vaší PHP aplikace
- Zajistíte konzistentní způsob, jak navrhovat, verzovat, přenášet a zálohovat databázové schéma
- Hodně databázových tabulek můžete [získat stažením balíčku](https://github.com/baraja-core/shop-product) bez nutnosti cokoli vymýšlet a konfigurovat
- Z relací mezi tabulkami se stanou opravdové fyzické entity
- Výstupy z databáze nebudou obyčejná netypová pole, ale získáte opravdové fyzické objekty
- Získáte snadný způsob, jak provádět mnoho operací současně v rámci jedné transakce
- Snadno zvýšíte bezpečnost a odolnost aplikace tím, že budete zkrátka vědět, kdy se co děje, a že se to stane bezpečně
- Získáte snadno testovatelný kód i databázovou vrstvu
- Objevíte celý ekosystém kolem Doctrine, který řeší mnoho problémů elegantně. Často najdete jednoduché řešení komplexních problémů, které jinak téměř nelze jednoduše vyřešit
- Naučíte se spoustu nových věcí, poznáte nové myšlenky a využijete databázi naplno
- Zbavíte se složitých SQL dotazů. Doctrine poskytuje vlastní rozhraní pro psaní dotazů (DQL), které je velmi mocné
- Aplikace se zrychlí. Snadno objevíte prostor pro optimalizaci aplikace, využijete lazy loading a najdete úzká hrdla aplikace

Dlouhodobý názor autora tohoto článku ([Jan Barášek](https://baraja.cz)) je, že Doctrine je nejlepší způsob, jak pracovat s databází v jazyce PHP. Zkrátka nemá konkurenci.

Jak začít?
----------

Ještě než začnete používat Doctrine naplno, je potřeba připravit vhodné prostředí. Pokud s PHP teprve začínáte, nebo nemáte seniorní znalosti, nejlepší volbou bude instalace Nette Framework s rozšiřujícím balíkem [Baraja Doctrine](https://github.com/baraja-core/doctrine), který automaticky integruje plnou podporu. Nejprve si stáhněte balík přes [Composer](/composer), poté nastavte DI Extension a Doctrine začne fungovat automaticky.

Aby Doctrine fungovala korektně, tak je potřeba připravit prázdnou databázi (Doctrine umí pracovat i s existujícím projektem, ale pro první kroky to je nevhodné, protože hrozí přepsání existujících dat) a nakonfigurovat připojení. Protože Doctrine není pouze databázová knihovna, ale poskytuje pokročilý databázový framework, tak je potřeba [vyřešit i další konfiguraci](/konfigurace-spojeni-s-baraja-doctrine). Většina nastavení se v uvedeném balíčku pro Nette propíše automaticky, nicméně v minimální konfiguraci váš server musí podporovat rozšíření `APCu Cache` nebo `SQLite3`.

Pokud se vše povedlo nakonfigurovat správně, tak v Nette vznikne nová DI služba `Baraja\Doctrine\EntityManager`, kterou můžete [injectnout](https://doc.nette.org/cs/3.1/di-usage) do Presenteru:

```php
<?php

namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Pokud se vám podaří injectnout základní službu EntityManageru, můžete se pustit do studia a práci s Doctrine.

Jak dál?
--------

Následující kapitoly jsou kombinací referenční příručky technologie Doctrine, dlouhodobých zkušeností, návrhových vzorů a hotových řešení. Projdeme společně všechny základní prvky Doctrine od definice vlastní entity, přes generování fyzického databázového schématu, až po spolupráci s verzovacím nástrojem a produkční nasazení.

Doctrine používám velmi dlouho a řešil jsem v ní tisíce případů. Ukážeme si tipy a triky, jak pomocí Doctrine optimalizovat rychlost databáze a jak databázi vhodně navrhnout. Doctrine je také možné použít pro již stávající projekt (pokud splníte určité podmínky) a ukážeme si, jak na to.

Tato série článků vznikla jako pomocné materiály pro studenty mého školení a konzultací. Pokud potřebujete probrat nebo podrobněji vysvětlit určitá témata, můžete mi napsat na e-mail jan@barasek.com. Protože jde o relativně náročnou technologii, bude na všechny dotazy hleděno jako na placenou konzultaci.
