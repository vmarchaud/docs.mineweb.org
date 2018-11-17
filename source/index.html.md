---
title: MineWeb docs

language_tabs:
  - php

toc_footers:
  - <a href='https://mineweb.org'>Retour sur mineweb.org</a>
  - <a href='https://github.com/tripit/slate'>Documentation powered by Slate</a>

includes:
  - setup
  - link_server
  - th_pl
  - plugins
  - themes
  - copyright

search: true
---

# Introduction

## C'est quoi MineWeb ?

MineWeb est un **CMS**. Un CMS est un système de gestion de contenu. Plus précisément il vous permet de vous créer rapidement et facilement un site complètement personnalisable. MineWeb est la version 2 de l'ancien CMS **LapisCraft** de Eywek. Celui-ci n'étant plus stable et mal développé (cf. raisons [ici](https://eywek.fr/lc-explications.pdf)).

MineWeb est développé sous un framework PHP nommé **CakePHP** permettant un développement plus **rapide**, **sécurisé** et **optimisé**. Le projet a été lancé en 2015 en compagnie de **Mac'** permettant de convenir entièrement à **vos besoins**.

## Pré-requis

Pour installer le CMS MineWeb votre hébergeur **doit** avoir :

*   Version PHP supérieure ou égale à 5.6 et inférieure à 7.2 (php 7.2 ne pourra pas être compatible avec le CMS)
*   PDO activé
*   cURL activé
*   Réécriture d'URL
*   .htaccess présent à la racine du CMS
*   La librairie GD2
*   La possibilité d'ouvrir un zip
*   La possibilité d'ouvrir un site à distance
*   ionCube Loader
*   OpenSSL

*   Falcutatif - Php-xml

Pour plus de simplicité vous pouvez télécharger le **fichier de compatibilité** [ici](https://docs.mineweb.org/files/compatibilite.zip). Vous avez juste à extraire cette archive sur votre FTP pour voir si votre hébergeur est compatible.

## Erreur 500

Pour corriger l'erreur 500 sur votre cms faites <strong>toutes</strong> les étapes :
*   Supprimez le contenu du fichier **"/app/tmp/"**
*   Si ca ne marche toujours pas : 
    *   Réeffectuez la manipulation qui a provoqué l'erreur
    *   Mettez en ligne sur Pastebin le fichier **"/app/tmp/logs/error.log"** afin de donner le lien au support dans le chat du [Discord](https://discordapp.com/invite/3QYdt8r)


Pour éviter les confusions ou éviter toute erreur de votre part, merci de bien vouloir lire toute la documentation avant de demander de l'aide au support et pour être plus efficace.
