# Installation de MongoDB sur Ubuntu 22.04

Pour vérifier la version d’Ubuntu installé sur la VM, exécutez la commande :

```bash
lsb_release -a
```

La recommandation que je fais est d’installer MongoDB Community Edition disponible au lien ci dessous

[Installation de MongoDB sur Ubuntu 22.04](https://www.notion.so/Installation-de-MongoDB-sur-Ubuntu-22-04-fd63bcc968604b36aa6a6a8db9c524ce)

Les principales étapes de l’installation de MongoDB sont les suivantes :

```bash
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
sudo touch /etc/apt/sources.list.d/mongodb-org-6.0.list
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
sudo apt-get update
```

Je marque une pour pause pour séparer ce bloc de commande qui fonctionne sans aucun problème sur les VM, avec la commande qui est problématique. Il s’agit de la commande ci dessous que je vous invite à lancer :

```bash
sudo apt-get install -y mongodb-org
```

Normalement vous obtiendrez l’erreur suivante :

![Untitled](Installation%20de%20MongoDB%20sur%20Ubuntu%2022%2004%20fd63bcc968604b36aa6a6a8db9c524ce/Untitled.png)

Ci ce n’est pas le cas, inutile de poursuivre, je n’ai pas d’autre solution que de vous conseiller de passer des heures de recherche entre YouTube et Google.

Si cependant, vous avez cette erreur, alors j’ai la solution pour vous. Mais avant faites une vérification. Après avoir retourner la toile pour trouver une solution viable, j’ai fini comprendre que le problème venait de la version OpenSSL 3 souvent empaqueté avec Ubuntu 22.04. En effet, à l’heure où j’écris cette page, les versions de MongoDB disponible ne fonctionne qu’avec OpenSSL 1.1. Pour vérifier votre version d’OpenSSL exécutez la commande ci dessous:

```bash
openssl version 
```

![Untitled](Installation%20de%20MongoDB%20sur%20Ubuntu%2022%2004%20fd63bcc968604b36aa6a6a8db9c524ce/Untitled%201.png)

Si toutes les conditions précédentes sont réunis alors les commandes qui suivent permettront d’installer la version 1.1 de OpenSSL

```bash
# Telecharger les binaires
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/openssl_1.1.1f-1ubuntu2.16_amd64.deb
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl-dev_1.1.1f-1ubuntu2.16_amd64.deb
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb

# Installer  les packages binaire téléchargés
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb
sudo dpkg -i libssl-dev_1.1.1f-1ubuntu2.16_amd64.deb
sudo dpkg -i openssl_1.1.1f-1ubuntu2.16_amd64.deb
```

Vérifier que la version encours d’OpenSSL est bien la 1.1 puis continuer avec l’installation de MongoDB.

```bash
sudo apt-get install -y mongodb-org
```

Si tout se passe bien après cette commande, alors cela signifie l’installation a réussi. Maintenant, vous pouvez lancer le serveur MongoDB et vérifier le statut avec les commandes ci-dessous :

```bash
sudo service mongod start
sudo service mongod status
```

Si le statut est “Active”, il ne reste plus qu’à lancer le shell de Mongo et à vos le NoSQL

```bash
mongosh
```

## Bonus - Quelques commandes utiles

```python
PATH="$PATH:$SPARK_HOME/bin" # On peut lancer SPARK ou n'importe quel autre application depuis n'importe quel repertoire en rajoutant le "bin" au PATH
du -sh /repertoire   # Vérifier la taille d'un repertoire, utile lorsqu'on qu'on stocke en local avant de balancer dans un base de données
ls -l | wc -l   # Nombre total de fichier contenu dans le répertoire actuel
ls -lh      # Taille avec unité de chaque élément contenu dans le répertoire local
```