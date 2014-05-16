#Déclarer les librairies "back-end"

A la racine de skeleton, créer un fichier `package.json` :

    skeleton/
    ├── public.src/
    |   ├── js/
    |   |   └── app.js
    |   └── index.html
    ├── main.js
    └── package.json

Avec le contenu suivant :

    {
      "name": "skeleton",
      "description" : "skeleton project",
      "version": "0.0.0",
      "dependencies": {
        "express": "4.1.x",
        "body-parser": "1.0.2"
      }
    }

Nous venons de définir quelles étaient les librairies dont nous avions besoin côté serveur. Maintenant pour les télécharger/installer c'est simple, tapez dans un terminal, la commande :

    npm install

En fonction de votre bande passante, cela prendra plus ou moins de temps, mais **npm** va télécharger les dépendances dans un répertoire `node_modules`, vous devriez donc avoir l'arborescence suivante :

    skeleton/
    ├── node_modules/
    |   ├── express/
    |   └── body-parser/   
    ├── public.src/
    |   ├── js/
    |   |   └── app.js
    |   └── index.html
    ├── main.js
    └── package.json

Vous noterez que vous pouvez définir dans le fichier `package.json` les versions dont vous avez besoin et les figer. Cela peut vous éviter de mauvaises surprises lors d'un changement de version de framework.

##Un bout de code serveur pour vérifier que tout fonctionne

Dans le fichier `main.js`, saisissez ceci :

```javascript
var express = require('express')
  , http = require('http')
  , bodyParser = require('body-parser')
  , app = express()
  , http_port = 3000;

app.use(express.static(__dirname + '/public.src'));
app.use(bodyParser());

app.get("/hello", function(req, res) {
  res.send("<h1>Hello</h1>");
});

app.listen(http_port);
console.log("Listening on " + http_port);
```

##Deux bouts de codes clients pour vérifier que tout fonctionne

Dans le fichier `app.js`, saisissez ceci :

```javascript
document.querySelector("hello").innerHTML = "Hello World!";
```

Dans le fichier `index.html`, saisissez ceci :

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>skeleton</title>
</head>
<body>
  <!-- Note de l'auteur
    certains n'aiment pas les tags personnalisés
    à l'approche des WebComponents, je pense qu'il faut commencer
    à penser et apprendre à bosser ensemble autrement
  -->
  <hello></hello>
  <script src="js/app.js"></script>

</body>
</html>
```

##Vérification

Il suffit de lancer :

    node main.js

Puis d'ouvrir dans votre navigateur :

- [http://localhos:3000](http://localhos:3000) qui vous chargera la page `index.html` avec le message "Hello World!"
- [http://localhos:3000/hello](http://localhos:3000/hello) qui va appeler notre route express `hello` et qui nous affichera un "gros" **Hello**.

Rien de transcendant, mais ça a le mérite de nous permettre de vérifier que nous avons bien tout installer correctement.

##Remarque avant de passer à la suite

Nul besoin de vous "trimbaler" le répertoire `node_module` avec tout son contenu, vous pourrez grâce à `package.json` tout re-télécharger. A moins de savoir à l'avance que vous allez faire une démo dans un endroit sans connexion web.
