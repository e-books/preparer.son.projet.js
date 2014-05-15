#Préliminaires

J'ai prévu d'écrire pas mal d'articles et de tutos sur différents outils et frameworks, notamment autour de javascript. Que ce soit pour du tuto, de la démo, du POC ou de la réalisation "pour de vrai", le besoin d'industrialiser ses projets javascript, ne serait ce que pour éviter les tâches répétitives, mais aussi pour faire plus "propre" et se faciliter la vie, se fait sentir rapidement.
Donc fini les downloads de librairies à la main, l'absence de gestion de version, etc. ...
Sans vouloir utiliser toute la batterie d'outils disponibles (on ne peut pas tout connaître, et au bout d'un moment c'est improductif), il y a un minimum syndical que l'on peut mettre en œuvre sans trop d'efforts pour des apports significatifs.

##Pourquoi ce tuto?

Je m'aperçois autour de moi (mais même moi) que finalement à part les "purs fronts", les développeurs utilisent très peu les outils d'intégration du monde javascript à leur disposition, voire même ne les connaissent pas.

##Objectifs 

Je voudrais pouvoir disposer d'un modèle de projet (ou squelette) qui me permette de démarrer ma production rapidement. Et que ce modèle de projet soit "duplicable" facilement.
Je vais partir d'une webapp avec un front-end en javascript (pour l'exemple j'aurais de l'**Angular** pour préparer mes futurs articles) et un back-end node.js avec du **Express.js**, mais cela peut se décliner avec d'autres frameworks, s'utiliser uniquement avec la partie front, etc. ...

##Pré-requis

- Avoir installé node.js
- Avoir installé npm
- le reste nous verrons au fur et à mesure

##Créer le squelette de projet

Créez une arborescence de projet de ce type avec (après vous pouvez adapter) un fichier `app.js` qui contiendra votre code applicatif, `index.html` votre page web, `main.js` sera le code "côté serveur".

    skeleton/
    ├── public.src/
    |   ├── js/
    |   |   └── app.js
    |   └── index.html
    └── main.js

###Déclarer les librairies "back-end"

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

####Un bout de code serveur pour vérifier que tout fonctionne

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

####Deux bouts de codes clients pour vérifier que tout fonctionne

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

####Vérification

Il suffit de lancer :

    node main.js

Puis d'ouvrir dans votre navigateur :

- [http://localhos:3000](http://localhos:3000) qui vous chargera la page `index.html` avec le message "Hello World!"
- [http://localhos:3000/hello](http://localhos:3000/hello) qui va appeler notre route express `hello` et qui nous affichera un "gros" **Hello**.

Rien de transcendant, mais ça a le mérite de nous permettre de vérifier que nous avons bien tout installer correctement.

####Remarque avant de passer à la suite

Nul besoin de vous "trimbaler" le répertoire `node_module` avec tout son contenu, vous pourrez grâce à `package.json` tout re-télécharger. A moins de savoir à l'avance que vous allez faire une démo dans un endroit sans connexion web.

###Déclarer les librairies "front-end"

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

####Modification de la page `index.html`

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

####Modification de `app.js`

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

##"Livrer"

###Le Front

####Grunt

Je ne sais pas si vous avez remarqué, (si ce n'est pas le cas allez vérifier), mais **Bower** ne se contente pas de télécharger simplement la librairie javascript ou la feuille de styles css, il ramène "tout le monde", c'est à dire l'ensemble des codes sources, documentation, tests, ... Et nous n'avons pas besoin de tout ça pour que notre application fonctionne en production. Donc vous avez la solution "à la main" ou la solution "industrialisée" avec **Grunt**, un lanceur de tâches [http://gruntjs.com/](http://gruntjs.com/).

Commençons par installer **Grunt** de manière global (accessible partout comme pour Bower). 

    npm install -g grunt

*Sous Ubuntu, je suis obligé de pré fixé par un `sudo`*.

Ensuite dans le fichier `package.json` ajoutez une nouvelle dépendance, le plugin grunt "grunt-contrib-copy" [https://github.com/gruntjs/grunt-contrib-copy](https://github.com/gruntjs/grunt-contrib-copy) qui sert à copier des fichiers d'une destination vers une autre.

    {
      "name": "skeleton",
      "description" : "skeleton project",
      "version": "0.0.0",
      "dependencies": {
        "express": "4.1.x",
        "body-parser": "1.0.2",
        "grunt-contrib-copy": "0.5.0"
      }
    }

et lancez un :

    npm update

*un `npm install` aurait aussi fonctionné*

Vous obtenez donc 2 nouveaux répertoires dans `node_modules` :

    skeleton/
    ├── node_modules/
    |   ├── express/
    |   ├── body-parser/         
    |   ├── grunt/
    |   └── grunt-contrib-copy/   

####`Gruntfile.js`

Ensuite créez un fichier `Gruntfile.js` à la racine de `skeleton` avec le contenu suivant :

```javascript
module.exports = function(grunt) {

  grunt.initConfig({
    copy: {
      main: { 
        files:[
            {expand: true, cwd: "public.src/bower_components/bootstrap/dist", src: ['fonts/**'], dest: 'public/js/vendors/bootstrap'}
          , {expand: true, cwd: "public.src/bower_components/bootstrap/dist/", src: ['js/bootstrap.min.js'], dest: 'public/js/vendors/bootstrap'}
          , {expand: true, cwd: "public.src/bower_components/bootstrap/dist/", src: ['css/bootstrap.min.css'], dest: 'public/js/vendors/bootstrap'}
          , {expand: true, cwd: "public.src/bower_components/bootstrap/dist/", src: ['css/bootstrap-theme.min.css'], dest: 'public/js/vendors/bootstrap'}
          , {expand: true, cwd: "public.src", src: ['js/**'], dest: 'public'}
          , {expand: true, cwd: "public.src/bower_components/jquery/dist/", src: ['jquery.min.js'], dest: 'public/js/vendors'}
          , {expand: true, cwd: "public.src/bower_components/angular/", src: ['angular.min.js'], dest: 'public/js/vendors'}
          , {expand: true, cwd: "public.src/bower_components/angular-resource/", src: ['angular-resource.min.js'], dest: 'public/js/vendors'}
        ]
      },
      index : {
        expand: true, cwd: "public.src", src: ['index.html'], dest: 'public',
        options: {
          process: function (content, path) {
            return content
              .replace(/bower_components/g, "js/vendors")
              .replace(/bootstrap\/dist/g,"bootstrap")
              .replace(/jquery\/dist\//g,"")
              .replace(/angular\//g,"")
              .replace(/angular-resource\//g,"")
          }
        }
      }
    }
  });

  grunt.loadNpmTasks('grunt-contrib-copy');
}
```

Dans ce fichier, je demande à **Grunt** de copier les fichiers qui sont dans `public.src/bower_components` dans le répertoire `public/js/vendors` avec la tâche `main`, je déplace aussi tous les fichiers présents dans `public.src/js` vers `public/js`, ensuite avec la tâche `index`, je demande à **Grunt** de déplacer `index.html` dans `public` mais aussi d'en modifier le contenu car j'ai changé les chemins des ressources en les déplaçant.

Ensuite j'enregistre ma tâche : `grunt.loadNpmTasks('grunt-contrib-copy')`

Et maintenant, dès que je veux livrer, je n'ai plus qu'à lancer la commande: 

    grunt copy

Et vous obtiendrez l'arborescence suivante:

    skeleton/
    ├── node_modules/
    |   ├── express/
    |   └── body-parser/  
    ├── public/              
    |   ├── js/ 
    |   |   ├── vendors/  
    |   |   |   ├── angular.min.js
    |   |   |   ├── angular-resource.min.js
    |   |   |   ├── bootstrap/
    |   |   |   |   ├── css/
    |   |   |   |   ├── fonts/
    |   |   |   |   └── js/ 
    |   |   |   └── jquery.min.js              
    |   |   └── app.js
    |   └── index.html     
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

###Le Back

Si vous vous souvenez, dans le code serveur (`main.js`), on définit l'emplacement des fichiers statiques de la façon suivante:

```javascript
app.use(express.static(__dirname + '/public.src'));
```

Et l'on voudrait pouvoir facilement tester la version de développement `/public.src` et la version de "prod" `/public`. Pour cela modifier le code de `main.js` de la façon suivante :

Remplacez:

```javascript
app.use(express.static(__dirname + '/public.src'));
```

Par:

```javascript
if (process.env.NODE_ENV == "dev") {
  app.use(express.static(__dirname + '/public.src')); 
} else {
  app.use(express.static(__dirname + '/public'));  
}
```

Et maintenant si vous voulez tester le front "développement", vous lancez node de cette manière :

    NODE_ENV=dev node main.js

Et pour la "production" on reste sur :

    node main.js

Voilà, c'est terminé pour cette partie.
Alors c'est une base. Cela peut être fait différemment, s'améliorer, etc. ... J'attends vos propositions :)

Vous pourrez trouver les code source ici [https://github.com/web-stacks/js-quick-start](https://github.com/web-stacks/js-quick-start).




