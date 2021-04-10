Bezpečná aplikace
=================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 
> publicationDate: "2019-10-01 14:19:04"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Pokud to s vývojem webových aplikací myslíte aspoň trochu vážně a stránky budou později dostupné na internetu, je velmi důležité vyřešit zabezpečení.

Reálně na vývojáře čekají tyto hrozby:

- **V aplikaci nastala vnitřní chyba**, například proto, že programátor udělal chybu, které si při psaní kódu nevšiml, nebo se projebuje jen "někdy"
- **Server má chybnou konfiguraci** nebo se **změnilo prostředí**, protože server admin změnil chování serveru a weby pro to nebyly přizpůsobeny. Případně probíhá deploy na nový stroj a neznáme přesnou konfiguraci.
- **Někdo se snaží zaútočit**, ať už externí útočník, nebo bývalý zaměstnanec zevnitř systému.

Všechny situace jsou extrémně nepříjemné a je potřeba ihned jednat. Navrhnout aplikaci tak, že nikdy nedojde k chybě není technicky možné.

Při vývoji je velmi důležité testovat již jednou napsaný kód a pokud na serveru dojde k chybě, je důležité o tom ihned informovat vývojáře s přesným popisem problému.

Pro odhalení chyb a sledování stavu serveru doporučuji nástroj <a href="https://tracy.nette.org/">Tracy</a>, který loguje veškeré fatální chyby, neošetřené výjimky a mnoho dalších do souborů přímo na disk serveru. Navíc lze nastavit e-mail, kam se budou posílat notifikace.

Máte zájem o konzultaci bezpečnosti Vaší aplikace?

Napište mi.
