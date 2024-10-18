layout: post
title: "Amélioration des fonctionnalités du package officiel docsify-cli"
date: 2024-10-18 22:00:00 +0002
tags: node


# Amélioration des fonctionnalités du package officiel docsify-cli

Afin de répondre à un besoin en sécurité au niveau de l'interface réseau du port d'écoute, et améliorer la traçabilité des connexions sur docsify-cli, ce nouveau dépôt a été créé sur github : [webapps-conception/docsify-cli](https://github.com/webapps-conception/docsify-cli){:target="_blank"}.


## Screencast

![Screencast](https://raw.githubusercontent.com/docsifyjs/docsify-cli/master/media/screencast.gif)

> Exécution d'un serveur sur localhost avec live-reload.


## Installation

Un package est à disponible sur NPM Js sous [@webappsconception/docsify-cli](https://www.npmjs.com/package/@webappsconception/docsify-cli){:target="_blank"}.

Installez `docsify-cli` via `npm` ou `yarn` globalement.

``` bash
npm i @webappsconception/docsify-cli -g
ou
yarn global add @webappsconception/docsify-cli
```

Le mode global permet de généraliser la commande `docsify` à tous les utilisateurs de la machine.

Sous Linux, il est conseillé de l'installer avec sudo :

``` bash
sudo npm i @webappsconception/docsify-cli -g
ou
sudo yarn global add @webappsconception/docsify-cli
```


## Usage

Les améliorations apportées concernent uniquement la partie `docsify serve`.

Exécutez un serveur sur `localhost` avec livereload.

```shell
docsify serve [path] [--open false] [--ip 127.0.0.1] [--port 3000] [--log logfile]

# docsify s [path] [-o false] [-I 127.0.0.1] [-p 3000] [-l logfile]
```

- `--open` option:
  - Sténographie: `-o`
  - Type: boolean
  - Défaut: `false`
  - Description: Ouvrez la documentation dans le navigateur par défaut, la valeur par défaut est « false ». Pour définir explicitement cette option sur « false », utilisez « --no-open ».
- `--port` option:
  - Sténographie: `-p`
  - Type: number
  - Défaut: `3000`
  - Description: Choisissez un port d'écoute, la valeur par défaut est « 3000 ».
- **`--ip`** option:
  - Sténographie: **`-I`**
  - Type: number
  - Défaut: `127.0.0.1`
  - Description: Choisissez une adresse IP de l'interface d'écoute, par défaut `127.0.0.1`.
- **`--log`** option:
  - Sténographie: **`-l`**
  - Type: number
  - Description: Fichier journal.

Les options **`--ip`** et **`--log`** ont été ajoutées pour sécuriser l'interface réseau avec le paramétrage de l'adresse ip, et le fichier journal pour visualiser les connexions sur l'application `docsify-cli`.

Avec cette version de `docsify-cli`, si l'adresse ip `127.0.0.1` est utilisée par défaut, le port ne sera pas ouvert sur votre interface LAN et/ou wifi, ce qui sécurise davantages vos données par rapport à la version du dépôt officielle de `docsify-cli`.

Ces améliorations méritaient bien un nouveau dépôt github !

## Exemple de site docsify

Vous trouverez sur mon dépôt github un exemple d'utilisation de `docsify` : [webapps-conception/docsify-exemple](https://github.com/webapps-conception/docsify-exemple){:target="_blank"}.

