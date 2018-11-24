# Créer un plugin

### Introduction

Un plugin (aussi appelé _extension_), est un ensemble de __fichiers__ qui sont ajoutés à votre CMS pour y ajouter plusieurs __fonctionnalités__ ou modifier certains __comportements__ par défaut. Différents plugins “officiels” sont _déjà disponibles_ [ici](https://github.com/MineWeb?utf8=%E2%9C%93&q=Plugin-&type=&language=php). D’autres peuvent être _développés par la communauté_ et c’est le but de ce tutoriel.

Un PDF est disponible [ici](https://docs.mineweb.org/files/Pl-Helper.pdf) pour vous aider à comprendre et à créer votre plugin.

## Création

### Création des fichiers/dossiers

Dans un premier temps, il va falloir créer le dossier de votre plugin, ce dossier doit être créé dans le dossier `app/Plugin/`. Le nom du dossier doit être le nom de votre plugin.

Par **exemple** si je veux créer un plugin de **boutique**, je vais l’appeler : **Shop**.

Une fois ce dossier créé, il faut créer différents autres dossiers et fichiers :

- `Config/`
- `Controller/`
- `Model/`
- `View/`
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
  "author":"AUTHOR",
  "version":"1.0.0",
  "useEvents":false,
  "permissions" : {
    "available" : [],
    "default" : {
      "0" : [],
      "2" : []
    }
  },
  "requirements" : {
    "CMS" : "^1.2.0"
  }
}
```

Maintenant, vous allez pouvoir configurer. Remplacez __NAME__ par le _nom de votre plugin_ et puis __AUTHOR__ par votre pseudo.
Ensuite voici une explication des autres lignes :

- `version` : la version de votre plugin (__double__),
- `useEvents` : si votre plugin utilise les événements disponible sur le CMS (__boolean__),
- `permissions` : les permissions de votre plugin (voir plus tard) (__array__)
- `requirements` : les pré-requis de votre plugin : vous devez spécifier comme clé un autre ID de plugin (au format _auteur.slug_) ou CMS et comme valeur une version correcte (cf. le semantic versionning donc préfixée ou non par ^ / ~ / >= / <=). Si un des pré-requis n'est pas rempli, le plugin ne sera pas installé sur le CMS.

### Les liens de la barre de navigation

Si votre plugin dispose d'une route publique (pouvant être accessible pour n'importe quel utilisateur) il peut être important de configurer lesquelles de ces routes peuvent être ajoutées sur la barre de navigation depuis le panel admin. Pour cela, il vous suffit d'ajouter la clé `navbar_routes` dans le fichier `config.json`.
Cette clé doit contenir un objet avec comme clé le nom de la route et comme valeur la route.

> Exemple

```json
{
	"name":"Boutique",
	"admin_menus": {},
	"navbar_routes": {
		"Boutique": "/shop"
	},
	"author":"Eywek",
	"version":"1.0.0",
	"useEvents":true,
	"permissions" : {
		"available" : [],
		"default" : {
			"0" : [],
			"2" : []
		}
	},
	"requirements" : {
		"CMS" : "^1.2.0"
	}
}

```

### Les menus panel admin

Vous pouvez, si vous le souhaitez, avoir un menu au niveau du panel admin avec des sous-liens (comme pour la boutique). Pour ceci, il vous suffit d'ajouter la clé `admin_menus` dans la configuration du plugin.
La clé sera le nom du menu, vous pouvez utilisez des noms déjà utilisés pour placer votre menu en tant que sous-menu d'un déjà présent (comme sur l'exemple). Vous pouvez alors ajouter un index pour être après tel ou tel sous-menu
Les clés du panel admin sont les suivantes

Valeur | Explication
--------- | -------
`Dashboard` | Correspondant au menu 'Dashboard' du panel admin
`GLOBAL__ADMIN_GENERAL` | Correspondant au menu 'Général' du panel admin
`GLOBAL__CUSTOMIZE` | Correspondant au menu 'Personnalisation' du panel admin
`SERVER__TITLE` | Correspondant au menu 'Serveur' du panel admin
`GLOBAL__ADMIN_OTHER_TITLE` | Correspondant au menu 'Autres' du panel admin
`STATS__TITLE` | Correspondant au menu 'Statistiques' du panel admin
`MAINTENANCE__TITLE` | Correspondant au menu 'Maintenance' du panel admin
`GLOBAL__UPDATE` | Correspondant au menu 'Mise à jour' du panel admin
`HELP__TITLE` | Correspondant au menu 'Aide' du panel admin

La valeur doit ensuite être un objet contenu l'`icon`, la `route` ou le `menu` (et optionnelement `permission` et `index`)

> Associez-lui comme valeur un tableau avec vos sous-liens, comme ceci par exemple :

```json
{
  "name":"NAME",
  "admin_menus": {
    "GLOBAL__CUSTOMIZE": {
      "Boutique": {
        "index": 1,
        "icon": "shopping-cart",
        "menu": {
          "Gérer les articles": {
            "icon": "shopping-basket",
            "permission": "SHOP__ADMIN_MANAGE_ITEMS",
            "route": "/admin/shop"
          },
          "Gérer les promotions": {
            "icon": "percent",
            "permission": "SHOP__ADMIN_MANAGE_VOUCHERS",
            "route": "/admin/shop/shop/vouchers"
          },
          "Gérer les paiements": {
            "icon": "credit-card",
            "permission": "SHOP__ADMIN_MANAGE_PAYMENT",
            "route": "/admin/shop/payment"
          }
        }
      }
    }
  },
  "navbar_routes": {
    "Boutique": "/shop"
  },
  "author":"AUTHOR",
  "version":"1.0.0",
  "useEvents":true,
  "permissions" : {
    "available" : [],
    "default" : {
      "0" : [],
      "2" : []
    }
  },
  "requirements" : {
    "CMS" : "^1.2.0"
  }
}
```

```json
{
  "name":"NAME",
  "admin_menus": {
    "Boutique": {
      "index": 1,
      "icon": "shopping-cart",
      "menu": {
        "Gérer les articles": {
          "icon": "shopping-basket",
          "permission": "SHOP__ADMIN_MANAGE_ITEMS",
          "menu": {
            "Gérer les promotions": {
              "icon": "percent",
              "permission": "SHOP__ADMIN_MANAGE_VOUCHERS",
              "route": "/admin/shop/shop/vouchers"
            }
          }
        },
        "Gérer les paiements": {
          "icon": "credit-card",
          "permission": "SHOP__ADMIN_MANAGE_PAYMENT",
          "route": "/admin/shop/payment"
        }
      }
    }
  },
  "navbar_routes": {
    "Boutique": "/shop"
  },
  "author":"AUTHOR",
  "version":"1.0.0",
  "useEvents":true,
  "permissions" : {
    "available" : [],
    "default" : {
      "0" : [],
      "2" : []
    }
  },
  "requirements" : {
    "CMS" : "^1.2.0"
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
- __beforePageDisplay__ : appelé lors de chaque chargement de page dans le afterFilter sans données particulières.
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

## Exemple d'un plugin

Je vais avec vous, développer un plugin vous présentant le développement sous Mineweb avec le framework Cakephp 2.x .

Arborescence du plugin :

- `app/Plugin/`
     - `Tutorial/`
        - `Config/`
          - `bootstrap.php`
          - `routes.php`
        - `Controller/`
          - `TutorialAppController.php`
          - `TutorialController`
        - `Model/`
          - `Info.php`
          - `TutorialAppModel.php`
        - `SQL/`
          - `schema.php`
        - `View/`
          - `Tutorial/`
               - `admin_index.ctp`
               - `index.ctp`
        - `lang/`
          - `en_US.json`
          - `fr_FR.json`
        - `config.json`

Dans un premier temps, nous allons créer les routes du plugin. Celles-ci permettent que nous puissions relier les divers arguments de l'url aux controleurs.

Pour des raisons de conventions, vous aurez remarqué que nous ne fermons pas nos balises PHP avec ?>. Cela évite de multiples problèmes et vous familiarise avec les frameworks PHP.  

Si vous voulez plus d'information, je vous conseille ces liens : [StackOverflow](http://stackoverflow.com/questions/4410704/why-would-one-omit-the-close-tag/4499749#4499749) ainsi que les recommandations [PHP-Fig](http://www.php-fig.org/psr/psr-2/)

### Les routes

Allons ensemble dans notre fichier `Config/routes.php` et écrivons ceci :

```php
<?php
Router::connect('/tutorial', ['controller' => 'tutorial', 'action' => 'index', 'plugin' => 'tutorial']);

```    

Notre plugin possède donc une route, lorsqu'un utilisateur ira sur monsite.fr/tutorial, la route s'occupera de rediriger notre visiteur dans le plugin tutorial, à notre controleur Tutorial puis à notre fonction index.

### Les contrôleurs

Ensuite, nous allons créer un contrôleur parent, celui-ci n'est pas obligatoire pour développer un plugin, mais si l'architecture de votre plugin fait que vous devez avoir plusieurs contrôleurs avec des fonctions communes aux deux, vous pourrez facilement joindre vos fonctions.

```php
<?php
class TutorialAppController extends AppController {
    // Vos fonctions communes ici

    protected function math($x, $y, $z){
        return ($x*$x)*$y-$z;
    }
}

```  

<br />

Voici donc notre contrôleur principal, je vous ai mis quelques exemples de code ainsi que des commentaires.

<br />

```php
<?php
class TutorialController extends TutorialAppController{
    public function index(){

        // Chargement du Model Tutorial
        $this->loadModel('Tutorial.Info');

        //On enregistre dans $datas le contenu de toute la table tutorial
        $datas = $this->Info->find('all');

        //On passe la variable à la vue afin de pouvoir la réutiliser dans index.ctp
        $this->set(compact('datas'));

        //Pour passer plusieurs variable à la vue :
        //$this->set(compact('datas', 'variable', 'infos'));

        //Pour donner un titre à votre page : Dans le html <title> Titre <title>
        $this->set('title_for_layout', 'Titre');
    }
}
```

### Les modèles

Les modèles sont des fichiers qui permettent l'interaction entre nos contrôleurs et notre base de données.

Dans notre fichier `Model/TutorialAppModel.php` et écrivons ceci :

```php
<?php
class TutorialAppModel extends AppModel{
    public $tablePrefix = 'tutorial__';
}
```

<br />

Cela nous permet de définir un préfix à notre table. Tous les modèles du plugin l'utiliseront. Il nous reste plus qu'à créer notre modèle.

Pour cela, créez un fichier `Model/Tutorial.php` et écrivez ceci :

```php
<?php
class Info extends TutorialAppModel{

}
```

<br />

Pour l'instant, il est vide, oui, car j'ai directement fait ma requête SQL depuis `Controller/TutorialController.php`.

Mais nous aurions pu faire ceci :

```php
<?php
class TutorialController extends TutorialAppController{
    public function index(){
        $this->loadModel('Tutorial.Info');

        //Appel de la fonction présent dans notre modèle.
        $datas = $this->Info->get();

        $this->set(compact('datas'));
    }
}
```
<br />

Ainsi que dans depuis `Model/Tutorial.php`.

<br />

```php
<?php
class Info extends TutorialAppModel{
    public function get(){
        return $this->find('all');
    }
}
```

### Les schémas / migrations

Les schémas / migrations selon les frameworks sont-ce qui permet à l'application de créer des bases de données, les supprimer afin qu'une base de données soit créer à l'exécution.

Voici celui utilisé pour le tutoriel :

```php
<?php
class TutorialAppSchema extends CakeSchema {

    public $file = 'schema.php';

    public function before($event = []) {
        return true;
    }

    public function after($event = []) {}

    public $tutorial__infos = [
        'id' => ['type' => 'integer', 'null' => false, 'default' => null, 'length' => 8, 'unsigned' => false, 'key' => 'primary'],
        'pseudo' => ['type' => 'string', 'null' => false, 'default' => null, 'length' => 30, 'unsigned' => false],
        'date' => ['type' => 'datetime', 'null' => false, 'default' => null]
    ];
}
```

<br />

Je vous conseille de toujours avoir un champ ID dans votre base de données, cela vous évitera des problèmes futures dans la conception de votre plugin.

### Les vues

Pour finir, il nous reste la vue, c'est là où l'on met notre code html.

Grâce a notre variable $datas transmise, nous pouvons la récupérer afin de l'afficher sous forme de tableau.

<br />

```php
<div id="content">
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <section>
                    <div id="text-page">
                        <table class="table">
                            <thead>
                                <th><?= $Lang->get('TUTORIAL__ID'); ?></th>
                                <th><?= $Lang->get('TUTORIAL__PSEUDO'); ?></th>
                                <th><?= $Lang->get('TUTORIAL__DATE'); ?></th>
                            </thead>
                            <tbody>
                                <?php foreach ($datas as $data): ?>
                                    <tr>
                                        <td><?= $data['Info']['id']; ?></td>
                                        <td><?= $data['Info']['pseudo']; ?></td>
                                        <td><?= $data['Info']['date']; ?></td>
                                    </tr>
                               <?php endforeach; ?>
                            </tbody>
                        </table>
                    </div>
                </section>
            </div>
        </div>
    </div>
</div>
```

<br />

Le fichier admin_index.ctp est la page d'accueil de notre plugin sur la panel d'administration. Je ne vous explique pas car il s'agit de html basique avec une boucle pour afficher les données.

Juste le data-ajax="true"pour envoyer notre formulaire en ajax

<br />

```php
<section class="content">
    <div class="row">
        <div class="col-md-12">
            <div class="box">
                <div class="box-header with-border">
                    <h3 class="box-title"><?= $Lang->get('TUTORIAL__ADD') ?></h3>
                </div>
                <div class="box-body">
                    <div class="row">
                        <div class="col-md-12">
                            <form action="" method="post" data-ajax="true">
                                <div class="form-group">
                                    <input type="text" name="pseudo" class="form-control" placeholder="Pseudo" />
                                </div>
                                <div class="form-group">
                                    <button type="submit" class="btn btn-primary center-block"><?= $Lang->get('GLOBAL__SUBMIT'); ?></button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
            <div class="box">
                <div class="box-body">
                    <div class="row">
                        <div class="col-md-12">
                            <table class="table table-responsive dataTable">
                                <thead>
                                <tr>
                                    <th><?= $Lang->get('TUTORIAL__PSEUDO') ?></th>
                                    <th><?= $Lang->get('TUTORIAL__DATE') ?></th>
                                    <th></th>
                                </tr>
                                </thead>
                                <tbody>
                                    <?php foreach ($datas as $data): ?>
                                        <tr>
                                            <td><?= $data['Info']['pseudo']; ?></td>
                                            <td><?= $this->Time->format($data['Info']['date'], '%H:%M, %e %B %Y'); ?></td>
                                            <td><a onclick="confirmDel('/admin/tutorial/tutorial/delete/<?= $data['Info']['id']; ?>')" class="btn btn-danger"><?= $Lang->get('GLOBAL__DELETE') ?></a></td>
                                        </tr>
                                    <?php endforeach; ?>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

<br />

Une fois fait cela, il faut donc dans notre contrôleur principal, mettre le code pour notre parti admin. Voici le rendu final :

<br />

```php
<?php
class TutorialController extends TutorialAppController{

    public function index(){

        // Chargement du Model Tutorial
        $this->loadModel('Tutorial.Info');

        //On enregistre dans $datas le contenu de toute la table tutorial
        $datas = $this->Info->find('all');

        //On passe la variable à la vue afin de pouvoir la réutiliser dans index.ctp
        $this->set(compact('datas'));

        //Pour passer plusieurs variable à la vue :
        //$this->set(compact('datas', 'variable', 'infos'));

        //Pour donner un titre à votre page : Dans le html <title> Titre <title>
        $this->set('title_for_layout', 'Titre');
    }

    public function admin_index(){
        //Important pour permettre seulements aux admins du site d'y avoir accès.
        if($this->isConnected AND $this->User->isAdmin()){
            $this->loadModel('Tutorial.Info');

            //Si la requete est de type ajax
            if($this->request->is('ajax')){
                //Vu que c'est en ajax, nous n'avons pas besoin du layout
                $this->autoRender = null;

                //Je récupère le champs name="pseudo"
                $pseudo = $this->request->data['pseudo'];
                $date = date('Y-m-d H:i:s');

                $this->Info->add($pseudo, $date);

                //Envoi réponse en ajax
                $this->response->body(json_encode(array('statut' => true, 'msg' => $this->Lang->get('GLOBAL__SUCCESS'))));
            }else{
                //Je déclare le thème du panel admin
                $this->layout = 'admin';

                //Je récupère les données de ma base.
                $datas = $this->Info->get();

                $this->set(compact('datas'));
            }
        }else {
            //Sinon on redirige notre visiteur indiscret vers la page d'accueil
            $this->redirect('/');
        }
    }

    public function admin_delete($id){
        if($this->isConnected AND $this->User->isAdmin()){
            $this->autoRender = null;

            $this->loadModel('Tutorial.Info');

            //J'utilise _delete() car delete() existe déjà avec cakephp
            $this->Info->_delete($id);

            //Redirection vers notre page
            $this->redirect('/admin/tutorial');
        }else {
            $this->redirect('/');
        }
    }
}
```

<br />

Et notre model final :

<br />

```php
<?php
class Info extends TutorialAppModel{

    public function get(){
        return $this->find('all');
    }

    public function _delete($id){
        return $this->delete($id);
    }

    public function add($pseudo, $date){
        $this->create();
        $this->set(['pseudo' => $pseudo, 'date' => $date]);
        return $this->save();
    }
}
```


### Les fichiers de langues

Vous aurez pu remarquer `$Lang->get()`, en effet mes textes sont rangés dans un fichier de traduction, voici sa structure :

`lang/fr_FR.json`

<br />

```json
{
  "TUTORIAL__ID": "ID",
  "TUTORIAL__PSEUDO": "Pseudo",
  "TUTORIAL__DATE": "Date"
}
```

<br />

`lang/en_US.json`

<br />

```json
{}
```

<br />

Si vous ne mettez pas deux accolades dedans, votre plugin ne fonctionnera pas correctement.

Notez que vous n'êtes pas obligé d'utiliser les fichiers de traductions, même si vous ne mettez rien dedans, veillez à les créer et à écrire ceci dedans : `{}`

### Téléchargement

Je vous laisse télécharger le plugin afin de récupérer les codes : [lien du plugin](/files/Tutorial.zip)

Il vous suffit d'extraire le .zip et de le mettre dans /app/Plugin. Vous allez ensuite dans placer le contenu extrait dans app/Plugin. Et allez sur la page de gestion des plugins,  si cela ne marche pas , videz votre cache : /app/tmp/

## Utilisation avancé

Dans le dossier `Config/` se trouve un fichier nommé `bootstrap.php`, je vous redirige vers ce lien [Documentation CakePHP](https://book.cakephp.org/2.0/fr/development/configuration.html#bootstrapping-cakephp) pour savoir à quoi sert t-il.

### Intéragir avec le serveur

Vous pouvez très bien envoyer des commandes ou récupérer toutes sortes d'informations grâce au plugin de Bridge du CMS.
Pour cela il vous suffit de procéder comme ceci (dans vos controllers) :

```php
<?php
// Pour récupérer la liste des connectés sur le serveur sélectionné
$result = $this->Server->call([['GET_PLAYER_LIST' => []]], $server_id);
// Vous pouvez également procéder comme ceci :
$result = $this->Server->call(['GET_PLAYER_LIST' => []], $server_id);
// Ou plus simplement
$result = $this->Server->call('GET_PLAYER_LIST', $server_id);

/*
  Vous pouvez également stack les méthodes
*/

$result = $this->Server->call(['GET_PLAYER_LIST' => [], 'GET_PLAYER_COUNT' => []], $server_id);
```

Pour savoir si **un joueur est connecté** vous pouvez utilisez cette méthode :

```php
<?php
$this->Server->userIsConnected($username, $server_id);
```

Pour **envoyer des commandes** vous avez ces deux méthodes :

```php
<?php
$this->Server->send_command('say Boujour', $server_id);
$this->Server->commands(['say Boujour', 'say Boujour 2'], $server_id);
```

Pour envoyer des **commandes différéees** pous avez cette méthode :

```php
<?php
$this->Server->scheduleCommands(['say Boujour', 'say Boujour 2'], $time, [$server_id]); // Le $time doit être en minute
```

Voici la **liste des méthodes disponibles** :

- GET_PLAYER_LIST
- GET_PLAYER_COUNT
- IS_CONNECTED
- GET_PLUGIN_TYPE
- GET_SYSTEM_STATS
- RUN_COMMAND
- RUN_SCHEDULED_COMMAND
- GET_SERVER_TIMESTAMP
- GET_BANNED_PLAYERS
- GET_MAX_PLAYERS
- GET_MOTD
- GET_VERSION
- GET_WHITELISTED_PLAYERS

### Rajouter des méthodes

Vous pouvez créer un plugin Java vous permettant d'ajouter des méthodes au plugin de Bridge pour pouvoir récupérer plus de données pour vos plugins. Pour cela il vous suffit de créer un plugin minecraft normal puis il vous faut ajouter MinewebBridge comme dépendance dans votre plugin.yml et d'appeler cette méthode dans votre plugin pour ajouter une méthode :

```java
BukkitCore.get().getMethods().put("GET_FACTIONS", new GetFactions());
```

La class que vous passez en paramètre doit ressembler a ceci :

```java
@MethodHandler
public class GetFactions implements IMethod {

    @Override
    public Object execute(ICore instance, Object... inputs) {
    }
}
```

<aside class="alert alert-info">
  <b>Note:</b> Vous pouvez vous aider du plugin de classement Factions disponible <a href="https://github.com/MineWeb/MineWebFactions">ici</a>.
</aside>
