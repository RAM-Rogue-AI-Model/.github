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
<img width="1013" height="782" alt="image" src="https://github.com/user-attachments/assets/8c24b048-f308-4caa-a23f-d9122b65bd30" />
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

Nous avons automatisé l'installation. Cette commande va cloner les 11 repositories, générer les secrets de sécurité, configurer le réseau et lancer les conteneurs.Le processus est un peu long, nous avons cherché à optimiser les images Docker pour un démarrage rapide, mais faute de temps, nous avons pas pu le faire, on s'excuse pour la gêne occasionnée. D'après nos tests dans différentes configurations, cela peut prendre entre 5 et 15 minutes, parfait pour prendre un petit café ☕️.

Ouvrez votre terminal dans le dossier où vous voulez cloner l'ensemble des microservices et lancez :

```bash
git clone https://github.com/RAM-Rogue-AI-Model/ram-infra.git
cd ram-infra
make init
```

Une fois l'installation terminée, tout les services devraient être en marche et le front accessible en local via [http://localhost:3000/ram](http://localhost:3000/ram), vous pouvez vérifier le bon état des services en tapant:

```bash
docker ps -a
```

En cas de soucis, vous pouvez jouer la commande ci-dessous, elle arrêtera tous les conteneurs et les relancera.

```bash
make down up
```

---

## 📝 Commandes utiles

*   `make up` : Démarre l'ensemble des microservices et des bases de données en arrière-plan.
*   `make down` : Arrête tous les conteneurs et réseaux Docker créés.
*   `make logs` : Affiche les logs d'un conteneur donné.
*   **Plus de commandes** : Voir `make help`.

---

## 📊 Tableau récapitulatif des services

| Service | Description | Localhost | Docs |
| :--- | :--- | :--- | :--- |
| **Front** | Interface utilisateur (React/Vite) | [http://localhost:3000/ram](http://localhost:3000/ram) | - |
| **API Gateway** | Point d'entrée unique & Redirection | [http://localhost:3001](http://localhost:3001) | [http://localhost:3001/docs](http://localhost:3001/docs) |
| **Battle** | Gestion des combats | [http://localhost:3002](http://localhost:3002) | [http://localhost:3002/docs](http://localhost:3002/docs) |
| **Effect** | Gestion des effets et statuts | [http://localhost:3003](http://localhost:3003) | [http://localhost:3003/docs](http://localhost:3003/docs) |
| **Enemy** | Gestion des ennemis | [http://localhost:3004](http://localhost:3004) | [http://localhost:3004/docs](http://localhost:3004/docs) |
| **Game** | Logique globale du jeu | [http://localhost:3005](http://localhost:3005) | [http://localhost:3005/docs](http://localhost:3005/docs) |
| **Item** | Gestion des objets et inventaire | [http://localhost:3006](http://localhost:3006) | [http://localhost:3006/docs](http://localhost:3006/docs) |
| **Logger** | Service de logs (RabbitMQ Consumer) | [http://localhost:3007](http://localhost:3007) | - |
| **Player** | Gestion des joueurs et stats | [http://localhost:3008](http://localhost:3008) | [http://localhost:3008/docs](http://localhost:3008/docs) |
| **User** | Authentification et comptes utilisateurs | [http://localhost:3009](http://localhost:3009) | [http://localhost:3009/docs](http://localhost:3009/docs) |
| **PhpMyAdmin** | Interface de gestion MariaDB | [http://localhost:8080](http://localhost:8080) | - |
| **RabbitMQ** | Interface de gestion des queues | [http://localhost:15672](http://localhost:15672) | - |

---

## ✨ Bonus & Qualité du Code

Pour garantir un projet robuste et maintenable, nous avons intégré plusieurs outils et pratiques :

*   **Interface soignée** : Un design moderne pour une expérience utilisateur agréable.
*   **ESLint** : Standardisation du style de code et détection d'erreurs potentielles dès le développement.
*   **SonarQube** : Analyse statique continue pour surveiller la qualité du code, la couverture de tests et la dette technique.
*   **Sécurité** : 
    *   Authentification sécurisée via **JWT** (JSON Web Tokens).
    *   Hachage des mots de passe.
    *   Validation stricte des données d'entrée pour prévenir les injections.
    *   Utilisation de variables d'environnement pour les données sensibles.
