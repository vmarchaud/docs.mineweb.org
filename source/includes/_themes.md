# Créer des thèmes

## Introduction

Un thème, c’est des fichiers qui sont ajoutés à votre CMS pour transformer l'apparence de celui-ci. Différents thèmes “officiels” sont déjà disponibles [ici](https://github.com/MineWeb?utf8=%E2%9C%93&q=Theme-&type=&language=). D’autres peuvent être développés par la communauté et c’est le but de ce tutoriel.

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
    "slider": false,
    "configurations": {},
    "supported": {}
}
```

Vous allez devoir configurer les informations les plus importantes.
Tout d'abord le _name_ qui est le nom de votre thème, vous pouvez à cet endroit écrire le nom de votre thème en français, avec des accents, espaces ou autre si vous le souhaitez. Ce nom sera affiché sur le panel admin du CMS. (exemple : Démonstration).

Après, il faut configurer le _slug_, il faut ici insérer le nom du dossier du thème précédemment choisi (exemple : Demo).

Pour la colonne _author_, mettez ici votre pseudo.

Pour la _version_ du thème, la version doit être au format X.Y.Z comme expliqué [ici](http://semver.org).

Pour _slider_, mettez **true** ou **false** selon si vous souhaitez ou non utiliser le système de slider prévu par le CMS _(cette clé permet d'afficher ou non le menu slider dans le panel admin)_.

Ne vous occupez pas non plus de _configurations_ et _supported_, nous verrons cela plus tard.

## Modifier la disposition globale du site

Nous allons dans cette étape voir comment modifier la disposition du site, tous les éléments qui seront affichés sur chaque page (head, footer...).
Pour modifier ceci, il vous faut créer un fichier _default.ctp_ dans le dossier _Layouts_.

<aside class="alert alert-info">A savoir que les fichiers de vue (modification de la structure HTML) ont tous comme extension **.ctp** et non **.html** ou **.php** mais vous pouvez quand même y insérer de l'HTML ou du PHP.</aside>

Pour le contenu du fichier _default.ctp_ vous pouvez prendre exemple sur les thèmes déjà existants ou sur le thème par défaut (le fichier _default.ctp_ se trouverait alors dans _/app/View/Layouts/default.ctp_).

Dans ce fichier, on repère plusieurs grandes parties:

 - L'entête
 - La barre de navigation
 - Le contenu
 - Le footer
 - Les scripts

### L'entête

Elle constitue les données nécessaires au chargement de la page. L'entête doit contenir tous les liens vers les fichiers CSS de votre thème, le titre de la page, ainsi que diverses méta-données. Le tout doit être englobé dans une balise `<head>`

Pour inclure vos fichiers CSS et JS, utilisez les fonctions suivantes:

```php
<?= $this->Html->css('NomDuDossierDuTheme.votrefichier.css'); ?>
<?= $this->Html->script('NomDuDossierDuTheme.votrefichier.js') ?>
```
Bien entendu, les fichiers CSS et JS nécessaires doivent être ajoutés dans les dossiers correspondants du `webroot`.

Variables utiles:
 - `$website_name` - le nom du site défini dans la configuration du CMS
 - `$title_for_layout` - le nom de la page actuelle

### La barre de navigation
La barre de navigation est une partie délicate des thèmes. Elle doit être générée par la configuration du CMS, et demande certaines connaissances en PHP pour être bien comprise.
Si vous n'êtes pas un adepte du PHP, vous pouvez essayer d'utiliser le code déjà présent de certains thèmes pour l'adapter au votre.

| Fonction ou variable | Description |
|----------------------|-------------|
| `$this->Html->url('/')` | Donne l'URL de l'accueil du site |
| `$Lang->get('GLOBAL__HOME')` | Retourne le mot 'Accueil' (ou sa traduction) |
| `$isConnected` | Vrai si l'utilisateur est connecté.|
| `$this->Html->url(array('controller' => 'profile', 'action' => 'index', 'plugin' => false))`| Donne l'URL de la page de profil|
| `$Permissions->can('ACCESS_DASHBOARD'); ?>` | Vrai si l'utilisateur peut accéder au panel d'administration. Vous pouvez, de cette manière, vérifier toutes les permissions possibles d'un utilisateur. |
| `$this->Html->url(array('controller' => 'admin', 'action' => 'index', 'plugin' => false, 'admin' => true))` | Donne l'URL du panel d'administration.|
| `$this->Html->url(array('controller' => 'user', 'action' => 'logout', 'plugin' => false))` | Donne l'URL permettant de se déconnecter.|

La génération se fait à partir du code suivant (exemple):

```php
<?php if(!empty($nav)): ?>
	<?php $i = 0; ?>
	<?php foreach ($nav as $key => $value): ?>
		<?php if(empty($value['Navbar']['submenu'])): ?>
			<li class="<?php if($this->here == $value['Navbar']['url']) { ?> active<?php } ?>"><a href="<?= $value['Navbar']['url'] ?>"><?= $value['Navbar']['name'] ?></a></li>
		<?php else: ?>
			<li class="dropdown">
				<a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false"><?= $value['Navbar']['name'] ?> <span class="caret"></span></a>
				<ul class="dropdown-menu" role="menu">
				<?php
				$submenu = json_decode($value['Navbar']['submenu']);
				foreach ($submenu as $k => $v) {
				?>
					<li><a href="<?= rawurldecode(rawurldecode($v)) ?>"<?= ($value['Navbar']['open_new_tab']) ? ' target="_blank"' : '' ?>><?= rawurldecode(str_replace('+', ' ', $k)) ?></a></li>
				<?php } ?>
				</ul>
			</li>
		<?php endif; ?>
	<?php endforeach; ?>
<?php endif; ?>
```
Il est conseillé de prendre pour exemple un thème déjà existant afin de comprendre les différentes variables et fonctions utilisées habituellement.

### Le contenu
Votre layout doit contenir un emplacement qui accueillera le contenu des pages.
Pour récupérer tout le contenu qui doit être affiché, utilisez la fonction suivante:

```php
<?= $this->fetch('content'); ?>
```
<aside class="alert alert-warning">Votre thème doit gérer la mise en page des plugins. Vous devez admettre que les plugins ne proposent aucune mise en page particulière au départ (vous pourrez modifier chaque plugin individuellement).</aside>

Aidez-vous de la classe `container` ou `container-fluid` de Bootstrap afin d'organiser le contenu sur la page.

```php
<div class="container">
	<?= $this->fetch('content'); ?>
</div>
```
<aside class="alert alert-info">Vous pouvez tester votre mise en page avec le plugin Banlist ou FactionRanking.</aside>

### Les scripts
Les scripts sont généralement chargés en fin de page.
Pour charger un script JS depuis votre thème, utilisez la fonction suivante:

```php
<?= $this->Html->script('NomDuDossierDuTheme.votrefichier.js') ?>
```

Certaines parties de code contenues dans le _default.ctp_ doivent être conservées, comme par exemple ceci en fin de page :

```php
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

## Les éléments

Le dossier `Elements` sert à charger différentes parties de votre thème quand vous en avez besoin, à la manière de modules que vous pouvez ajouter.
Par exemple, on peut y créer un fichier `navbar.ctp`, et y ajouter tout le code de notre barre de navigation.
Pour ajouter la barre de navigation, il suffit d'utiliser le code suivant sur n'importe quelle page (dans notre cas, dans le fichier de layout `default.ctp`):

```php
<?= $this->element('navbar'); ?>
```
Le code de la barre de navigation sera alors inséré à l'endroit choisi.

Il est recommandé de séparer les parties où le code est assez imposant (par exemple, la barre de navigation). Cela vous permettra d'isoler au mieux les différents bugs que vous pouvez rencontrer, d'éviter une fausse manipulation sur l'ensemble du code, et surtout d'avoir un espace de travail plus aéré. On peut par exemple créer un élément pour le slider, le footer, la sidebar, etc.
Vous pouvez aussi, de cette manière, remplacer certains éléments chargés par le CMS ou différents plugins (comme le modal de connexion/inscription).

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
	    "eywek.shop": "1.0.0"
    }
}
```

Vous pouvez voir que l'_ID_ du plugin est spécial, en effet celui-ci se compose de la manière suivante : **auteur.nomdudossier**, vous pouvez donc trouver cet ID vous même facilement en regardant l'intérieur du _config.json_ d'un plugin.
Pour la version, nous avons ici mis la version du plugin, mais vous pouvez très bien la faire précéder d'un outil de comparaison (cf. le semantic versionning donc les outils sont ^ / ~ / >= / <=). Comme ceci :

```json
{
	...
    "supported": {
	    "eywek.shop": ">=1.0.0"
    }
}
```

Dans ce cas là, vous indiquez que votre thème supporte uniquement les versions du plugin "Boutique" au dessus et incluant la 1.0.0.

```json
{
	...
    "supported": {
	    "eywek.shop": "^1.0.0"
    }
}
```

Dans ce cas là, vous indiquez que votre thème supporte uniquement les versions du plugin "Boutique" au dessus et incluant la 1.0.0 mais en dessous de la 2.0.0.

```json
{
	...
    "supported": {
	    "eywek.shop": "~1.0.0"
    }
}
```
Dans ce cas là, vous indiquez que votre thème supporte uniquement les versions du plugin "Boutique" au dessus et incluant la 1.0.0 mais au dessous de la 1.1.0.

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
<?= $this->Html->image('votreimage.ext', array('alt' => 'Ce que je veux')) ?>
```

>Vous pouvez avoir plus d'informations [ici](http://book.cakephp.org/2.0/fr/core-libraries/helpers/html.html).

## Page de personnalisation

Votre thème peut proposer une page de personnalisation, qui permet à l'administrateur de modifier certains aspects de votre thème, tels que la favicon, le logo, le style de navigation, etc.
Les modifications faites sur la page de personnalisation seront sauvegardées dans le fichier `config.json`. Vous n'avez pas besoin de modifier ce fichier pour créer votre page.

Il vous faut pour cela créer un fichier `view.ctp` dans le dossier `Config` du thème.
Comme d'habitude, prenez pour exemple la page de personnalisation d'un thème déjà existant. Copiez-collez le fichier pour être sûr d'avoir la bonne structure.

Chaque champ de votre page sera sauvegardé si sa valeur est définie et valide.
Le nom d'un champ correspondra à son entrée dans la configuration du thème c'est-à-dire que:

```html
<input class="form-control" type="text" name="monOption" value="<?= $config['monOption']">
```

aura pour nom 'monOption' et sera accessible dans toutes les pages de votre thème grâce à la variable `$theme_config['monOption']`.
<aside class="alert alert-warning">Comme vous pouvez le voir, le nom de la variable est `$config['monOption']` dans la page de personnalisation. Cette variable n'existe que sur cette page, tout comme la variable `$theme_config` n'existe pas dans la personnalisation.</aside>

Libre à vous de choisir les différents types de champs dont vous avez besoin dans la configuration du thème.

### Uploader un logo
Réferez-vous au thème Universal pour voir de quelle manière est uploadé le logo sur la page de personnalisation.
Insérez ce PHP au début de votre page:

```php
<?php
$form_input = array('title' => $Lang->get('THEME__UPLOAD_LOGO'));

if(isset($config['logo']) && $config['logo']) {
  $form_input['img'] = $config['logo'];
  $form_input['filename'] = 'theme_logo.png';
}
?>
```

Lorsque vous souhaitez afficher l'élément permettant d'envoyer le logo, utilisez:

```php
<?= $this->element('form.input.upload.img', $form_input) ?>
```

Vous pourrez trouver ci-dessous des exemples d'options.

### Champ de sélection
Voici un exemple d'une option où plusieurs choix sont possibles:

```php
<div class="form-group">
	<label>Désactiver le Slider</label>
	<select name="slider_enabled" class="form-control">
		<option value="true"<?= ($config['slider_enabled'] == "true") ? ' selected' : '' ?>>Oui</option>
	  <option value="false"<?= ($config['slider_enabled'] == "false") ? ' selected' : '' ?>>Non</option>
	</select>
</div>
```

### Champ à choix multiples

```php
<div class="form-group">
	<label>Couleur de navigation</label>
	<small>Couleur générale de la navigation</small>
	<select name="nav-color" class="form-control">
		<option value="#ba2222"<?= ($theme_config['nav-color'] == "#ba2222") ? ' selected' : '' ?>>Rouge</option>
		<option value="#0f1bab"<?= ($theme_config['nav-color'] == "#0f1bab") ? ' selected' : '' ?>>Bleu foncé</option>
			<option value="#129cc5"<?= ($theme_config['nav-color'] == "#129cc5") ? ' selected' : '' ?>>Bleu cyan</option>
			<option value="#c54c12"<?= ($theme_config['nav-color'] == "#c54c12") ? ' selected' : '' ?>>Orange</option>
			<option value="#c5b712"<?= ($theme_config['nav-color'] == "#c5b712") ? ' selected' : '' ?>>Jaune</option>
			<option value="#9112c5"<?= ($theme_config['nav-color'] == "#9112c5") ? ' selected' : '' ?>>Violet</option>
			<option value="#3ec512"<?= ($theme_config['nav-color'] == "#3ec512") ? ' selected' : '' ?>>Vert clair</option>
			<option value="#175f00"<?= ($theme_config['nav-color'] == "#175f00") ? ' selected' : '' ?>>Vert foncé</option>
	</select>
</div>
```

### Champ HTML
Pour afficher un module d'édition HTML, vous devez inclure le code suivant en haut de votre page, permettant de charger le plugin JavaScript TinyMCE:

```php
<?= $this->Html->script('admin/tinymce/tinymce.min.js'); ?>
```

Vous devez ensuite charger le plugin sur un élément donné.

```html
<div class="form-group">
	<label>HTML Supplémentaire de la sidebar</label>
	<p>Ajoutez ici votre propre HTML à ajouter en bas de la sidebar.</p>
		<script type="text/javascript">
		tinymce.init({
				selector: "#sidebar_code",
				height : 100,
				width : '100%',
				language : 'fr_FR',
				plugins: "textcolor code image link",
				toolbar: "fontselect bold italic link image forecolor alignleft aligncenter alignright alignjustify cut copy paste bullist numlist code"
		 });
		</script>
		<textarea class="form-control" id="sidebar_code" name="sidebar_code" cols="30" rows="10"><?= $config['sidebar_code']; ?></textarea>
</div>
```
