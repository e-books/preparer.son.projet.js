#"Livrer" votre application

##Le Front

###Grunt

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

###`Gruntfile.js`

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

##Le Back

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

*Stay tuned for the next episode ...*