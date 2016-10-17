---
title: MineWeb docs

language_tabs:
  - php

toc_footers:
  - <a href='http://mineweb.org'>Retour sur mineweb.org</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

## C'est quoi MineWeb ?

MineWeb est un **CMS**. Un CMS est un système de gestion de contenu. Plus précisément il vous permet de vous créer rapidement et facilement un site complètement personnalisable. MineWeb est la version 2 de l'ancien CMS **LapisCraft** de Eywek. Celui-ci n'étant plus stable et mal développé (cf. raisons [ici](http://eywek.fr/lc-explications.pdf)).

MineWeb est développé sous un framework PHP nommé **CakePHP** permettant un développement plus **rapide**, **sécurisé** et **optimisé**. Le projet a été lancé il y a maintenant plus d'**1 an** en compagnie de **Mac'** permettant de convenir entièrement à **vos besoins**.

## Pré-requis

<aside class="alert alert-info"><strong>Hébergeur compatible :</strong> L'hébergeur <a href= "http://revolta-hosting.fr">Revolta-Hosting</a> est 100% <strong>compatible</strong> avec notre CMS !</aside>

Pour installer le CMS MineWeb votre hébergeur **doit** avoir :

*   Version PHP supérieure ou égale à 5.4
*   PDO activé
*   cURL activé
*   Réécriture d'URL
*   .htaccess activés
*   La librairie GD2
*   La possibilité d'ouvrir un zip
*   La possibilité d'ouvrir un site à distance
*   ionCube Loader version 5.4
*   OpenSSL

Pour plus de simplicité vous pouvez télécharger le **fichier de compatibilité** [ici](http://mineweb.org/files/compatibilite.zip). Vous avez juste à extraire cette archive sur votre FTP pour voir si votre hébergeur est compatible.

## Hébergeur MineWeb

Vous ne souhaitez pas vous embêter à chercher un **hébergeur compatible** ? Alors n'attendez plus et utilisez **notre hébergeur** qui vous installera **automatiquement** et **sans aucun effort** le CMS. Tous les prix et informations sont disponibles [ici](http://mineweb.org/download).

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve
