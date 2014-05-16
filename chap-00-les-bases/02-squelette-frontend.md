#Déclarer les librairies "front-end"

Pour faire une jolie webapp et dans tout faire "from scratch", nous allons avoir besoin de framework(s) javascript, de css, etc. ... Pour cela nous avons besoin d'installer **Bower** [http://bower.io/](http://bower.io/), c'est un utilitaire (par Twitter) qui permet de télécharger différents projets de frameworks javascript et css. Bower c'est un peu comme **npm** mais plutôt dédié front.

Pour l'installer une fois pour toute :

    npm install -g bower 

le paramètre `g` signifie que **Bower** sera installé de façon "globale" donc accessible de n'importe où dans vos répertoire (donc pas dans installé dans le répertoire `node_module`). *Sous Ubuntu je suis obligé de faire `sudo npm install -g bower`*.

Ensuite créez à la racine de `skeleton` un fichier `bower.json` :

    {
      "name": "skeleton",
      "version": "0.0.0",
      "dependencies": {
        "jquery" : "2.1.1",
        "bootstrap" : "3.1.1",
        "angular" : "1.2.16",
        "angular-resource" : "1.2.16"
      }
    }

Ensuite je souhaite que **Bower** me télécharge les composants (frameworks) dans le répertoire `public.src/bower_components`, donc créez, toujours à la racine de `skeleton` un fichier `.bowerrc` :

    {
      "directory": "public.src/bower_components"
    }

Et puis lancer la commande :

    bower install

Vous obtiendrez ceci :

    skeleton/
    ├── node_modules/
    |   ├── express/
    |   └── body-parser/   
    ├── public.src/   
    |   ├── bower_components/
    |   |   ├── angular/
    |   |   ├── angular-resource/
    |   |   ├── bootstrap/   
    |   |   └── jquery/             
    |   ├── js/    
    |   |   └── app.js
    |   └── index.html
    ├── main.js
    ├── package.json
    ├── bower.json       
    └── .bowerrc

Ensuite nous allons faire un tout petit peu d'Angular pour valider que tout est installé correctement.

##Modification de la page `index.html`

Modifions notre page `index.html` pour référencer nos css et frameworks javascript :

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>skeleton</title>
  <link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.min.css">
  <link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap-theme.min.css">
</head>
<body ng-app="skeletonApp">

  <hello-world ng-controller="HelloCtrl" class="container">
    <h1>{{message}}</h1>
  </hello-world>

  <script src="bower_components/angular/angular.min.js"></script>
  <script src="bower_components/angular-resource/angular-resource.min.js"></script>
  <script src="bower_components/jquery/dist/jquery.min.js"></script>
  <script src="bower_components/bootstrap/dist/js/bootstrap.min.js"></script>
  <script src="js/app.js"></script>

</body>
</html>
```

##Modification de `app.js`

Remplacer le code de `app.js` par celui-ci:

```javascript
var skeletonApp = angular.module("skeletonApp", []);

var HelloCtrl = skeletonApp.controller("HelloCtrl", function($scope) {
  $scope.message = "Hello World!";
});
```

Vous pouvez relancer `node main.js` et ouvrir [http://localhost:3000](http://localhost:3000) et vous obtiendrez un splendide **Hello World!**.

Tout fonctionne, vous pouvez commencez à coder!

Mais, mais, mais ...
