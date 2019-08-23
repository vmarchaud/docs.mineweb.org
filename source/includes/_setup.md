# Installation

<aside class="alert alert-info">
<h3>Informations</h3>
<h4>Sur un VPS</h4>

Pour une installation simple, et rapide du CMS et de ses dépendances, utilisez le script d'installation : [TELECHARGEMENT & DOCUMENTATION](https://github.com/MaximeMichaud/mineweb-install)

<h4>Sur un mutialisé</h4>
<p>Vous devez placer tout le contenu de l'archive téléchargée, sur votre hébergeur (les fichiers .DS_Store & \__MACOSX sont inutiles). Il faut ensuite appliquer les permissions 777 sur tous les fichiers (pour permettre les mises à jour et création des fichiers). Puis rendez-vous sur sur http://{votre site}/install</p>
</aside>

## Étape 1 - Configuration de la base de données

Vous devez avoir une page demandant vos identifiants de base de données, vous devez entrez ceux-ci et tester la connexion, puis il vous faudra cliquer sur un bouton qui installera la base de données automatiquement.

## Étape 2 - Création de l'administrateur

Voici la deuxième et dernière étape de l'installation du CMS, vous allez créer votre compte administrateur qui vous permettra d'accéder au panel admin du CMS. Vous avez juste à remplir les champs qui vous sont présentés et à soumettre vos informations en cliquant sur le bouton "Suivant".

## Installation terminée

Une fois l'installation du CMS complétée vous pouvez passer à l'utilisation du CMS en lui-même. Pour configurer votre CMS il vous faut vous connecter en cliquant sur le bouton en haut à droite de la barre de navigation. Vous cliquez sur "Connexion" puis entrez vos informations pour vous connecter. Vous cliquez ensuite sur "Panel administrateur", vous serez redirigé vers l'espace d'administration où vous pouvez configurer votre site.

## Aide externe et problèmes fréquents

### Important

**Pour éviter les confusions ou éviter toute erreur de votre part, merci de bien vouloir lire toute la documentation avant de demander de l'aide au support et pour être plus efficace.**

### Problème chez livehost

Il est possible que vous obteniez une erreur dans ce style là :

`Warning: include(Cake/bootstrap.php): failed to open stream: No such file or directory in`

Pour résoudre ce problème, rendez-vous dans _app/webroot_, ouvrez le fichier _index.php_ et allez à la ligne 64

`//define('CAKE_CORE_INCLUDE_PATH', ROOT . DS . 'lib');`

Retirez les deux slashs

`define('CAKE_CORE_INCLUDE_PATH', ROOT . DS . 'lib');`

Sauvegardez et rechargez la page.

### Erreur CRSF

* Supprimer le contenu du dossier **"/app/tmp/"***
* Supprimer le cache de votre navigateur
* Vérifier que le **HTTPS** est forcé

### Erreur CRSF chez OVH

Il est possible que les instructions ci-dessus pour l'erreur CRSF n'est aucun effet si vous êtes hébergés chez OVH.

Dans le .ovhconfig, changez le **app.engine=php** en **app.engine=phpcgi**

### Erreur 500 (Ionos)

* Supprimez le contenu du dossier **"/app/tmp/"**
* Ajouter **RewriteBase /** dans le **.htaccess** contenu dans **/app/webroot/**

### Erreur 500 (Hors Ionos)

Pour corriger l'erreur 500 sur votre cms faites **toutes** les étapes :

*   Supprimez le contenu du dossier **"/app/tmp/"**
*   Si ca ne marche toujours pas :
    *   Réeffectuez la manipulation qui a provoqué l'erreur
    *   Mettez en ligne sur Pastebin le fichier **"/app/tmp/logs/error.log"** afin de donner le lien au support dans le chat du [Discord](https://discordapp.com/invite/3QYdt8r)

### Page introuvable

- Lorsque vous mettez un dossier a la racine du CMS vous avez une erreur de page introuvable suivez le PDF disponible [ici](https://docs.mineweb.org/files/Webroot-Helper.pdf).

### Autres

- Si vous avez besoin d'aide, si vous rencontrez un problème non répertorié ici, vous pouvez nous contacter sur notre [Discord](https://discordapp.com/invite/3QYdt8r).
