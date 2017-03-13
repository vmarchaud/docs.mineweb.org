# Créer un plugin

### Introduction

Un plugin (aussi appelé _extension_), est un ensemble de __fichiers__ qui sont ajoutés à votre CMS pour y ajouter plusieurs __fonctionnalités__ ou modifier certains __comportements__ par défaut. Différents plugins “officiels” sont _déjà disponibles_ sur le site de MineWeb. D’autres peuvent être _développés par la communauté_ et c’est le but de ce tutoriel.

## Création

### Création des fichiers/dossiers

Dans un premier temps, il va falloir créer le dossier de votre plugin, ce dossier doit être créé dans le dossier `app/Plugin/`. Le nom du dossier doit être le nom de votre plugin.

Par **exemple** si je veux créer un plugin de **boutique**, je vais l’appeler : **Shop**.

Une fois ce dossier créé, il faut créer différents autres dossiers et fichiers :

- `Config/`
- `Controller/`
- `Controller/Component/`
- `Model/`
- `Model/Behavior`
- `View/`
- `View/Helper/`
- `View/Layouts`
- `lang/`
- `SQL/`

C’est tout pour les dossiers, il nous reste maintenant encore des fichiers à créer tels que :

- `Config/bootstrap.php`
- `Config/routes.php`
- `config.json`

### Explications et configuration

Maintenant que vous avez créé tous ces fichiers, nous allons passer à la configuration de votre plugin. Pour cela, ouvrez le fichier __config.json__, c'est dans ce fichier que toute la configuration sera située.

> Configuration par défaut (à remplir)

```json
{
  "name":"NAME",
  "slug":"SLUG",
  "nav":true,
  "admin":true,
  "admin_group_menu":"customisation",
  "admin_name":"Boutique",
  "admin_icon":"shopping-cart",
  "admin_route": "/admin/shop",
  "author":"AUTHOR",
  "version":"0.1.0",
  "apiID":1,
  "useEvents":true,
  "permissions" : {
    "available" : [],
    "default" : {
      "0" : [],
      "2" : []
    }
  },
  "requirements" : {
    "CMS" : ">= 1.1.0"
  }
}
```

Maintenant, vous allez pouvoir configurer. Remplacez __NAME__ par le _nom de votre plugin_, __SLUG__ par le _nom du dossier_ et puis __AUTHOR__ par votre pseudo.
Ensuite voici une explication des autres lignes :

- `nav` : si votre plugin est affiché ou non dans la barre de navigation (__boolean__),
- `admin` : si votre plugin a besoin de fichiers pour l’administrer (__boolean__),
- `admin_group_menu` : si votre plugin a besoin de fichiers pour l’administrer, le nom de la section de liens dans le panel admin (__string__) _(optionnel)_,

Valeur | Explication
--------- | -------
`general` | Correspondant au menu 'Général' du panel admin
`customisation` | Correspondant au menu 'Personnalisation' du panel admin
`server` | Correspondant au menu 'Serveur' du panel admin
`other` / `default` | Correspondant au menu 'Autres' du panel admin

- `admin_name` : si votre plugin a besoin de fichiers pour l’administrer, le nom du lien dans le panel admin (__string__),
- `admin_icon` : si votre plugin a besoin de fichiers pour l’administrer, le nom de l'icone FontAwesome qui doit précéder le nom de l'onglet (__string__) _(optionnel)_,
- `admin_route` : si votre plugin a besoin de fichiers pour l’administrer, la route du controller admin (__string__),
- `version` : la version de votre plugin (__double__),
- `apiID` : l’ID du plugin, ce champ est à remplir par la valeur -1 _(sera automatiquement rempli après)_,
- `useEvents` : si votre plugin utilise les événements disponible sur le CMS (__boolean__),
- `permissions` : les permissions de votre plugin (voir plus tard) (__array__)

### Les sous-menu du panel admin

Vous pouvez, si vous le souhaitez, avoir un menu au niveau du panel admin avec des sous-liens (comme pour la boutique). Pour ceci, il vous suffit d'ajouter la clé `admin_menus` dans la configuration du plugin juste après `admin_icon` (et donc la clé `admin_route` devient optionnelle).

> Associez-lui comme valeur un tableau avec vos sous-liens, comme ceci par exemple :

```json
{
  "name":"NAME",
  "slug":"SLUG",
  "nav":true,
  "admin":true,
  "admin_group_menu":"customisation",
  "admin_name":"Boutique",
  "admin_icon":"shopping-cart",
  "admin_menus": [
  	{
  		"name": "Gérer les articles",
  		"icon": "shopping-basket",
  		"url": "/admin/shop",
  		"permission": "SHOP__ADMIN_MANAGE_ITEMS"
  	},
  	{
  		"name": "Gérer les promotions",
  		"icon": "percent",
  		"url": "/admin/shop/shop/vouchers",
  		"permission": "SHOP__ADMIN_MANAGE_VOUCHERS"
  	},
  	{
  		"name": "Gérer les paiements",
  		"icon": "credit-card",
  		"url": "/admin/shop/payment",
  		"permission": "SHOP__ADMIN_MANAGE_PAYMENT"
  	}
  ],
  "author":"AUTHOR",
  "version":"0.1.0",
  "apiID":1,
  "useEvents":true,
  "permissions" : {
    "available" : [],
    "default" : {
      "0" : [],
      "2" : []
    }
  },
  "requirements" : {
    "CMS" : ">= 1.1.0"
  }
}
```

> La clé `permission` dans chaque lien est optionnelle, elle permet d'afficher le lien seulement si la permission est accordée au groupe de l'utilisateur.

## Les tables SQL

Les tables dont vous avez besoin pour votre plugin vont être générées automatiquement par un shell.
Dans un premier temps, toutes les tables de votre plugin doivent être préfixées par le nom de votre plugin.
Par exemple, pour le plugin Shop les tables doivent être préfixés par __shop\_\___.

Pour générer vos tables automatiquement dans un schema (qui sera __indispensable__ pour avoir un plugin valide) il vous faut vous rendre sur le SSH de votre serveur dédié/VPS/ordinateur pour pouvoir utiliser la console de CakePHP. Il vous faut ensuite vous rendre dans le dossier contenant les fichiers du CMS puis, il vous faudra taper
`app/Console/cake schema generate plugin-shop`
Un fichier _schema.php_ sera automatiquement créé dans le dossier SQL de votre plugin.

Si vous ne pouvez pas accéder à la console de CakePHP, vous pouvez toujours créer votre fichier _SQL/schema.php_ __manuellement__.

>Vous devez commençer le fichier comme ceci

```php
<?php
class ShopAppSchema extends CakeSchema {

    public $file = 'schema.php';

    public function before($event = array()) {
        return true;
    }

    public function after($event = array()) {}
}
```

>Il vous faut ensuite suivre la documentation [ici](http://book.cakephp.org/2.0/fr/console-and-shells/schema-management-and-migrations.html#ecrire-un-schema-cakephp-a-la-main)

## Callbacks

Les __callbacks__ sont des fonctions appelées automatiquement par le CMS lors de certaines actions.

Vous pouvez, si vous le souhaitez, créer un fichier __MainComponent.php__ dans le dossier `Controller/Component` de votre plugin.

>Dans ce fichier vous pouvez y ajouter :

```php
<?php
class MainComponent extends Object {

    public function onEnable() {
    }

    public function onDisable() {
    }

}
```

Ces fonctions __onEnable__ et __onDisable__ seront __automatiquement__ appelées par le CMS lors de l’__installation__, l’__activation__ _(pour le onEnable)_, et pour la __désinstallation__ et la __désactivation__ _(pour le onDisable)_ du plugin.

## Fonctions disponibles

_Toutes les fonctions/variables présentes ici, sont celles du **EyPluginComponent**._
### Fonctions

Tout comme les variables, certaines méthodes/fonctions sont disponibles pour vous permettre d’intéragir avec les plugins installés.
Tout d’abord vous avez la fonction __loadPlugins()__ qui vous retournera tout comme la variable __$pluginsLoaded__ (voir ci-dessous) les plugins chargés et installés, cette méthode/fonction vous donne simplement les données à jour si celles-ci ont été modifiées depuis le chargement du Component des plugins.

Vous avez ensuite la fonction __getPluginConfig($slug)__ qui vous retourne le JSON de la configuration d’un plugin. Vous devez __spécifier__ le __slug du plugin__, par exemple, pour Boutique c’est Shop.

Vous pouvez savoir si un plugin est installé avec __isInstalled(*$id*)__, qui retourne un __boolean__, sachant que _$id_ est l’ID au format _auteur.nom.apiID_.

Vous pouvez rechercher un plugin avec différentes fonctions :

- __findPluginsByName(*$name*)__
- __findPluginBySlug(*$slug*)__
- __findPluginByApiID(*$apiid*)__
- __findPluginByID(*$id*)__
- __findPluginsByAuthor(*$author*)__

Ces fonctions vous retourneront soit __un objet__ contenant les données du/des plugin(s) trouvé(s) (selon la fonction) avec comme clé l’ID du plugin, soit un __objet vide__ si aucun plugin ne correspond à la recherche.

### Variables

Les plugins sont chargés dès le chargement de votre site, en effet le CMS __installe automatiquement__ les plugins qui sont valides dans le dossier _/app/Plugin/_ et qui ne sont pas encore présents dans la base de données. Et celui-ci __supprime__ aussi __automatiquement__ les plugins qui sont encore en base de données mais plus dans le dossier.

Après ces opérations effectuées, le CMS __rafraichi les permissions__ (pour pouvoir supprimer/ajouter les permissions des plugins installés/supprimés).
Une fois cela fait, vous pouvez accèder à une variable automatiquement initialisée nommée __$pluginsLoaded__. Celle-ci contient les différents plugins chargés et installés en base de données.

>Vous pouvez l’utilisez comme ceci :

```php
<?php
debug($this->EyPlugin->pluginsLoaded); // Dans un controller
```

>Ce qui retournera :

```php
object(stdClass) {
  eywek.support.2 => object(stdClass) {
      name => 'Support'
      slug => 'support'
      nav => true
      admin => false
      author => 'Eywek'
      version => '0.2.0'
      apiID => (int) 2
      useEvents => false
      permissions => object(stdClass) {
          available => array(
              (int) 0 => 'POST_TICKET',
              (int) 1 => 'VIEW_TICKETS',
              (int) 2 => 'VIEW_ALL_TICKETS',
              (int) 3 => 'DELETE_ALL_TICKETS',
              (int) 4 => 'DELETE_HIS_TICKET',
              (int) 5 => 'RESOLVE_HIS_TICKET',
              (int) 6 => 'RESOLVE_ALL_TICKETS',
              (int) 7 => 'SHOW_TICKETS_ANWSERS',
              (int) 8 => 'REPLY_TO_HIS_TICKETS',
              (int) 9 => 'REPLY_TO_ALL_TICKETS'
          )
          default => object(stdClass) {
              0 => array(
                  (int) 0 => 'POST_TICKET',
                  (int) 1 => 'VIEW_TICKETS',
                  (int) 2 => 'DELETE_HIS_TICKET',
                  (int) 3 => 'RESOLVE_HIS_TICKET',
                  (int) 4 => 'SHOW_TICKETS_ANWSERS',
                  (int) 5 => 'REPLY_TO_HIS_TICKETS'
              )
              2 => array(
                  (int) 0 => 'POST_TICKET',
                  (int) 1 => 'VIEW_TICKETS',
                  (int) 2 => 'DELETE_HIS_TICKET',
                  (int) 3 => 'RESOLVE_HIS_TICKET',
                  (int) 4 => 'SHOW_TICKETS_ANWSERS',
                  (int) 5 => 'REPLY_TO_HIS_TICKETS'
              )
          }
      }
      tables => array(
          (int) 0 => 'support__tickets',
          (int) 1 => 'support__reply_tickets'
      )
  }
  eywek.shop.1 => object(stdClass) {
      name => 'Boutique'
      slug => 'shop'
      nav => true
      admin => false
      author => 'Eywek'
      version => '0.3.1'
      apiID => (int) 1
      useEvents => false
      permissions => object(stdClass) {
          available => array(
              (int) 0 => 'CREDIT_ACCOUNT',
              (int) 1 => 'CAN_BUY'
          )
          default => object(stdClass) {
              0 => array(
                  (int) 0 => 'CREDIT_ACCOUNT',
                  (int) 1 => 'CAN_BUY'
              )
              2 => array(
                  (int) 0 => 'CREDIT_ACCOUNT',
                  (int) 1 => 'CAN_BUY'
              )
          }
      }
      tables => array(
          (int) 0 => 'shop__items',
          (int) 1 => 'shop__categories',
          (int) 2 => 'shop__vouchers',
          (int) 3 => 'shop__paypals',
          (int) 4 => 'shop__paysafecards',
          (int) 5 => 'shop__paysafecard_messages',
          (int) 6 => 'shop__starpasses'
      )
  }
}
```

Les plugins sont différenciés avec un __ID__ propre à eux-même qui se compose comme ceci : _auteur.slug.apiID_ où le slug est le nom du dossier de votre plugin.

Vous avez aussi accès aux variables __$pluginsInFolder__ et __$pluginsInDB__ qui vous retournent un array des plugins contenus dans leurs dossiers ou en base de données (donc installés).
Ces listes __ne sont pas rafraîchies__ après la suppression et/ou installation de nouveaux plugins. Il y a donc de fortes chances que celles-ci ne soient pas à jour.

## Utiliser les events

Dans la config.json du plugin, passez __useEvents__ à __true__.

Pour créer un __écouteur__ _(Listener)_, il vous faut créer un fichier dans le dossier _/Event/_ de votre plugin. Le fichier doit être appelé de la manière suivante _{PLUGIN_NAME}{NOM}EventListener.php_ (préfixé par le slug de votre plugin).

>Exemple: ShopBuyEventListener
>Et son contenu doit être comme ceci :

```php
<?php
  App::uses('CakeEventListener', 'Event');

  class {PLUGIN_NAME}{NOM}EventListener implements CakeEventListener {

    private $controller;

    public function __construct($request, $response, $controller) {
      $this->controller = $controller;
    }

    public function implementedEvents() {
        return array();
    }
  }
```

>Pour écouter un event il vous faut l'ajouter dans l'array retourné par la fonction __implementedEvents()__ avec votre fonction comme valeur. Et il vous faut ensuite créer votre fonction.
>Exemple :

```php
<?php
  App::uses('CakeEventListener', 'Event');

  class NAMEEventListener implements CakeEventListener {

    public function implementedEvents() {
        return array(
          'requestPage' => 'mafonction'
        );
    }

    public function mafonction($event) {

    }
  }
```

### Liste des events disponibles

### Global

- __requestPage__ : appelé lors de chaque requête sans données particulières.
- __onPostRequest__ : appelé lors d’une requête POST sans données particulières.
- __onLoadPage__ : appelé lors de chaque chargement de page dans le beforeRender sans données particulières.
- __onLoadAdminPanel__ : appelé lors de chaque chargement de page admin (prefix) dans le beforeRender sans données particulières.

### Fonction particulière

- __beforeEncodePassword__ : appelé avant chaque encodage de mot de passe avec le pseudo et le mot de passe en données.
- __beforeSendMail__ : appelé avant chaque envoi d’email avec le message et la configuration en données.
- __beforeUploadImage__ : appelé avant chaque upload d’image avec la requête et le nom de l’image voulu en données.

### News

- __beforeAddComment__ : appelé avant que le commentaire ne soit enregistré avec le contenu, l’ID de la news et les infos de l’utilisateur en données.
- __beforeLike__ : appelé avant que le like ne soit enregistré avec l’ID de la news et les infos de l’utilisateur en données.
- __beforeDislike__ : appelé avant que le like ne soit supprimé avec l’ID de la news et les infos de l’utilisateur en données.
- __beforeDeleteComment__ : appelé avant que le commentaire ne soit supprimé avec l’ID du commentaire, l’ID de la news et les infos de l’utilisateur en données.
- __beforeDeleteNews__ : appelé avant que la news ne soit supprimé avec l’ID de la news et les infos de l’utilisateur en données.
- __beforeAddNews__ : appelé avant que la news ne soit enregistré avec le contenu de la requête et les infos de l’utilisateur en données.
- __beforeEditNews__ : appelé avant que la news ne soit enregistré avec le contenu de la requête, l’ID de la news et les infos de l’utilisateur en données.

### User

- __onLogin__ : appelé à chaque login avec l’utilisateur et register (boolean) comme données.
- __beforeRegister__ : appelé avant l’enregistrement d’un utilisateur (après la validation) avec les données de la requête comme données.
- __beforeConfirmAccount__ : appelé avant la confirmation en base de donnée de l’utilisateur avec l’ID de l’utilisateur et manual si confirmé par un administrateur comme données.
- __beforeSendResetPassMail__ : appelé avant l’envoi de l’email permettant la réinitialisation du mot de passe avec l’ID de l’utilisateur et clé de reset comme données
- __beforeResetPassword__ : appelé avant l’enregistrement du nouveau mot de passe avec l’ID de l’utilisateur et le nouveau mot de passe comme données.
- __onLogout__ : appelé pendant la déconnexion avec la session “user” comme données.
- __beforeUpdatePassword__ : appelé avant l’enregistrement du nouveau mot de passe avec l’utilisateur et le nouveau mot de passe encodé comme données.
- __beforeUpdateEmail__ : appelé avant l’enregistrement du nouvel email avec l’utilisateur et le nouveau email comme données.
- __beforeSendPoints__ : appelé avant l’enregistrement de la transaction avec l’utilisateur, le nouveau solde de l’utilisateur, à qui sont transféré les points et combien comme données.
- __beforeEditUser__ : appelé avant que les données ne soit enregistrées avec l’ID de l’utilisateur, les données et `password_updated` comme données.
- __beforeDeleteUser__ : appelé avant que l’utilisateur ne soit supprimé avec ses informations comme données.

### [PLUGIN] Shop

- __onBuy__ : appelé à chaque achat avant tout enregistrement avec les articles, le prix total et les infos de l’user comme données.
- __beforeEditItem__ : appelé avant que les modifications de l’article ne soit enregistrées avec les infos et l’utilisateur en données.
- __beforeAddItem__ : appelé avant que les modifications de l’article ne soit enregistrées avec les infos et l’utilisateur en données.
- __beforeAddCategory__ : appelé avant l’enregistrement de la catégorie avec le nom de la catégorie et les infos de l’utilisateur en données.
- __beforeDeleteItem__ : appelé avant la suppression de l’article avec l’ID de l’article et les infos de l’utilisateur en données.
- __beforeDeleteCategory__ : appelé avant la suppression de la catégorie avec l’ID de la catégorie et les infos de l’utilisateur en données.
- __beforeDeletePaypalOffer__ : appelé avant la suppression de l’offre PayPal avec l’ID de l’offre et les infos de l’utilisateur en données.
- __beforeDeleteStarpassOffer__ : appelé avant la suppression de l’offre StarPass avec l’ID de l’offre et les infos de l’utilisateur en données.
- __beforeAddVoucher__ : appelé avant l’enregistrement du code promo avec les données et les infos de l’utilisateur en données.
- __beforeDeleteVoucher__ : appelé avant la suppression du code promo avec l’ID du code et les infos de l’utilisateur en données.

### [PLUGIN] Vote

- __onVote__ : appelé pendant le vote (après l’enregistrement du vote, avant les récompenses) avec le choix du moyen de récompense (now/later), le site, la config et les infos de l’utilisateur en données.
- __beforeReceiveRewards__ : appelé avant le gain des récompenses avec les récompenses, le type et les infos de l’utilisateur en données.
- __beforeGetWaitingReward__ : appelé avant que la récompense en attente ne soit donnée avec l’utilisateur en données.
- __beforeResetVotes__ : appelé avant que tous les votes ne soit rénitialisés avec les infos de l’utilisateur en données.

## Utiliser les modules

### C'est quoi ?

Les modules permettent aux développeurs de plugins d'ajouter du __code HTML__, __code PHP__, etc... facilement depuis des __pages du CMS__.

### Liste des modules

Les modules disponibles sont :

- `user_profile` _profil d'utilisateur_
- `user_profile_messages` _profil d'utilisateur (haut de page)_
- `admin_user_edit` _modification admin d'un utilisateur (bas de page)_
- `admin_user_edit_form` _modification admin d'un utilisateur (dans le formulaire)_
- `home` _accueil_
- `news` _page affichant une news_

### Comment les utiliser ?

Pour utiliser un module dans votre __plugin__, il vous suffit de créer un dossier _/Modules/_ dans le dossier de votre plugin. Il vous faut ensuite __créer__ un fichier nommé par le __nom du module__ que vous voulez utiliser et avec l'extension __.ctp__.

Par exemple pour utiliser le module _user_profile_ il vous faut créer le fichier _/Modules/user_profile.ctp_.

Dans ce fichier, vous pouvez ajouter le code que vous souhaitez, __HTML__, __PHP__ ou encore __JS__ ou __CSS__.
