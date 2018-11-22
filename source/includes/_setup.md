# Erreur 500

Pour corriger l'erreur 500 sur votre cms faites <strong>toutes</strong> les étapes :
*   Supprimez le contenu du fichier **"/app/tmp/"**
*   Si ca ne marche toujours pas : 
    *   Réeffectuez la manipulation qui a provoqué l'erreur
    *   Mettez en ligne sur Pastebin le fichier **"/app/tmp/logs/error.log"** afin de donner le lien au support dans le chat du [Discord](https://discordapp.com/invite/3QYdt8r)


Pour éviter les confusions ou éviter toute erreur de votre part, merci de bien vouloir lire toute la documentation avant de demander de l'aide au support et pour être plus efficace.

# Installation

<aside class="alert alert-info">
<h3>Informations</h3>
<p>Vous devez placer tout le contenu de l'archive téléchargée, sur votre hébergeur (les fichiers .DS_Store & \__MACOSX sont inutiles). Il faut ensuite appliquer les permissions 755 ou 775 sur tous les fichiers (pour permettre les mises à jour). Puis rendez-vous sur sur http://{votre site}/install</p>
</aside>

## Étape 1 - Configuration de la base de données

Vous devez avoir une page demandant vos identifiants de base de données, vous devez entrez ceux-ci et tester la connexion, puis il vous faudra cliquer sur un bouton qui installera la base de données automatiquement.

## Étape 2 - Création de l'administrateur

Voici la deuxième et dernière étape de l'installation du CMS, vous allez créer votre compte administrateur qui vous permettra d'accéder au panel admin du CMS. Vous avez juste à remplir les champs qui vous sont présentés et à soumettre vos informations en cliquant sur le bouton "Suivant".

## Installation terminée

Une fois l'installation du CMS complétée vous pouvez passer à l'utilisation du CMS en lui-même. Pour configurer votre CMS il vous faut vous connecter en cliquant sur le bouton en haut à gauche de la barre de navigation. Vous cliquez sur connexion puis entrez vos informations pour vous connecter. Vous cliquez ensuite sur Panel administrateur, vous serez redirigé et vous pourrez ensuite cliquer sur _Général_ puis _Préférences générales_.

## Aide externe et problèmes fréquents

### Problème chez livehost

Il est possible que vous obteniez une erreur dans ce style là :

`Warning: include(Cake/bootstrap.php): failed to open stream: No such file or directory in`

Pour résoudre ce problème, rendez-vous dans _app/webroot_, ouvrez le fichier _index.php_ et allez à la ligne 64

`//define('CAKE_CORE_INCLUDE_PATH', ROOT . DS . 'lib');`

Retirez les deux slashs

`define('CAKE_CORE_INCLUDE_PATH', ROOT . DS . 'lib');`

Sauvegardez et rechargez la page.

### Autres

- Si vous avez besoin d'aide, si vous rencontrez un problème non répertorié ici, vous pouvez nous contacter sur notre [Discord](https://discordapp.com/invite/3QYdt8r).
