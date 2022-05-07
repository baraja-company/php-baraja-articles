Traitement des requêtes POST ajax en PHP
========================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	fr: traitement-des-requetes-post-ajax-en-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

En développant des applications ajax Vue.js, après des années, j'ai finalement découvert comment utiliser ajax en PHP et comment recevoir des données par la méthode POST.

La variable superglobale `$_POST` n'est disponible que pour les formulaires
-------------------------------------------------------------

En PHP, la <a href="/superglobal-variable">variable superglobale `$_POST`</a> est couramment disponible pour contenir les données soumises par un formulaire.

Il est **relativement** facile à utiliser.

Du côté HTML, vous devez créer un formulaire :

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

Et ensuite, dans le fichier `process.php`, les valeurs peuvent être accédées comme des éléments de tableau :

```php
echo htmlspecialchars($_POST['nom d'utilisateur'] ?? '');
```

> **Attention:**
>
> Avec cette approche simple, tout le monde pourrait penser que les données POSTed sont automatiquement définies comme des index de tableau dans la variable `$_POST`. Mais ce n'est pas vrai !

En fait, la raison pour laquelle les données envoyées par un formulaire utilisant la méthode POST sont écrites dans la variable `$_POST` est que le navigateur envoie automatiquement l'en-tête HTTP `'Content-Type' : 'application/x-www-form-urlencoded'` lorsque le formulaire HTML est soumis.

Sans un en-tête correctement défini, il est tout simplement impossible d'accéder aux valeurs et nous devons recourir à une solution de fortune.

Envoi de données par ajax
-------------------

Lorsque nous essayons d'envoyer des données à l'aide d'ajax, nous devons changer un peu l'approche du côté de PHP. Vous pouvez <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">lire la discussion sur Facebook</a> pour plus de détails.

En javascript, par exemple, vous pouvez utiliser la bibliothèque <a href="https://github.com/axios/axios">axios</a> pour envoyer des données par ajax. Pour l'utiliser facilement, il suffit de lier le javascript du serveur CDN et de l'utiliser immédiatement :

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    axios.post('/api/form-process', {
        username: 'Jméno uživatele'
    })
    .then(response => {
        // Zpracování odpovědi z API
        alert(response.data.message); // Vyhodí hlášku se zprávou
    });
</script>
```

Dans ce cas simple, l'URL `'/api/form-process'` est appelée par ajax et la méthode POST transmet l'objet `{username : 'User name' }`. La bibliothèque elle-même gère déjà la logistique du transfert automatique des données, qui sont donc envoyées sérialisées en Json. Dans le langage des développeurs frontaux, cela s'appelle **json payload**.

Du côté de PHP, je m'attendrais à l'utiliser comme un formulaire (après tout, il s'agit d'une méthode POST) :

```php
echo htmlspecialchars($_POST['nom d'utilisateur'] ?? '');
```

Cependant, dans ce cas, le champ `$_POST` sera vide et aucune donnée ne sera transmise. La variable `$_POST` n'est utilisée que pour les données récupérées à partir de formulaires (vous pouvez le dire par l'en-tête HTTP que nous n'avons pas jeté).

Donc, dans ce cas, nous devons obtenir les données directement à partir de la requête HTTP, la **solution astucieuse** est utilisée pour cela. Il est ensuite facile à utiliser :

```php
$data = json_decode(file_get_contents('php://entrée'), true);

echo htmlspecialchars($data['nom d'utilisateur'] ?? '');

header('Content-Type : application/json');
echo json_encode([
    'message' => 'La liane du serveur',
]);
die;
```

À titre d'exemple, je fournis également une réponse simple en javascript. L'important est de lancer correctement l'en-tête HTTP `'Content-Type : application/json'` et de quitter le script après l'envoi de toutes les données.

Appliquer l'utilisation de `$_POST`.
-------------------------

Si vous souhaitez toujours traiter les données soumises directement comme un formulaire, il existe un moyen de les transférer. Dans ce cas, vous devez modifier la création de la requête ajax elle-même et passer les en-têtes HTTP correctement :

```js
axios.post(
    '/api/form-process',
    {
        username: 'Jméno uživatele'
    },
    {
        headers: {'Content-Type': 'application/x-www-form-urlencoded'}
    }
).then(response => {
    // Nějaké zpracování
});
```

Dans ce cas, cependant, le traitement du côté PHP ne sera pas agréable, car nous devons encore corriger les données et les convertir en tableau.

J'ai réussi à trouver cette horreur :

```php
if (\count($_POST) === 1
    && preg_match('/^\{.*\}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['nom d'utilisateur'] ?? '');
```

Toutefois, il est préférable de s'en tenir à la première approche et d'utiliser la méthode "php://input" pour obtenir les données.
