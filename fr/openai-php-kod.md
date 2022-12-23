Que faire lorsque l'intelligence artificielle supprimera les emplois de programmeurs l'année prochaine ?
========================================================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	fr: que-faire-lorsque-l-intelligence-artificielle-supprimera-les-emplois-de-programmeurs-l-annee-prochaine
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

Les possibilités du modèle linguistique OpeanAI sont incroyables. Il suffit de saisir le code source, de demander son analyse, sa correction et sa réécriture dans un autre langage, et tout se fait automatiquement.

Lorsque j'ai essayé pour la première fois, j'ai été choqué par ce que l'on peut réaliser et par la puissance des outils mis à la disposition du grand public.

Prenons un exemple :

**Question:** Trouvez la vulnérabilité de sécurité dans cette fonction, et fournissez le code corrigé :

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Janvier',
		'Février',
		'Mars',
		'Avril',
		'Mai',
		'Juin',
		'Juillet',
		'Août',
		'Septembre',
		'Octobre',
		'Novembre',
		'Décembre',
	];

	return $czMonths[(int) date('m')];
}
```

**Réponse:**

L'une des failles de sécurité de cette fonctionnalité est le danger d'exploitation d'une attaque par injection SQL. Lorsque des données mal formatées ou malveillantes sont transmises à la fonction, la base de données peut être compromise ou exploitée.

Une solution consiste à utiliser des constantes de mois prédéfinies au lieu des données obtenues par un appel à la fonction date(). Nous pouvons également utiliser la fonction filter_var() avec un filtre approprié pour nous assurer que l'entrée est numérique et ne contient pas de code malveillant.

Voici le code corrigé :

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Janvier',
		'Février',
		'Mars',
		'Avril',
		'Mai',
		'Juin',
		'Juillet',
		'Août',
		'Septembre',
		'Octobre',
		'Novembre',
		'Décembre',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Question:** Convertir l'implémentation de la fonction en typecript.

**Réponse:**

Voici l'implémentation réécrite de la fonction en TypeScript :

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if (month === false || month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

**Request:** Ceci n'est pas un typecript valide !

**Réponse:**

Je m'excuse pour l'erreur. Voici la version corrigée de la fonction en TypeScript :

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = Number(new Date().getMonth()) + 1;

	if (month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

Note : Il n'y a pas de fonction filter_var() en TypeScript, donc je l'ai remplacée en appelant la fonction getMonth() de la classe Date et en la convertissant en nombre avec la fonction Number(). Cette version de la fonction devrait être valide en TypeScript.

**Question:** Votre implémentation de Typescript contient-elle une vulnérabilité de sécurité ?

**Réponse:**

Mon implémentation corrigée de la fonction en TypeScript ne devrait pas contenir la vulnérabilité de sécurité de l'attaque par injection SQL qui était initialement présente dans l'implémentation PHP originale. La fonction getMonth() de la classe Date renvoie une valeur numérique du mois (0 pour janvier, 1 pour février, etc.), de sorte que nous n'avons pas besoin de passer l'entrée de la fonction date() et pouvons éviter le danger de l'attaque par injection SQL.

Cependant, toute application peut contenir des vulnérabilités de sécurité, il est donc important d'examiner périodiquement le code pour rechercher des problèmes de sécurité potentiels. Si vous soupçonnez une faille de sécurité dans votre code, vous pouvez contacter des experts en sécurité ou utiliser des outils automatisés d'analyse des failles de sécurité.
