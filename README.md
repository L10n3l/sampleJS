[![Build Status](https://travis-ci.org/L10n3l/sampleJS.svg?branch=master)](https://travis-ci.org/L10n3l/sampleJS)
[![Coverage Status](https://coveralls.io/repos/github/L10n3l/sampleJS/badge.svg?branch=master)](https://coveralls.io/github/L10n3l/sampleJS?branch=master)

# sampleJS

## Prerequis (node modules pour le build et la couverture de tests)
* mocha
* mocha-lcov-reporter
* nyc
* coveralls

```sh
npm install mocha mocha-lcov-reporter nyc coveralls --save-dev
```


## Badge build

Avec Travis CI
* Se connecter [Travis](https://travis-ci.org/) avec les identifiants Github
* Autoriser Travis sur Github
* Dans les settings, activer le projet Github

Un webhook sera créé dans Github (cf settings > webhook) 

Dans le repo, ajouter un fichier .travis.yml à la racine avec à minima

```sh
language: node_js
node_js:
 - "node"
```

Plus d'infos [Tuto Github - Travis](https://www.vogella.com/tutorials/TravisCi/article.html)


## Badge code coverage

Avec Coveralls (démarche similaire à Travis)
* Se connecter à [Coveralls](https://coveralls.io/)  avec les identifiants Github
* Autoriser Coveralls sur Gitbub
* Sélectionner le repo à utiliser

Un webhook sera créé dans Github (cf settings > webhook) 

Dans le repo, modifier le package.json

```sh
"scripts": {
    "test": "mocha",
    "coverage": "nyc npm test && nyc report --reporter=text-lcov | coveralls"
  }
```

Dans le repo, modifier le fichier .travis.yml

```sh
language: node_js
node_js:
 - "node"
install:
  - npm install -g nyc coveralls mocha mocha-lcov-reporter 
script:
  - npm run coverage
```

## Ajout des badges dans le readme

Cf 2 premières lignes du README.md

## Fonctionnement

A chaque push dans Github, les 2 webhooks seront déclenchés : Travis et Coveralls et cela mettra à jour les badges.

ATTENTION : les badges sont relatifs à une branche (si plusieurs branches alors modifier le README pour faire apparaître les badges par branche)

## Sonar

Avec Coveralls (démarche similaire à Travis)
* Se connecter à [SonarCloud](https://sonarcloud.io/)  avec les identifiants Github
* Autoriser Coveralls sur Gitbub
* Sélectionner le repo à utiliser

Dans le repo, modifier le fichier .travis.yml

```sh
language: node_js
node_js:
 - "node"
install:
  - npm install -g nyc coveralls mocha mocha-lcov-reporter 
script:
  - npm run coverage
  - sonar-scanner
```



## ESlint

[ESLint](https://github.com/eslint/eslint)

ESLint est, comme son nom l’indique, un linter, c’est-à-dire un outil qui analyse statiquement du code et vérifie que celui-ci respecte un certain nombre de règles.

Initialisation (suppose eslint installé en global npm install eslint -g)
Répondre aux question notamment framework react et environnement node 

```sh
eslint --init
```

Pour supprimer les messages (jest) du type
 
```sh
  3:1  error  'describe' is not defined  no-undef
  4:5  error  'describe' is not defined  no-undef
  5:9  error  'it' is not defined        no-undef
```

ajouter

```sh
 "env": {
        "jest": true
      }
```

Dans le repo, modifier le fichier package.json

```sh
"scripts": {
    "lint": "eslint . --ext .js",
    "test": "mocha",
    "coverage": "nyc npm test && nyc report --reporter=text-lcov | coveralls"
  }
```
