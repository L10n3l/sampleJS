**Build Travis**

[![Build Status](https://travis-ci.org/L10n3l/sampleJS.svg?branch=master)](https://travis-ci.org/L10n3l/sampleJS)

**Code coverage Coveralls**

[![Coverage Status](https://coveralls.io/repos/github/L10n3l/sampleJS/badge.svg?branch=master)](https://coveralls.io/github/L10n3l/sampleJS?branch=master)

**SonarCloud**

[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=alert_status)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=bugs)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=code_smells)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=coverage)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=duplicated_lines_density)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=ncloc)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=reliability_rating)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=security_rating)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=sqale_index)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)
[![<Sonarcloud quality gate>](https://sonarcloud.io/api/project_badges/measure?project=L10n3l_sampleJS&metric=vulnerabilities)](https://sonarcloud.io/dashboard?id=L10n3l_sampleJS)


# sampleJS

Montrer l'utilisation des badges pour les build et tests avec Travis, Coveralls et Sonar (SonarCloud).

## Prérequis (node modules pour le build et la couverture de tests)
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

Plus d'infos : [Github/Coveralls](https://github.com/nickmerwin/node-coveralls)

## Sonar

Avec Coveralls (démarche similaire à Travis)
* Se connecter à [SonarCloud](https://sonarcloud.io/)  avec les identifiants Github
* Autoriser Sonar sur Gitbub
* Sélectionner le repo à utiliser

Encrypter le token [travis](https://docs.travis-ci.com/user/encryption-keys/#usage) et le coller dans .travis.yml

```sh
apt install ruby ruby-dev
sudo gem install travis
travis encrypt <token fourni par Sonar Cloud>
```

Dans le repo, modifier le fichier .travis.yml

```sh
language: node_js
node_js:
 - "node"
dist: trusty
addons:
  sonarcloud:
    organization: "<organisation du projet, dans le cas de ce projet l10n3l>"
    token:
      secure: "<token encrypté>"
install:
  - npm install -g nyc coveralls mocha mocha-lcov-reporter 
script:
  - npm run coverage
  - sonar-scanner
```

Créer un fichier sonar-project.properties

```sh
sonar.projectKey=<pour cet exemple, L10n3l_sampleJS>
sonar.organization=<pour cet exemple, l10n3l>

# this is the name and version displayed in the SonarCloud UI.
sonar.projectName=<pour cet exemple, sampleJS>
sonar.projectVersion=1.0
 
# Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.
# This property is optional if sonar.modules is set. 
sonar.sources=.
 
# Encoding of the source code. Default is default system encoding
#sonar.sourceEncoding=UTF-8
```

## Ajout des badges dans le readme

Cf premières lignes du README.md

Pour Sonar, il existe plusieurs badges : il suffit de changer la valeur du paramètre metric => must be one of: [bugs, code_smells, coverage, duplicated_lines_density, ncloc, sqale_rating, alert_status, reliability_rating, security_rating, sqale_index, vulnerabilities]

## Fonctionnement

A chaque push dans Github, les 2 webhooks seront déclenchés : Travis et Coveralls et cela mettra à jour les badges.
Conernant Sonar, c'est le contenu du fichier .travis.yml qui déclenche l'analyse (sonar-scanner).

ATTENTION : les badges sont relatifs à une branche (si plusieurs branches alors modifier le README pour faire apparaître les badges par branche)


## ESlint

[ESLint](https://github.com/eslint/eslint)

ESLint est, comme son nom l’indique, un linter, c’est-à-dire un outil qui analyse statiquement du code et vérifie que celui-ci respecte un certain nombre de règles.

Initialisation (suppose eslint installé en global npm install eslint -g)
Répondre aux questions notamment framework react et environnement node 

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

Lancer le lint

```sh
npm run lint
``` 

