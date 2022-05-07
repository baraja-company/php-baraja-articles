Séria Doctrine - Úvod
=====================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	sk: seria-doctrine---uvod
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine je pokročilá knižnica PHP na objektovo orientovanú prácu s databázami. Hlavným účelom a cieľom Doctrine je popísať databázovú schému pomocou dátových entít a manipulovať s údajmi plne objektovo orientovaným spôsobom.

Táto paradigma sa nazýva ORM (Object-relational mapping), čo je [design-pattern](/design-patterns) na prevod (obalenie) údajov uložených v relačnej databáze na objekt, ktorý možno použiť v objektovo orientovanom jazyku. Preto, aby ste pochopili a používali Doctrine, musíte poznať aspoň základy [objektovo orientovaného programovania](/oop).

Prečo sa učiť doktrínu?
------------------------

Existuje mnoho dôvodov:

- Doctrine je najpoužívanejšia databáza ORM, ktorú používa väčšina pokročilých používateľov PHP.
- Zásadne zjednoduší návrh vašej aplikácie PHP
- Poskytujete konzistentný spôsob návrhu, verzie, prenosu a zálohovania databázovej schémy
- Stiahnutím balíka môžete [získať množstvo databázových tabuliek](https://github.com/baraja-core/shop-product) bez toho, aby ste museli čokoľvek zisťovať a konfigurovať.
- Vzťahy medzi tabuľkami sa stávajú skutočnými fyzickými entitami
- Výstupy databázy nebudú obyčajné netypované polia, ale dostanete skutočné fyzické objekty
- Získate jednoduchý spôsob vykonávania mnohých operácií súčasne v rámci jednej transakcie
- Bezpečnosť a odolnosť aplikácií ľahko zvýšite tým, že jednoducho budete vedieť, kedy sa čo stane a že sa to stane bezpečne.
- Získate ľahko testovateľnú vrstvu kódu a databázy
- Objavíte celý ekosystém okolo Doctrine, ktorý elegantne rieši mnohé problémy. Často nájdete jednoduché riešenia zložitých problémov, ktoré je inak takmer nemožné ľahko vyriešiť.
- Naučíte sa veľa nových vecí, preskúmate nové nápady a naplno využijete potenciál databázy.
- Zbavte sa zložitých dotazov SQL. Doctrine poskytuje vlastné rozhranie na písanie dotazov (DQL), ktoré je veľmi výkonné
- Aplikácie sa zrýchlia. Ľahko objavíte priestor na optimalizáciu aplikácie, využijete lenivé načítanie a nájdete úzke miesta aplikácie.

Autor tohto článku ([Jan Barasek](https://baraja.cz)) dlhodobo zastáva názor, že Doctrine je najlepší spôsob práce s databázou PHP. Jednoducho nemá konkurenciu.

Ako začať?
----------

Skôr ako začnete naplno používať Doctrine, musíte si pripraviť vhodné prostredie. Ak s PHP len začínate alebo nemáte staršie znalosti, najlepšou voľbou je nainštalovať Nette Framework s balíkom rozšírenia [Baraja Doctrine](https://github.com/baraja-core/doctrine), ktorý automaticky integruje plnú podporu. Najprv si stiahnite balík prostredníctvom [Composer](/composer), potom nastavte rozšírenie DI a Doctrine začne pracovať automaticky.

Aby Doctrine fungoval správne, je potrebné pripraviť prázdnu databázu (Doctrine môže pracovať s existujúcim projektom, ale pre prvé kroky je to nevhodné, pretože hrozí prepísanie existujúcich údajov) a nakonfigurovať pripojenie. Keďže Doctrine nie je len databázová knižnica, ale poskytuje pokročilý databázový rámec, musíte [vyriešiť ďalšie konfigurácie](/configure-connections-with-baraja-doctrine). Väčšina nastavení je automaticky prepísaná v tomto balíku pre Nette, avšak v minimálnej konfigurácii musí váš server podporovať rozšírenia `APCu Cache` alebo `SQLite3`.

Ak bolo všetko správne nakonfigurované, v Nette sa vytvorí nová služba DI `Baraja\Doctrine\EntityManager`, ktorú môžete [injektovať](https://doc.nette.org/cs/3.1/di-usage) do programu Presenter:

```php
<?php

menný priestor App\FrontModule\Presenters;

použiť Baraja\Doctrine\EntityManager;

finálna trieda HomepagePresenter rozširuje BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Ak sa vám podarí injektovať základnú službu EntityManager, môžete sa začať učiť a pracovať s Doctrine.

Ako postupovať?
--------

Nasledujúce kapitoly sú kombináciou referenčnej príručky technológie Doctrine, dlhoročných skúseností, návrhových vzorov a hotových riešení. Spoločne prejdeme všetky základné prvky Doctrine od definovania vlastnej entity, cez generovanie fyzickej schémy databázy až po prácu s nástrojom na tvorbu verzií a produkčné nasadenie.

Doctrine používam už veľmi dlho a vyriešil som v nej tisíce prípadov. Ukážeme si tipy a triky, ako používať Doctrine na optimalizáciu rýchlosti databázy a ako vhodne navrhnúť databázu. Doctrine môžete použiť aj pre existujúci projekt (ak spĺňate určité podmienky) a my vám ukážeme, ako na to.

Táto séria článkov bola vytvorená na pomoc mojim študentom v oblasti školení a poradenstva. Ak potrebujete niektoré témy podrobnejšie prediskutovať alebo vysvetliť, môžete mi napísať na e-mailovú adresu jan@barasek.com. Keďže ide o pomerne náročnú technológiu, všetky otázky budú považované za platenú konzultáciu.
