# Lier serveur-site

<aside class="alert alert-warning">
  <h3>Compatibilité</h3>
  <p>Le plugin est compatible à partir de la version 1.8</p>
</aside>
<aside class="alert alert-info">
  <p><strong>Type de connexion</strong>:  <br>
  - Par défaut : <em>connexion à un serveur lié avec le <a href="https://github.com/MineWeb/ServerBridge/raw/master/mineweb_bridge-3.0.4.jar">plugin Spigot/BungeCord</a>, permet l’utilisation de toutes les fonctionnalités du CMS (boutique, classement factions, vote…)</em> <br>
  - Rcon : <em>Permet l'utilisation des plugins (boutique, classement factions, vote…)</em></br>
  - Ping : <em>connexion à un serveur sans plugin, permet uniquement d’avoir le nombre de joueurs en ligne et le nombre de joueurs maximums (la <strong>boutique</strong> et le <strong>classement</strong> factions <strong>ne pourront pas être utilisés</strong> avec ce type de connexion) </em></p>
</aside>

## Configuration préalable

Rendez-vous sur la page de liaison site-serveur sur le panel admin de votre CMS.
Vous devez dans un premier temps configurer le temps d’exécution maximum (appelé aussi timeout), il est conseillé de mettre 1 (seconde).

![](https://docs.mineweb.org/images/server_timeout.png)

<aside class="alert alert-info">
Vous avez à côté deux boutons pour désactiver la fonctionnalité (qui mettra automatiquement le statut du serveur à : éteint) si vous ne souhaitez pas utiliser la liaison site-serveur. Et un autre bouton pour activer/désactiver le cache sur la “bannière” (permet que le message affiché sur le site (joueurs connectés, MOTD…) soit stocké pendant quelques minutes permettant d’éviter la surcharge de commande au serveur).
</aside>

## Liaison à un serveur (par défaut)

Maintenant nous allons lier votre __serveur Minecraft__ au __CMS__. Pour cela vous devez avoir installé sur votre serveur le plugin MineWeb disponible à [**cette adresse**](https://github.com/MineWeb/ServerBridge/raw/master/mineweb_bridge-3.0.4.jar).
Une fois le plugin téléchargé, placez-le dans votre dossier `plugins` de votre serveur et __redémarrez celui-ci__.
Vous n’avez __pas à toucher à la configuration du plugin__.

Maintenant que vous avez configuré le serveur vous pouvez __ajouter un serveur__ et remplir les informations nécessaires :

- `Type de la connexion`, donc mettez _Par défaut_.
- `Nom` par le nom affiché.
- `IP` par l’ip (ou domaine) sans le port (Ex. 187.25.69.24).
- `Port` change suivant le type de serveur (Ex. 25565)
  - `Port` **Pour Spigot :** par le port de votre serveur, attention à ne pas effectuer de commande en jeu si vous utilisez le port du serveur (seulement si vous avez protocolibs, sinon utilisez un port personnalisé).
  - `Port` **Pour BungeeCord :** par le port de votre serveur.

Cliquez ensuite sur _Connexion_ pour tester la connexion, si celle-ci échoue, rendez-vous sur [**cette section**](https://docs.mineweb.org/#la-connexion-choue)

<aside class="alert alert-warning">
<b>Port customisé :</b> Protocolibs n'est pas obligatoire pour utiliser un port personnalisé. Vous pouvez également configurer un port personnalisé si vous ne souhaitez pas utiliser le port minecraft de votre serveur. Pour cela, assurez-vous d'avoir un port disponible et ouvert (Ne pas mettre le port de son serveur) et effectuez la commande /mineweb port <port>.
</aside>

## Liaison par RCON

Vous n’avez pas besoin de plugin pour cette liaison. Via ce protocol vous ne pouvez pas utiliser l'option obligeant un joueur à être connecté sur le serveur, il faut utiliser le plugin pour cela. 
Le RCON est disponible sur tous les serveurs minecraft et permet de communiquer avec le site. Pour utiliser ce protocol il vous faut le configurer dans votre server.properties :

- Passer enable-rcon à true, puis redémarrez votre serveur
- Changer rcon.password par un mot de passe
- Changer rcon.port par un port disponible

Il vous suffit ensuite de configurer un serveur comme ceci depuis la page de liaison :

- `Type` de la connexion, donc mettez _RCON_.
- `Nom` par le nom affiché.
- `IP` par l’ip (ou domaine) __sans le port__.
- `Port` par le port de votre serveur (25565 par défaut si vous n’avez pas de port).
- `Port RCON` par le port de RCON configureé (doit être ouvert et disponible bien sûr).
- `Mot de passe RCON` par le mot de passe RCON.

Cliquez ensuite sur _Connexion_ pour tester la connexion.

<aside class="alert alert-warning">
  <h3>Bungeecord</h3>
  <p>Pour lier votre site Mineweb avec un serveur Bungeecord, vous devez utiliser le Rcon. Pour cela, installez BungeeRcon sur votre serveur Bungeecord, et reliez le Rcon à BungeeRcon.</p>
</aside>

## Liaison par PING

Vous n’avez pas besoin de plugin pour cette liaison, il vous suffit juste de configurer un serveur comme ceci depuis la page de liaison :

- `Type` de la connexion, donc mettez _Ping_.
- `Nom` par le nom affiché.
- `IP` par l’ip (ou domaine) __sans le port__.
- `Port` par le port de votre serveur (25565 par défaut si vous n’avez pas de port).

Cliquez ensuite sur _Connexion_ pour tester la connexion.

## La connexion échoue

Dans un premier temps, vérifiez que vous n'avez pas d'__erreur dans la console__ de votre serveur en rapport avec le plugin MineWeb.

Ensuite, essayez de __redémarrer votre serveur__.

Si la connexion échoue toujours, venez-nous voir sur [Discord](https://discordapp.com/invite/3QYdt8r) avec un maximum d'explications et d'informations, et répondez à ces questions :

- Quelle IP avez-vous mis ?
- Quel port ?
- Aucune erreur dans la console ?
- Quelle version de Spigot avez-vous ?
- Avez-vous essayé de rédemarrer le serveur ?
- Fournissez-nous des logs de démarrage sur pastebin.com
- Fournissez-nous le fichier config.json du plugin sur pastebin.com
- Fournissez-nous le fichier mineweb.log du plugin sur pastebin.com
