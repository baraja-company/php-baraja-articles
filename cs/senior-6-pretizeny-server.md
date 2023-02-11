Urgentní oprava přetíženého serveru
===================================

> id: "07c5d1ed-f854-4797-8efe-350a24594762"
> slug:
> 	cs: senior-6-pretizeny-server
>
> publicationDate: "2023-02-11 14:25:00"
> mainCategoryId: 8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a

Externí monitorovací nástroj vám zahlásí, že se průměrná doba response z 5 sledovaných URL zvýšila na dvojnásobek za posledních 30 minut. Projekt běží na jednom fyzickém serveru, který není ve vaší správě, a běží kdesi v datacentru. Připojíte se přes SSH, spustíte htop, a vidíte, že zátěž CPU je 95 % a paměti jsou dávno přeplněné.

Podle gitu víte, že se asi týden zpět dělala migrace databáze na novou strukturu tabulek, a kolega do chatu píše, že musel migraci spustit přes noc, neboť přepočet sloupců a indexů trval asi 5 hodin, během kterých byla skoro celá databáze zamknutá, a nefungoval INSERT ani SELECT.

Za problémy s výkonem tedy pravděpodobně mohou nevhodně navržené indexy, špatně předělané SQL dotazy, nebo velký connection pooling. Na revert není čas, na webu je podle Google Analytics právě 7 tisíc uživatelů, a výpadek na 5 hodin by znamenal reputační riziko u klienta, a za tu dobu ztrátu desítek až stovek tisíc korun (těžko odhadnout, projeťáci si toho vymyslí dost). Uvědomíte si, že testovat pouze funkčnost na testovacím prostředí nestačí, a musíte zavést i test zátěže.

Protože jde o důležitý e-shop vašeho největšího klienta, a očekáváte, že se situace může zhoršit, na rozhodnutí máte 30 sekund.

Jak se zachováte?

1. Nebudete dělat nic. Web bude sice pomalejší, ale dokud to server ustojí, tak asi dobrý...
2. Zkusíte připravit plán pro revert DB struktury, a spustíte ho co nejdřív to bude možné.
3. Přemigrujete web na výkonnější server (to ale znamená operativně klientovi zvýšit cenu, a pak počkat třeba 6 hodin, než se migrace dokončí, protože máte stovky GB databázových tabulek).
4. Zjistíte, které SQL dotazy a které stránky trvají nejvíc, a jednoduše je zakážete.
5. Spustíte Cloudflare a necháte staticky odbavovat co půjde.
6. Nějaká jiná kouzla (moc se toho asi vymyslet nedá)...
