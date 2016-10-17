# Créer un plugin

### Introduction

Un plugin (aussi appelé _extension_), c’est des __fichiers__ qui sont ajoutés à votre CMS pour y ajouter plusieurs __fonctionnalités__ ou modifier certains __comportement__ par défaut. Différents plugins “officiels” sont _déjà disponibles_ sur le site de MineWeb. D’autres peuvent être _développés par la communauté_ et c’est le but de ce tutoriel.

## Création

### Création des fichiers/dossiers

Dans un premier temps, il va falloir créer le dossier de votre plugin, ce dossier doit être créé dans le dossier `app/Plugin/`. Le nom du dossier doit être le nom de votre plugin.

Par **exemple** si je veux créer un plugin de **boutique**, je vais l’appeler : **Shop**.

Une fois ce dossier créé, il faut créer différents autres dossiers et fichiers :

- Config/
- Controller/
- Controller/Component/
- Model/
- Model/Behavior
- View/
- View/Helper/
- View/Layouts
- lang/
- SQL/

C’est tout pour les dossiers, il nous reste maintenant encore des fichiers à créer tels que :

- Config/bootstrap.php
- Config/routes.php
- config.json

### Explications et configuration

Maintenant que vous avez créé tout ces fichiers, nous allons passer à la configuration de votre plugin. Pour cela, ouvrez le fichier __config.json__, c'est dans ce fichier que toute la configuration sera située.

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

Maintenant, vous allez pouvoir configurer. Remplacez __NAME__ par le _nom de votre plugin_, __SLUG__ par le _nom du dossier_ et puis AUTHOR par votre pseudo.
Ensuite voici une explication des autres lignes :

- nav : Si votre plugin est affiché ou non dans la barre de navigation (__Boolean__),
- admin : Si votre plugin a besoin de fichiers pour l’administrer (__Boolean__),
- admin_group_menu : Si votre plugin a besoin de fichiers pour l’administrer, le nom de la section de liens dans le panel admin (__String__)_(Optionnel)_,

Valeur | Explication
--------- | -------
`general` | Correspondant au menu 'Général' du panel admin
`customisation` | Correspondant au menu 'Personnalisation' du panel admin
`server` | Correspondant au menu 'Serveur' du panel admin
`other` / `default` | Correspondant au menu 'Autres' du panel admin

- admin_name : Si votre plugin a besoin de fichiers pour l’administrer, le nom du lien dans le panel admin (__String__),
- admin_icon : Si votre plugin a besoin de fichiers pour l’administrer, le nom de l'icone FontAwesome qui doit précéder le nom de l'onglet (__String__)(Optionnel),
- admin_route : Si votre plugin a besoin de fichiers pour l’administrer, la route du controller admin (__String__),
- version : La version de votre plugin (__Double__),
- apiID : L’ID du plugin, ce champ est à remplir par la valeur -1 (_Sera automatiquement rempli après_),
- useEvents : Si votre plugin utilise les événements disponible sur le CMS (__Boolean__),
- permissions : Les permissions de votre plugin (voir plus tard) (__Array__)

### Les sous-menu du panel admin

Vous pouvez, si vous le souhaitez, avoir un menu au niveau du panel admin avec des sous-liens (comme pour la boutique). Pour ceci, il vous suffit d'ajouter la clé `admin_menus` dans la configuration du plugin juste après `admin_icon` (Et donc la clé `admin_route` devient optionnelle).

> Associez-lui comme valeur un tableau avec vos sous-lien, comme ceci par exemple :

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

Les tables dont vous avez besoin pour votre plugin vont être générées automatiquement par un Shell.
Dans un premier temps, toutes les tables de votre plugin doivent être préfixé par le nom de votre plugin.
Par exemple, pour le plugin Shop les tables doivent être préfixés par __shop\_\___.

Pour générer vos tables automatiquement dans un schema (qui sera __indispensable__ pour avoir un plugin valide) il vous faut vous rendre sur le ssh de votre VPS/Ordinateur/Dédié pour pouvoir utiliser la console de CakePHP. Il vous faut ensuite vous rendre dans le dossier contenant les fichiers du CMS puis, il vous faudra taper
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

Tout comme les variables, certaines méthodes/fonctions sont disponibles pour vous permettre d’interagir avec les plugins installés.
Tout d’abord vous avez la fonction __loadPlugins()__ qui vous retournera tout comme la variable __$pluginsLoaded__ (voir ci-dessous) les plugins chargés et installés, cette méthode/fonction vous donne simplement les données à jour si celle-ci ont été modifiés depuis le chargement du Component des plugins.

Vous avez ensuite la fonction __getPluginConfig($slug)__ qui vous retourne le JSON de la configuration d’un plugin. Vous devez __spécifier__ le __slug du plugin__, par exemple, pour Boutique c’est Shop.

Vous pouvez savoir si un plugin est installé avec __isInstalled(*$id*)__, qui retourne un __boolean__, sachant que _$id_ est l’ID au format _auteur.nom.apiID_.

Vous pouvez rechercher un plugin avec différentes fonctions :

- __findPluginsByName(*$name*)__
- __findPluginBySlug(*$slug*)__
- __findPluginByApiID(*$apiid*)__
- __findPluginByID(*$id*)__
- __findPluginsByAuthor(*$author*)__

Qui vous retournerons ou __un objet__ contenant les données du/des plugin(s) trouvé(s) (selon la fonction) avec comme clé l’ID du plugin. Ou la fonction vous retournera __objet vide__ si aucun plugin ne correspond à la recherche.

### Variables

Les plugins sont chargés dès le chargement de votre site, en effet le CMS __installe automatiquement__ les plugins qui sont valides dans le dossier _/app/Plugin/_ et qui ne sont pas encore présent dans la base de données. Et celui-ci __supprime__ aussi __automatiquement__ les plugins qui sont encore en base de données mais plus dans le dossier.

Après ces opérations effectuées, le CMS __rafraichi les permissions__ (pour pouvoir supprimer/ajouter les permissions des plugins installés/supprimés).
Une fois cela fais, vous pouvez accèder à une variable automatiquement initialisée nommée __$pluginsLoaded__. Celle-ci contient les différents plugins chargés et installés en base de données.

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

Les plugins sont différencier avec un __ID__ propre à eux-même qui se compose comme ceci : _auteur.slug.apiID_
Où le slug est le nom du dossier de votre plugin.

Vous avez aussi accès aux variables __$pluginsInFolder__ et __$pluginsInDB__ qui vous retourne un array des plugins contenus dans leur dossier ou en base de données (donc installés).
Ces listes __ne sont pas rafraîchi__ après la suppression et/ou installation de nouveaux plugins. Il y a donc de forte chance que celles-ci ne soient pas à jour.

## Utiliser les events

## Utiliser les modules
