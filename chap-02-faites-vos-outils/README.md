#Faites vos outils avec npm et node

Lorsque je code, j'ai souvent des tâches répétitives (quel que soit le langage), ie: 

- écrire des modèles et des collections Backbone
- écrire des modèles Play
- écrire des routes pour Express.js
- ...

Nul doute, que vous avez énormément d'autres exemples en tête. L'outil du moment pour régler ce type de problème est [Yeoman](http://yeoman.io/) (oui il y en a d'autres, mais Yeoman c'est "hype" ;)) et ensuite, ou vous avez de la chance, ou il faut écrire le plugin dont vous avez besoin. Dans certains cas, surtout si vous n'avez pas besoin de quelque chose de complexe, vous pouvez rapidement écrire un utilitaire avec uniquemnt node et npm.

##Spécifications (par exemple)

Je passe mon temps à écrire des modèles et des collections Backbone qui ressemblent à ceci :

```javascript
/*--- Human Model ---*/
var HumanModel = Backbone.Model.extend({
    defaults : function (){
      return {}
    },
    urlRoot : "humans"
});

/*--- Humans Collection ---*/
var HumansCollection = Backbone.Collection.extend({
  url : "humans",
  model: HumanModel
});
```

Et j'aimerais aller plus vite, juste taper `bb Human` et obtenir mon fichier généré.