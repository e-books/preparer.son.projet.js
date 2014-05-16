#Les bases : Objectifs 

Je voudrais pouvoir disposer d'un modèle de projet (ou squelette) qui me permette de démarrer ma production rapidement. Et que ce modèle de projet soit "duplicable" facilement.
Je vais partir d'une webapp avec un front-end en javascript (pour l'exemple j'aurais de l'**Angular** pour préparer mes futurs articles) et un back-end node.js avec du **Express.js**, mais cela peut se décliner avec d'autres frameworks, s'utiliser uniquement avec la partie front, etc. ...

##Pré-requis

- Avoir installé node.js
- Avoir installé npm
- Pour le reste nous verrons au fur et à mesure

##Créer le squelette de projet

Créez une arborescence de projet de ce type avec (après vous pouvez adapter) un fichier `app.js` qui contiendra votre code applicatif, `index.html` votre page web, `main.js` sera le code "côté serveur".

    skeleton/
    ├── public.src/
    |   ├── js/
    |   |   └── app.js
    |   └── index.html
    └── main.js
