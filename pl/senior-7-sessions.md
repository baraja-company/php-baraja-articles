Jak radzić sobie z nagłymi awariami skryptów PHP
================================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	pl: jak-radzic-sobie-z-naglymi-awariami-skryptow-php
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

Historia z końca 2016 roku, kiedy to zostałem dosłownie uratowany przez kolegę: w aplikacji PHP decydujemy się na odprawianie obrazków za pośrednictwem skryptu proxy, który m.in. potrafi dostosować ich wymiary i inne parametry do przychodzącego żądania. W ramach optymalizacji zapisujesz również wygenerowane warianty fizycznie na dysku.

Jednak w pracy produkcyjnej nagle zaczynasz widzieć ogromne obciążenie i tysiące żądań ustawionych w kolejce. Obrazy są ładowane sekwencyjnie jeden po drugim dla każdego użytkownika. Nie działają odświeżenia strony i kliknięcia w linki. Aplikacja wydaje się całkowicie zamrożona. Działa tylko po to, by czekać, aż wszystko się przetworzy.

Co może być problemem? W tekście wymieniłem 3 główne wskazówki umożliwiające szybkie wyszukanie problemu. Hotfix ma banalne rozwiązanie.
