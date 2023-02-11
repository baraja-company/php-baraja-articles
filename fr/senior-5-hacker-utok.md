Attaque de l'agence par des pirates informatiques
=================================================

> id: '7d5edd94-cce4-4a32-9ec5-51260dcd1653'
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
> 	fr: attaque-de-l-agence-par-des-pirates-informatiques
> 
> publicationDate: '2023-02-11 14:20:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '18511f7b3ff80124720244a93e140932'

Une histoire de 2017 : vous travaillez comme développeur principal dans une agence, et vous gérez environ 300 projets de tailles diverses que l'entreprise a développés pendant cette période. La plupart d'entre elles sont de simples applications Nette comportant jusqu'à 10 modèles, quelques formulaires et des tables de base de données. Rien d'extraordinaire. Vous ne savez pas grand-chose des projets, car chacun d'entre eux a été développé par un fournisseur légèrement différent, les gens entrent et sortent de l'entreprise et vous espérez arriver à vos fins d'une manière ou d'une autre. Comme l'entreprise fait beaucoup d'optimisation des coûts, elle héberge peut-être 50 projets sur un serveur à la fois.

Soudain, un chef de projet vient vous voir en courant pour vous dire que l'un des sites importants du client ne fonctionne plus. Au lieu du vrai site, vous obtenez un écran noir, et il y a toutes sortes de textes disant que le site a été piraté et qu'il a accès à l'ensemble du serveur.

Encore une fois, vous ne savez pas (et ne pouvez pas valablement) en savoir autant sur l'architecture et le contenu des projets, et de nombreux projets fonctionnent uniquement sur le serveur. Le projectionniste vous pousse à croire que le site doit fonctionner, et pourtant vous ne connaissez pas l'ampleur de l'attaque et ne savez pas où est passé l'attaquant, ni s'il existe des portes dérobées du passé.

Comment décidez-vous ?

1. vous ne faites rien et essayez d'abord d'analyser l'incident.
2. vous mettez le serveur hors ligne. Vous ne connaissez pas l'étendue des dégâts et l'attaquant peut continuer à pénétrer dans le serveur et à voler des données. En même temps, cela signifie que de nombreux sites sont indisponibles.
3. Vous effectuez une restauration d'un site spécifique à partir d'une sauvegarde effectuée il y a un jour, et entre-temps vous vous occupez de ce qui s'est passé. Mais l'attaquant peut retrouver l'accès et continuer à voler des données.
4. Une autre solution...
