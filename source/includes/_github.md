# Github

Comme vous le savez certainement, le cms est passé totalement opensource. Tout le système de mise à jour / téléchargement passe désormais via l'api de github.

Cependant, une limite de requête est imposée de base par github. Elle est de 60 requêtes/heures. Une fois excédé, l’adresse ip de votre machine est blacklisté ce qui peut conduire à différents messages sur le panel admin comme par exemple : 

> ![example erreur]( https://i.imgur.com/MgyHg3D.png)

Vous pouvez augmenter facilement et rapidement cette limite.

## Etape 1 : Création d'un compte github

Tout d'abord, vous devez avoir un compte sur [github](https://github.com/).
Si vous n'en avez pas, créez-vous en un.

## Etape 2 : Création d'une application OAuth

Ensuite, vous devrez générer une application OAuth qui sera utilisé pour augmentée la limitation. Pour ce faire, allez sur [https://github.com/settings/developers](https://github.com/settings/developers).
Cliquez sur le bouton "New OAuth App"

> ![bouton](https://i.imgur.com/p0zus5c.png)

Vous arrivez alors la dessus : 

> ![app](https://i.imgur.com/CVZFlTk.png)

- Dans le champ **"Application name"** vous pouvez indiquer n'importe quoi, pour ma part j'ai mis "mineweb".
- Pour les champs **"Homepage URL"** et **"Authorization callback Url"**, vous pouvez mettre n'importe quoi (sous entendu des urls valides). Mettez l'url de votre site pas exemple.
- Le champ **"Application description"** est optionel.
- Cliquer sur le bouton **"Register application"**

## Etape 3 : Récuperations des données

Après avoir créé l'application, vous serez redirigez (ou non) sur une page ressemblant à ceci : 
> ![app](https://i.imgur.com/UFj9RQZ.png)

Notez bien les valeurs pour : 
- Client ID
- Client Secret

## Etape 4 : Configuration de votre cms

Maintenant, rendez-vous sur votre panel admin. "Onglet Général > Préférences générales > Préférences autres".
Tout en bas de la page vous avez ce formulaire : 
> ![formulaire](https://i.imgur.com/XerrnHF.png)

Dans le champs : 
-  "Id Client Github", indiquez la valeur de "Client ID" récupérée tantôt. 
-  "Clé secrète Github", indiquez la valeur de "Client Secret" récupérée tantôt. 

Et voila ;)
