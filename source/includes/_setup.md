# Installation

<aside class="alert alert-info">
<h3>Informations</h3>
<p>Vous devez placer tout le contenu de l'archive téléchargée sur votre espace membre, sur votre hébergeur (les fichiers .DS_Store & \__MACOSX sont inutiles). Il faut ensuite appliquer les permissions 755 ou 775 sur tous les fichiers (pour permettre les mises à jour). Puis rendez-vous sur sur http://{votre site}/install</p>
</aside>

## Étape 1 - Configuration de la base de données

__Sautez cette étape si vous utilisez l'hébergeur MineWeb__

Vous devez avoir une page demandant vos identifiants de base de données, vous devez entrez ceux-ci et tester la connexion, puis il vous faudra cliquer sur un bouton qui installera la base de données automatiquement.

## Étape 2 - Mise en place de la licence

Une fois la connexion à la base de données effectuée, le CMS vous demandera alors votre clé d'activation. Celle-ci est disponible sur votre [profil](http://mineweb.org/user/profile).
Si vous obtenez des erreurs, voici leurs explications et résolutions.

Erreur | Explication | Résolution
--------- | ------- | -----------
`Licence inconnue` | Votre licence n'est pas trouvée dans notre base de données | Téléchargez à nouveau les fichiers du CMS
`Site d'installation non valide` | L'URL que vous utilisez pour accèder à votre site ne correspond pas à celle configurée | Rendez-vous sur votre [profil](http://mineweb.org/user/profile) et changez l'URL de votre licence
`Licence désactivée` | La licence n'est pas activée | Rendez-vous sur votre [profil](http://mineweb.org/user/profile) et réactivez votre licence. Celle-ci peut être désactivée par l'administration si un problème survient, une explication est alors affichée
`L'API de MineWeb.org est temporairement indisponible` | Le système de vérification de licence n'est pas disponible | Patientez, si ce problème survient nous travaillons à la résolution du problème

## Étape 3 - Création de l'administrateur

Voici la troisième et dernière étape de l'installation du CMS, vous allez créer votre compte administrateur qui vous permettra d'accéder au panel admin du CMS. Vous avez juste à remplir les champs qui vous sont présentés et à soumettre vos informations en cliquant sur le bouton "Suivant".

## Installation terminée

Une fois l'installation du CMS complétée vous pouvez passer à l'utilisation du CMS en lui-même. Pour configurer votre CMS il vous faut vous connecter en cliquant sur le bouton en haut à gauche de la barre de navigation. Vous cliquez sur connexion puis entrez vos informations pour vous connecter. Vous cliquez ensuite sur Panel administrateur, vous serez redirigé et vous pourrez ensuite cliquer sur _Général_ puis _Préférences générales_.

## Aide externe et problèmes fréquents

### Problème avec ionCube

Il est possible que vous obteniez une erreur dans ce style là :

`/app/Controller/Component/UpdateComponent.php cannot be decoded by this version of the ionCube Loader. If you are the administrator of this site then please install the latest version of the ionCube Loader.`

Cela signifie que votre hébergeur n'a pas installé la bonne version du _ionCube Loader_, il faut alors voir avec leur support. À noter que la version de PHP ne doit pas être supérieur à la version 7.

### Problème chez livehost

Il est possible que vous obteniez une erreur dans ce style là :

`Warning: include(Cake/bootstrap.php): failed to open stream: No such file or directory in`

Pour résoudre ce problème, rendez-vous dans _app/webroot_, ouvrez le fichier _index.php_ et allez à la ligne 64

`//define('CAKE_CORE_INCLUDE_PATH', ROOT . DS . 'lib');`

Retirez les deux slashs

`define('CAKE_CORE_INCLUDE_PATH', ROOT . DS . 'lib');`

Sauvegardez et rechargez la page.


### Autres

- Si vous avez besoin d'aide, si vous rencontrez un problème non répertorié ici, vous pouvez nous contacter sur le [support](http://mineweb.org/support).
