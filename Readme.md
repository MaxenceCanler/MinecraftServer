# Guide d'Installation et Configuration de Serveurs Minecraft avec TLauncher et BungeeCord

Ce guide détaille les étapes pour installer et configurer des serveurs Minecraft en utilisant TLauncher et BungeeCord sous un environnement Linux.

## Prérequis

- Java 17
- Screen
- Accès à un terminal Linux

## Installation

### Java

1. Installer Java :
sudo apt install openjdk-17-jdk

2. Vérifier l'installation :
java -version


### Screen

1. Installer Screen :
sudo apt install screen

2. Vérifier l'installation :
screen -v

### TLauncher

1. Télécharger TLauncher depuis [le site officiel](https://tlauncher.org/en/).
2. Lancer le fichier `.jar` téléchargé :
java -jar TLauncher-2.895.jar

3. Sélectionner un pseudo et lancer le jeu en version Optifine 1.12.2.

## Configuration des Serveurs

### Création du Premier Serveur

1. Créer un dossier nommé `server1` et y entrer.
2. Créer un screen :
screen -S server1
Pour quitter le screen : `Ctrl + a` puis `d`.
Pour retourner dans le screen : `screen -r server1`.
3. Copier le fichier `spigot1.12.2` fourni dans ce dossier.

### Lancement du Serveur

1. Lancer le script fourni pour démarrer le serveur.
2. Accepter l'EULA en modifiant le fichier `eula.txt` à `true`.
3. Redémarrer le serveur.
4. Pour autoriser les joueurs non-officiels (crack), modifier `online-mode` à `false` dans `server.properties`.
5. Relancer le serveur et se connecter via `localhost:25565`.

### Configuration du Deuxième Serveur

Répéter les étapes de création pour le `server2`, en modifiant également `server-port` à `25566` dans `server.properties`.

### Configuration du Proxy BungeeCord

1. Créer un dossier `proxy` et y placer `bungeecord.jar`.
2. Ouvrir un screen :
screen -S proxy
3. Lancer le script dans le dossier BungeeCord, puis l'éteindre pour configurer.
4. Modifier `config.yml` :
- Changer le port par défaut en `0.0.0.0:25565`.
- Configurer les permissions admin et la liste des serveurs.
- Mettre `online_mode` à `false`.
5. Adapter les ports dans `server.properties` de chaque serveur et redémarrer tout.

### Ajout d'un Serveur Lobby

Créer un serveur appelé `lobby` en suivant les mêmes étapes, l'ajouter dans BungeeCord et le configurer en tant que serveur principal.

## Objectif

L'architecture finale comprend 3 serveurs : `lobby` (serveur d'accueil), `server1` et `server2`. Des plugins seront développés pour permettre aux joueurs de naviguer entre ces serveurs.

## Création d'un Plugin Java pour la Commande /Lobby

Ce plugin permettra aux joueurs d'utiliser la commande `/lobby` pour retourner au serveur lobby depuis n'importe quel autre serveur dans le réseau BungeeCord.

### Prérequis

- Connaissance de base en Java
- Environnement de développement Java (IDE)
- API Spigot ou Bukkit

### Étapes de Développement

1. **Configuration de l'Environnement de Développement** :
   - Configurer un projet Java dans votre IDE.
   - Ajouter l'API Spigot ou Bukkit en tant que dépendance dans votre projet.

2. **Création de la Classe Principale** :
   - Créer une nouvelle classe Java qui étend `JavaPlugin`.
   - Surchargez les méthodes `onEnable` et `onDisable` pour gérer le démarrage et l'arrêt du plugin.

3. **Enregistrement de la Commande /Lobby** :
   - Dans la méthode `onEnable`, enregistrez la commande `/lobby` pour qu'elle soit reconnue par le serveur.
   - Créez une nouvelle classe qui implémente `CommandExecutor`.
   - Dans cette classe, implémentez la logique pour connecter le joueur au serveur lobby.

4. **Implémentation de la Commande /Lobby** :
   - Utilisez l'API BungeeCord pour transférer le joueur au serveur lobby.
   - La commande doit vérifier que le joueur n'est pas déjà dans le lobby.

5. **Compilation et Test du Plugin** :
   - Compilez le plugin dans un fichier `.jar`.
   - Placez le fichier `.jar` dans le dossier `plugins` de votre serveur Spigot/Bukkit.
   - Redémarrez le serveur et testez la fonctionnalité de la commande `/lobby`.

### Exemple de Code pour la Commande /Lobby

```java
public class LobbyCommand implements CommandExecutor {

    @Override
    public boolean onCommand(CommandSender sender, Command command, String label, String[] args) {
        if (sender instanceof Player) {
            Player player = (Player) sender;
            // Logique pour connecter le joueur au serveur lobby
            // Exemple : Utilisation de l'API BungeeCord
        }
        return true;
    }
}

```

## Configuration de LuckPerms sur BungeeCord et les Serveurs

LuckPerms est un système de gestion des permissions avancé. Cette section décrit comment configurer LuckPerms sur votre réseau BungeeCord, y compris sur chaque serveur individuel.

### Prérequis

- LuckPerms installé sur BungeeCord et sur chaque serveur Spigot/Bukkit.
- Accès aux fichiers de configuration de chaque serveur.

### Installation de LuckPerms

1. **Télécharger LuckPerms** :
   - Assurez-vous de télécharger la version appropriée de LuckPerms pour BungeeCord et Spigot/Bukkit depuis [leur site officiel](https://luckperms.net/).

2. **Installer LuckPerms sur BungeeCord** :
   - Placez le fichier `.jar` de LuckPerms dans le dossier `plugins` du serveur BungeeCord.

3. **Installer LuckPerms sur chaque Serveur Spigot/Bukkit** :
   - Répétez le processus en plaçant le fichier `.jar` dans le dossier `plugins` de chaque serveur Spigot/Bukkit.

### Configuration de Base

1. **Configuration de BungeeCord** :
   - Ouvrez le fichier de configuration de LuckPerms sur le serveur BungeeCord.
   - Assurez-vous que la synchronisation est activée pour permettre la cohérence des permissions à travers le réseau.

2. **Configuration sur les Serveurs Spigot/Bukkit** :
   - Ouvrez le fichier de configuration de LuckPerms sur chaque serveur.
   - Configurez chaque serveur pour qu'il se connecte à la même base de données que celle utilisée par le serveur BungeeCord.
   - Cela permettra la synchronisation des données de permissions entre les serveurs.

### Synchronisation des Permissions

- Pour synchroniser les permissions à travers votre réseau, configurez LuckPerms pour utiliser une base de données partagée (comme MySQL ou MariaDB).
- Cette configuration garantit que toutes les modifications de permissions effectuées sur un serveur seront reflétées sur les autres.

### Conseils d'Utilisation

- Utilisez les commandes LuckPerms pour gérer les permissions de manière globale à partir de n'importe quel serveur connecté à la base de données partagée.
- Assurez-vous de sauvegarder régulièrement votre base de données de permissions pour éviter toute perte de données.

---

En suivant ces instructions, LuckPerms sera configuré sur votre réseau BungeeCord, permettant une gestion cohérente et centralisée des permissions sur tous vos serveurs Minecraft.
