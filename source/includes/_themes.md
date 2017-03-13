# Créer des thèmes

## Introduction

Un thème, c’est des fichiers qui sont ajoutés à votre CMS pour transformer l'apparence de celui-ci. Différents thèmes “officiels” sont déjà disponibles sur le site de MineWeb. D’autres peuvent être développés par la communauté et c’est le but de ce tutoriel.

Il faut savoir que les thèmes sont une couche supplémentaire, c'est-à-dire que si votre thème ne s'occupe pas d'une page du site, c'est le thème par défaut qui prendra la relève.

<aside class="alert alert-info">Il est important et même presque indispensable d'avoir des connaissances en HTML/CSS voire JS et PHP pour pouvoir faire son propre thème.</aside>


## Création des dossiers & fichiers

Dans un premier temps, il va falloir créer le dossier de votre thème, ce dossier doit être créé dans le dossier `app/View/Themed/`. Le nom du dossier doit être le nom de votre thème. Par exemple si je veux créer un thème de démonstration, je vais l’appeler : Demo (évitez les accents, les espaces et les tirets, essayez d'utiliser des mots anglais pour le nom du dossier).

Une fois ce dossier créé, il faut créer différents autres dossiers et fichiers :

- `Config/`
- `Layouts/`
- `webroot/`
 - `css/`
 - `js/`
 - `img/`

C’est tout pour les dossiers, il nous reste maintenant encore des fichiers à créer tels que :

- `Config/config.json`

## Utilisation de la configuration

Pour que votre thème soit valide, celui-ci doit contenir un fichier _config.json_ dans le dossier _Config/_ du thème. Ce fichier doit contenir des informations bien précises au format JSON.

```json
{
    "name": "",
    "slug": "",
    "author": "",
    "version": "",
    "apiID": 0,
    "configurations": {},
    "supported": {}
}
```

Vous allez devoir configurer les informations les plus importantes.
Tout d'abord le _name_ qui est le nom de votre thème, vous pouvez à cet endroit écrire le nom de votre thème en français, avec des accents, espaces ou autre si vous le souhaitez. Ce nom sera affiché sur le panel admin du CMS. (exemple : Démonstration).
Après, il faut configurer le _slug_, il faut ici insérer le nom du dossier du thème précédemment choisi (exemple : Demo).
Pour la colonne _author_, mettez ici votre pseudo.
Pour la _version_ du thème, la version doit être au format X.Y.Z comme expliqué [ici](http://semver.org).
Pour _apiID_, laissez ceci à 0, ne vous en souciez pas, il sera automatiquement rempli quand vous soumettrez votre thème au market de mineweb.org.
Ne vous occupez pas non plus de _configurations_ et _supported_, nous verrons cela plus tard.

## Modifier la disposition globale du site

Nous allons dans cette étape voir comment modifier la disposition du site, tous les éléments qui seront affichés sur chaque page (head, footer...).
Pour modifier ceci, il vous faut créer un fichier _default.ctp_ dans le dossier _Layouts_.

<aside class="alert alert-info">A savoir que les fichiers de vue (modification de la structure HTML) ont tous comme extension **.ctp** et non **.html** ou **.php** mais vous pouvez quand même y insérer de l'HTML ou du PHP.</aside>

Pour le contenu du fichier _default.ctp_ vous pouvez prendre exemple sur les thèmes déjà existants ou sur le thème par défaut (le fichier _default.ctp_ se trouverait alors dans _/app/View/Layouts/default.ctp_).
Certaines parties de code contenues dans le _default.ctp_ doivent être conservées, comme par exemple ceci en fin de page :

```html
<?= $this->Html->script('app.js') ?>
<?= $this->Html->script('form.js') ?>
<script>
// Config FORM/APP.JS

var LIKE_URL = "<?= $this->Html->url(array('controller' => 'news', 'action' => 'like')) ?>";
var DISLIKE_URL = "<?= $this->Html->url(array('controller' => 'news', 'action' => 'dislike')) ?>";

var LOADING_MSG = "<?= $Lang->get('GLOBAL__LOADING') ?>";
var ERROR_MSG = "<?= $Lang->get('GLOBAL__ERROR') ?>";
var INTERNAL_ERROR_MSG = "<?= $Lang->get('ERROR__INTERNAL_ERROR') ?>";
var FORBIDDEN_ERROR_MSG = "<?= $Lang->get('ERROR__FORBIDDEN') ?>"
var SUCCESS_MSG = "<?= $Lang->get('GLOBAL__SUCCESS') ?>";

var CSRF_TOKEN = "<?= $csrfToken ?>";
</script>

<?php if(isset($google_analytics) && !empty($google_analytics)) { ?>
  <script>
    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
    (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
    m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

    ga('create', '<?= $google_analytics ?>', 'auto');
    ga('send', 'pageview');
  </script>
<?php } ?>
<?= (isset($configuration_end_code)) ? $configuration_end_code : '' ?>
```
>Vous pouvez trouver plus d'informations sur la [documentation officiel de CakePHP](http://book.cakephp.org/2.0/fr/views.html) ([Framework](https://fr.wikipedia.org/wiki/Framework) PHP utilisé pour MineWeb).


Ce code permet de faire fonctionner correctement MineWeb notamment les formulaires en AJAX ou d'afficher les paramètres configurées sur le panel admin.

## Modifier les différentes pages

Pour modifier les différentes pages du CMS il vous faudra créer un dossier avec le nom du Controller et un fichier _.ctp_ avec le nom de l'action. Si vous ne comprenez pas ou que vous ne pouvez pas vous embêter, prenez exemple sur des thèmes déjà fait (_app/View/Themed_ si vous en avez téléchargé depuis votre panel admin) ou sur le thème de base (_app/View/_).

Par exemple, pour modifier la page affichée quand vous lisez une news/un article, il vous faudra créer un fichier **News/index.ctp** et y insérer le contenu que vous souhaitez selon le modèle du thème par défaut (pour utiliser les bonnes variables).

<aside class="alert alert-info">Tout comme pour le <em>layout.ctp</em> vous pouvez vous référer à la <a href="http://book.cakephp.org/2.0/fr/views.html">documentation officielle de CakePHP</a>.</aside>

## Modifier les différentes pages d'un plugin

Pour modifier les pages d'un plugin il vous faudra tout comme à l'étape précédente créer un dossier avec le nom du Controller et un fichier avec le nom de l'action, **sauf que** il vous faudra placer ce dossier dans un autre dossier appelé _Plugin_.

<aside class="alert alert-info">Pour créer vos fichiers/dossiers, référez-vous aux plugins en eux-mêmes, situés dans <em>app/Plugin/</em>, leurs fichiers <em>.ctp</em> seront dans le dossier <em>View</em>.</aside>

Il est mieux aussi de **préciser dans la configuration** si vous _supporter_ (stylisé) des plugins. Pour cela, revenons dans le fichier _Config/config.json_ et nous allons toucher à _supported_. En effet, vous aller devoir écrire l'_ID_ du plugin et la ou les versions supportées. Par exemple :

```json
{
	...
    "supported": {
	    "eywek.shop.1": "1.0.0"
    }
}
```

Vous pouvez voir que l'_ID_ du plugin est spécial, en effet celui-ci se compose de la manière suivante : **auteur.nomdudossier.apiID**, vous pouvez donc trouver cet ID vous même facilement en regardant l'intérieur du _config.json_ d'un plugin.
Pour la version, nous avons ici mis la version du plugin, mais vous pouvez très bien la faire précéder d'un outil de comparaison. Comme ceci :

```json
{
	...
    "supported": {
	    "eywek.shop.1": ">= 1.0.0"
    }
}
```

À ce moment-là, vous indiquez que votre thème supporte uniquement les versions du plugin "Boutique" au dessus et incluant la 1.0.0.

## Utiliser des fichiers CSS, JS ou images personnalisées

Il est utile quand vous créez un thème de pouvoir utiliser des fichiers CSS/JS personnalisés ou même des images.
Pour cela, il vous suffit de créer un dossier _webroot_ contenant les sous-dossiers :

- `css`
- `js`
- `img`

C'est donc dans ces dossiers que vous glisserez vos fichiers.
Pour pouvoir les inclure facilement, il vous suffit de faire ceci dans vos fichiers _.ctp_ :

```php
<?= $this->Html->css('NomDuDossierDuTheme.votrefichier.css') ?>
```

```php
<?= $this->Html->script('NomDuDossierDuTheme.votrefichier.js') ?>
```

```php
<?= $this->Html->img('votreimage.ext', array('alt' => 'Ce que je veux')) ?>
```

>Vous pouvez avoir plus d'informations [ici](http://book.cakephp.org/2.0/fr/core-libraries/helpers/html.html).

## La personnalisation

<aside class="alert alert-warning">En cours de rédaction... (vous pouvez vous référer au thème existant comme "Universal")</aside>
