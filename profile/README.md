# 🤖 RAM - Rogue AI Model

Bienvenue sur notre jeu **RAM**, Un roguelike dans la thématique de l'évolution de l'IA. Ce **README** vous permet de démarrer le jeu en local via Docker.

---

## 📋 Architecture

Le projet suit une architecture **Microservices** distribuée :

* **Front** : React / Vite
* **Gateway** : Point d'entrée unique qui redirige les requêtes.
* **Microservices** : Services métiers isolés (User, Player, Game, etc.).
* **Databases** : Chaque microservice possède sa propre base de données MariaDB, Battle est géré avec Redis.
   * MariaDB a été choisi pour son stockage de données structuré avec un format fixe,
   * Redis a été utilisé pour sa rapidité en tant que cache pendant l’exécution de l’application.
* **Network** : Tous les conteneurs communiquent via un réseau Docker privé `ram-shared-network`.
* **Broker de messages** : RabbitMQ est utilisé pour la gestion des logs :
  * Les microservices (sauf Front et Logger) agissent comme producers, en envoyant des messages dans une queue à chaque action effectuée.
  * Le microservice Logger agit comme consumer, récupérant les messages pour les traiter et les stocker.

<img width="1027" height="790" alt="image" src="https://github.com/user-attachments/assets/80aaf425-d08d-4bea-a07d-79946e54804d" />

---

## 🛠 Prérequis

Avant de commencer, assurez-vous d'avoir installé :

1.  **Docker Desktop** (avec Docker Compose) - [Télécharger](https://www.docker.com)
    * *Assurez-vous que Docker est lancé.*
2.  **Git** - [Télécharger](https://git-scm.com/downloads)
3.  **Make**
    * *Linux/Mac* : Pré-installé.
    * *Windows* : Utilisez **WSL2** ou installez-le via `choco install make`.

---

## 🚀 Démarrage Rapide (Onboarding)

Nous avons automatisé l'installation. Cette commande va cloner les 11 repositories, générer les secrets de sécurité, configurer le réseau et lancer les conteneurs. Ce processus prend plus ou moins 2 minutes d'après nos tests dans différentes configurations. 

Ouvrez votre terminal dans le dossier où vous voulez cloner l'ensemble des microservices et lancez :

```bash
git clone https://github.com/RAM-Rogue-AI-Model/ram-infra.git
cd ram-infra
make init
```

---

## 📝 Commandes utiles

*   `make up` : Démarre l'ensemble des microservices et des bases de données en arrière-plan.
*   `make down` : Arrête tous les conteneurs et réseaux Docker créés.
*   `make logs` : Affiche les logs d'un conteneur donné.
*   **Plus de commandes** : Voir `make help`.



