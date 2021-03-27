Získání seznamu všech definovaných funkcí
================================

> id: 99ed7887-33c8-44d7-b0cb-ac37b2336f48
> slugCS: ziskani-seznamu-vsech-definovanych-funkci
> publicationDate: 2021-03-27 22:01:31
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3

Občas se nám může hodit získat seznam všech dostupných funkcí na aktuálním prostředí. Je to zejména v případě, kdy spravujeme server někoho jiného a potřebujeme se trochu zorientovat.

Seznam funkcí lze získat zavoláním funkce `get_defined_functions()`, která vrátí data ve tvaru pole:

```
[
   internal => [
      ...,
   ],
   user => [
      ...,
   ]
]
```

Seznam funkcí je totiž rozdělený na dva velké seznamy.

- Interní (`internal`) funkce jsou takové, které definuje samotné PHP a nainstalované rozšíření.
- Uživatelské (`user`) funkce jsou zase takové, které definuje samotný uživatelský kód. To jsou všechny funkce, které jsme napsali do zdrojového kódu, nebo které jsou zahrnuty v nainstalovaných knihovnách.

Tento seznam lze dobře použít k debuggingu aplikace.
