# Lier serveur-site

<aside class="alert alert-warning">
  <h3>Compatibilité</h3>
  <p>Les plugins sont compatibles à partir de la version 1.7.10</p>
  <p><small><em>Des problèmes de compatibilité peuvent survenir avec Cauldron et Sponge.</em></small></p>
</aside>
<aside class="alert alert-info">
  <p><strong>Type de connexion</strong>:  <br>
  - Par défaut : <em>connexion à un serveur lié avec le <a href="https://docs.mineweb.org/files/mineweb_bridge-2.0.0.jar">plugin Bukkit/Spigot/Bungeecord</a>, permet l’utilisation de toutes les fonctionnalités du CMS (boutique, classement factions, vote…)</em> <br>
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

Maintenant nous allons lier votre __serveur Minecraft__ au __CMS__. Pour cela vous devez avoir installé sur votre serveur le plugin MineWeb disponible à [cette adresse](https://docs.mineweb.org/files/mineweb_bridge-2.0.0.jar).
Une fois le plugin téléchargé, placez-le dans votre dossier `plugins` de votre serveur et __redémarrez celui-ci__.
Vous n’avez __pas à toucher à la configuration du plugin__.

Maintenant que vous avez configuré le serveur vous pouvez __ajouter un serveur__ et remplir les informations nécessaires :

- `Type de la connexion`, donc mettez _Par défaut_.
- `Nom` par le nom affiché.
- `IP` par l’ip (ou domaine) sans le port.
- `Port` par le port de votre serveur (25565 par défaut si vous n’avez pas de port).

Cliquez ensuite sur _Connexion_ pour tester la connexion, si celle-ci échoue, rendez-vous sur [cette section](https://docs.mineweb.org/#la-connexion-choue)

<aside class="alert alert-info">
**Cauldron :** Dans le dossier de votre serveur, vous devez vous rendre dans le dossier _Librairies/com/google/guava/guava_ et changez le JAR par [celui-ci](https://docs.mineweb.org/files/guava-17.jar).
</aside>

## Liaison par Ping

Vous n’avez pas besoin de plugin pour cette liaison, il vous suffit juste de configurer un serveur comme ceci depuis la page de liaison :

- `Type` de la connexion, donc mettez _Ping_.
- `Nom` par le nom affiché.
- `IP` par l’ip (ou domaine) __sans le port__.
- `Port` par le port de votre serveur (25565 par défaut si vous n’avez pas de port).

Cliquez ensuite sur _Connexion_ pour tester la connexion.

## La connexion échoue

Dans un premier temps, vérifiez que vous n'avez pas d'__erreur dans la console__ de votre serveur en rapport avec le plugin MineWeb.

Ensuite, essayez de __redémarrer votre serveur__.

Si la connexion échoue toujours, contactez le [support](http://mineweb.org/support) avec un maximum d'explications et d'informations, et répondez à ces questions :

- Quelle IP avez-vous mis ?
- Quel port ?
- Aucune erreur dans la console ?
- Avez-vous essayé de rédemarrer le serveur ?
- Fournissez-nous des logs de démarrage sur pastebin.com
- Fournissez-nous le fichier config.json du plugin sur pastebin.com
- Fournissez-nous le fichier mineweb.log du plugin sur pastebin.com
