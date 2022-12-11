Komunikácia cez SSH a kľúč RSA2
===============================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	sk: komunikacia-cez-ssh-a-kluc-rsa2
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH je sieťový protokol na šifrovaný prenos súborov a terminálov. SSH sa najčastejšie používa na vzdialené ovládanie webového servera a bezpečný prenos súborov. Na rozdiel od FTP ponúka vlastné šifrované pripojenie. SSH komunikuje cez predvolený port `22`. Pripojenie sa inicializuje pomocou používateľského mena a adresy cieľového servera. Na overenie možno použiť heslo (neodporúča sa) alebo kľúč SSH RSA2 (odporúča sa).

Získanie (generovanie) kľúča
--------------------------

Pred pripojením k serveru musíme získať (alebo vygenerovať) náš prvý kľúč SSH RSA2. Dôležité je, že ide o algoritmus `RSA2`. Je to preto, že existuje niekoľko kľúčov a nie všetky sa dajú použiť.

V Linuxe sa na jeho generovanie používa nástroj `ssh-keygen`, ktorému zadáme zložitosť kľúča (v tomto prípade 4096) a e-mail oprávneného používateľa:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Po spustení príkazu budeme požiadaní o zadanie cesty na uloženie kľúča a prípadného `hesla` (autorizačné heslo). Ako cestu nezadávajte nič (automaticky sa vyberie predvolené umiestnenie) a voliteľne zadajte prístupovú frázu (ak tak urobíte, budete musieť pred použitím kľúča zakaždým zadať rovnaké heslo).

Vygenerovaný kľúč sa automaticky uloží do predvoleného umiestnenia `~/.ssh`, t. j. do adresára `.ssh` v domovskom adresári aktuálneho používateľa.

Generovanie kľúča SSH v systéme Windows
-------------------------------

Systém Windows bohužiaľ nemá predvolenú cestu pre kľúč SSH. Preto je ideálne nainštalovať napríklad nástroj `Putty` a `PuttyGen` na generovanie kľúča. Vždy si vyberte algoritmus `RSA2`. Opäť sa vygeneruje dvojica kľúčov, preto ich niekde bezpečne uložte. Pred použitím kľúča SSH v programe `Putty` musíte vybrať cestu k disku, z ktorého sa má kľúč načítať. V Linuxe to nie je potrebné, existuje predvolená cesta k disku.

Zabezpečenie kľúčov
---------------

Pri generovaní kľúčov sa v skutočnosti generujú dva. Jeden verejný kľúč (ten, ktorý poskytnete druhej strane, aby ste umožnili komunikáciu) a súkromný kľúč (ten je len váš, nikdy ho nikomu nepoviete a používa sa na dešifrovanie komunikácie).

Je dôležité, aby ste nikdy nestratili súkromný kľúč. Stratiť ho znamená prerušiť celú komunikáciu!

Vo všeobecnosti sa odporúča generovať jedinečný pár verejného a súkromného kľúča pre každé zariadenie a používateľa, aby sa znížila pravdepodobnosť úniku. Ak však chcete kľúč preniesť medzi zariadeniami, môžete to urobiť. Kľúč SSH je na úrovni hesla, takže keď ho presuniete na iný počítač, pripojenie funguje.

Niektoré servery si pamätajú, s ktorým zariadením naposledy komunikovali prostredníctvom SSH. Preto je možné, že po prenesení kľúča do nového počítača nebude pripojenie fungovať. V tomto prípade je potrebné vymazať vyrovnávaciu pamäť kľúča na serveri.

Autorizácia kľúča na pripojenie k serveru
--------------------------------------

Na pripojenie k serveru sa používa príkaz `ssh`. Stačí zadať používateľa a doménu:

```shell
ssh root@baraja.cz
```

Potom sa pokúsi nadviazať spojenie SSH. Ak máte funkčný a správne nakonfigurovaný kľúč SSH, pripojenie sa vytvorí automaticky. Ak nie, musíte zadať heslo.

Ak sa chcete voči serveru overovať pomocou kľúča SSH namiesto hesla, musíte na server preniesť svoj **verejný kľúč**.

Stačí ho zobraziť príkazom:

```shell
cat ~/.ssh/id_rsa.pub
```

A skopírujte celý jeho obsah na cieľový server na miesto `~/.ssh/authorized_keys`. Ak máte viac ako jeden kľúč, každý na samostatnom riadku.
