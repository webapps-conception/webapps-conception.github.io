layout: post
title: 'Créer un site web avec Gatsby'
date: '2022-11-15T22:00:00.000Z'
tags: react gatsby

# Site Gatsby Starter de Web Apps Conception
* <https://github.com/webapps-conception/gatsby-starter-default>{:target="_blank"}

# Modèles de sites web
* <https://www.gatsbyjs.com/starters/>{:target="_blank"}

# Créer un blog
## Introduction

Au cours de ce didacticiel, vous allez créer et déployer votre premier site Gatsby : un site de blog prenant en charge les images et MDX ! (Si cela ne signifie rien pour vous maintenant, ce n'est pas grave ! Cela le sera au moment où vous atteindrez la fin.) Voici un exemple fini du site que vous allez créer. Vous pouvez également suivre le référentiel GitHub pour l'exemple fini.

Dans cette partie du didacticiel, vous passerez par le processus de création du modèle de votre site de blog et de son déploiement en ligne pour que tout le monde puisse le voir.

Le diagramme ci-dessous montre une vue d'ensemble de la façon dont tous les éléments de ce processus s'imbriquent.

![Workflow d'un déploiement Gatsby](/assets/images/gatsby-deployment-workflow.png)

Tout d'abord, vous écrivez le code de votre site Gatsby à partir de votre ordinateur. Lorsque vous êtes prêt à mettre vos modifications en ligne sur Internet, vous suivez les étapes suivantes :
* Vous transférez vos modifications de votre ordinateur vers un référentiel distant sur GitHub. GitHub est une plateforme en ligne pour stocker le code de vos projets.
* Gatsby Cloud surveille votre référentiel GitHub pour les modifications. Lorsqu'il voit vos nouvelles modifications, Gatsby Cloud construit votre site à partir de votre code sur GitHub.
* Gatsby Cloud héberge la version finale de votre site sur une URL unique, que les utilisateurs peuvent utiliser pour accéder à la dernière version de votre site.

## Installation

Pour utiliser la CLI Gatsby, vous devez installer globalement ``gatsby-cli`` :

```shell
sudo npm install -g gatsby-cli
```

## Création d'un nouveau site

```shell
gatsby new
```

## Cloner une site directement depuis GitHub

Vous pouvez également ignorer l'invite et cloner un démarreur directement depuis GitHub. Par exemple, pour cloner un nouveau blog gatsby-starter, exécutez :

```shell
gatsby new my-new-blog https://github.com/gatsbyjs/gatsby-starter-blog
```

## Executer le serveur de développement

Depuis votre répertoire de site, démarrez le serveur de développement en exécutant la commande suivante :

```shell
gatsby develop
```

## Configurer un référentiel GitHub pour votre site

GitHub est un site Web que de nombreux développeurs utilisent pour sauvegarder et partager leur code en ligne. En téléchargeant votre code sur GitHub, vous pourrez travailler sur la même base de code à partir de plusieurs ordinateurs. Vous pourrez également utiliser Gatsby Cloud pour créer et héberger votre site.

1. Chaque base de code sur GitHub est stockée dans son propre référentiel (également appelé « référentiel », en abrégé). Pour créer un nouveau référentiel pour votre blog, cliquez sur l'icône plus dans le coin supérieur droit de la barre de navigation. Sélectionnez "Nouveau référentiel".
2. Lorsque vous remplissez le formulaire pour votre nouveau dépôt, vous pouvez le rendre public ou privé. (Cela n'affecte que la visibilité de votre code sur GitHub. Votre site sera toujours visible par tous une fois que vous l'aurez déployé avec Gatsby Cloud.) Laissez les cases d'option d'initialisation décochées.
3. Pour transférer votre code existant de votre ordinateur vers votre nouveau référentiel GitHub, entrez les commandes ci-dessous dans la ligne de commande. Assurez-vous de remplacer ``YOUR_GITHUB_USERNAME`` par votre nom d'utilisateur réel et ``YOUR_GITHUB_REPO_NAME`` par le nom que vous avez donné à votre dépôt GitHub (comme ``my-first-gatsby-site``).

```shell
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/YOUR_GITHUB_REPO_NAME.git
git branch -M main
git push -u origin main
```

Vous avez maintenant une copie de votre code enregistré sur les serveurs de GitHub. À l'étape suivante, vous connecterez votre compte Gatsby Cloud au référentiel GitHub que vous venez de créer.

## Créez votre site avec Gatsby Cloud

Gatsby Cloud est une plate-forme d'infrastructure spécialement optimisée pour la création, le déploiement et l'hébergement de sites Gatsby. Une fois que vous aurez connecté votre compte Gatsby Cloud à votre référentiel GitHub, Gatsby Cloud créera votre site et le rendra accessible aux autres sur Internet.

Pour connecter votre code sur GitHub à votre compte Gatsby Cloud, procédez comme suit :
1. Accédez à votre [tableau de bord Gatsby Cloud](https://www.gatsbyjs.com/dashboard/){:target="_blank"}. Cliquez sur le bouton **"Ajouter un site"**.
2. Les prochaines étapes vous aideront à ajouter votre site à Gatsby Cloud. Tout d'abord, dans la carte **"Importer depuis un référentiel Git"**, cliquez sur l'icône **"GitHub"** pour sélectionner GitHub comme fournisseur Git.
3. Si c'est la première fois que vous connectez GitHub à Gatsby Cloud, vous devrez autoriser Gatsby Cloud à accéder à votre compte GitHub.
4. Une nouvelle fenêtre de navigateur devrait s'ouvrir, où GitHub vous demandera si vous souhaitez autoriser Gatsby Cloud à vos référentiels GitHub. Vous pouvez choisir de donner à Gatsby Cloud l'accès à tous vos référentiels GitHub ou uniquement au référentiel que vous avez créé (``my-first-gatsby-site``). Cliquez ensuite sur **"Installer"**.
5. Maintenant, lorsque vous revenez à la fenêtre Gatsby Cloud, la liste des référentiels doit contenir votre référentiel GitHub. Sélectionnez-le en cliquant sur **"Importer"**.
6. Une fois que vous avez sélectionné votre dépôt, vous serez dirigé vers l'étape de configuration qui vous présente quelques entrées supplémentaires. Ceux-ci vous permettent d'indiquer à Gatsby Cloud où chercher dans votre référentiel GitHub pour votre site Gatsby. Vous pouvez également modifier le nom que Gatsby Cloud donnera à votre site. Laissez les **paramètres par défaut** et cliquez sur le bouton **"Suivant"**.
7. Gatsby Cloud vous demandera si vous souhaitez ajouter des intégrations à votre site. Pour de futurs projets, cela pourrait être utile si vous souhaitez utiliser un CMS. Gatsby Cloud vous demandera également si vous souhaitez ajouter des variables d'environnement. Encore une fois, cela peut être utile pour de futurs projets, mais pour l'instant, faites défiler et cliquez sur le bouton **"Build Site"**.
8. Maintenant que votre site a été créé, vous serez redirigé vers un tableau de bord du site où vous pourrez voir l'état de vos builds. Gatsby Cloud devrait commencer à créer votre site automatiquement. Vous verrez un lien vers votre nouveau site, qui est automatiquement hébergé sur Gatsby Cloud. Vous pouvez partager ce lien avec n'importe qui, et ils pourront voir votre site en ligne !

Votre site Gatsby est désormais en ligne !

Chaque fois que vous apportez une nouvelle modification à la branche principale de votre référentiel GitHub, Gatsby Cloud verra les modifications et démarrera automatiquement une construction pour la nouvelle version de votre site.

## Sources
* <https://www.gatsbyjs.com>{:target="_blank"}
* <https://www.gatsbyjs.com/docs/tutorial/part-1/>{:target="_blank"}
