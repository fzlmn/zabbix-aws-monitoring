# Mise en œuvre d'une infrastructure cloud de supervision centralisée sous AWS

## Déploiement de Zabbix conteneurisé pour le monitoring d'un parc hybride (Linux & Windows)

---

## 1. Introduction

Ce projet a pour objectif de mettre en œuvre une infrastructure de supervision centralisée dans le cloud AWS en utilisant **Zabbix déployé sous forme de conteneurs Docker**. La solution permet de surveiller un parc informatique hybride composé de **machines Linux et Windows**, tout en respectant les contraintes du Learner Lab AWS.

Les technologies utilisées dans ce projet sont :

* **Amazon Web Services (AWS)** pour l’infrastructure cloud
* **Docker & Docker Compose** pour la conteneurisation
* **Zabbix** pour la supervision et le monitoring
* **Linux (Ubuntu)** et **Windows Server** comme systèmes supervisés

---

## 2.1 Architecture Réseau

L’architecture réseau repose sur une infrastructure simple afin de faciliter l’accès sans VPN.

* **VPC unique**
* **Un sous-réseau public**
* **Groupes de sécurité** autorisant les ports suivants :

  * 80 / 443 : Interface Web Zabbix
  * 10050 / 10051 : Communication Zabbix Agent / Server
  * 22 : Accès SSH (Linux)
  * 3389 : Accès RDP (Windows)

![1](https://github.com/user-attachments/assets/d9fdf2d9-1e27-478e-9cc8-b948fa787478)

![2](https://github.com/user-attachments/assets/8f0c1657-5ef8-4a22-b263-22f134137a13)

## 2.2 Création et attachement de l’Internet Gateway

Une Internet Gateway a été créée puis attachée au VPC afin de permettre aux instances EC2 d’accéder à Internet.

Actions réalisées :

Création de l’Internet Gateway

Attachement au VPC principal

![3 1](https://github.com/user-attachments/assets/ce9ccff0-205e-4efc-8745-e2176e5f4e4f)

## 2.3 Configuration de la Route Table

Une table de routage a été configurée afin de permettre le trafic sortant vers Internet via l’Internet Gateway.

Actions réalisées :

Ajout de la route 0.0.0.0/0 vers l’Internet Gateway

Association de la Route Table au subnet public

![3 2](https://github.com/user-attachments/assets/e8233ab3-2fe8-4670-b7c2-a775823231b7)

## 2.4 Configuration des Security Groups

Des groupes de sécurité ont été configurés afin d’autoriser uniquement les ports nécessaires au fonctionnement de l’infrastructure.

Ports autorisés :

80 / 443 : Interface Web Zabbix

10050 / 10051 : Communication Agents / Serveur Zabbix

22 : Accès SSH (Linux)

3389 : Accès RDP (Windows)

![4 1](https://github.com/user-attachments/assets/ae0d5713-ef84-4855-a281-cf9cbc552d7a)

![4 2](https://github.com/user-attachments/assets/13677cda-c34e-463e-845b-67b7fbf30e8a)

---


## 3. Architecture des Instances EC2

Trois instances EC2 ont été déployées :

### 3.1 Serveur Zabbix

* Type : **t3.medium**
* Système : **Ubuntu Server**
* Rôle : Hébergement des conteneurs Zabbix (Server, Web, Base de données)

### 3.2 Client Linux

* Type : **t3.medium**
* Système : **Ubuntu Server**
* Rôle : Machine supervisée via Zabbix Agent Linux

### 3.3 Client Windows

* Type : **t3.medium**
* Système : **Windows Server**
* Rôle : Machine supervisée via Zabbix Agent Windows

![5](https://github.com/user-attachments/assets/d68bad26-9f32-4d45-a747-05781eaa9cbe)

![6](https://github.com/user-attachments/assets/72745130-f33a-4892-bdf2-f777b5f36241)

![7](https://github.com/user-attachments/assets/8c144d7b-fd40-449a-9a45-5c94bf5ea882)


---


## 4. Déploiement du Serveur Zabbix

### 4.1 Installation de Docker et Docker Compose

Docker et Docker Compose ont été installés sur le serveur Ubuntu afin de déployer Zabbix sous forme de conteneurs.

### 4.2 Lancement des conteneurs

Les services suivants ont été déployés via **Docker Compose** :

* Zabbix Server
* Zabbix Web Interface
* Base de données (MySQL/PostgreSQL)

![11](https://github.com/user-attachments/assets/7e53e180-5310-48dc-aeeb-5a61d46ffd21)


### 4.3 Accès à l’interface Web

L’interface Web Zabbix est accessible via l’adresse IP publique du serveur.

![13](https://github.com/user-attachments/assets/292528ea-d468-4de8-b61e-a4ca7297c731)


---

## 5. Configuration des Clients (Agents)

### 5.1 Installation de l’agent Linux

L’agent Zabbix a été installé sur la machine Ubuntu cliente et configuré pour communiquer avec le serveur Zabbix.

![15](https://github.com/user-attachments/assets/b401c50d-266e-4864-9b93-e6293d9c3fda)


### 5.2 Installation de l’agent Windows

L’agent Zabbix Windows a été installé via l’assistant d’installation. Les paramètres du serveur Zabbix ont été renseignés.

![16](https://github.com/user-attachments/assets/ad9af818-b5a7-4387-8230-6f7e1190f2a3)


---

## 6. Monitoring et Tableaux de Bord

### 6.1 Ajout des hôtes

Les machines Linux et Windows ont été ajoutées dans l’interface Zabbix et associées aux templates appropriés.

![18](https://github.com/user-attachments/assets/5bbf76ad-cc10-409b-ad9f-12792e61c999)


### 6.2 Supervision en temps réel

Les données de supervision (CPU, RAM, réseau) sont affichées en temps réel dans l’onglet **Monitoring > Latest Data**.

![19](https://github.com/user-attachments/assets/aab76c4f-820e-409f-bc35-04c35a479d53)

![21](https://github.com/user-attachments/assets/98184e9e-667e-4a92-a623-11cf7de93e42)

![20](https://github.com/user-attachments/assets/9749d7e3-ad90-424e-80f5-e0e659c525d2)


---

## 7. Conclusion

Ce projet a permis de :

* Comprendre le déploiement d’une infrastructure cloud sur AWS
* Mettre en œuvre Zabbix sous Docker
* Superviser un parc hybride Linux & Windows
* Gérer des agents, des hôtes et des alertes

### Difficultés rencontrées

* Configuration réseau et Security Groups
* Communication entre agents et serveur
* Gestion des conteneurs Docker

### Solutions apportées

* Vérification des ports ouverts
* Correction des fichiers de configuration
* Tests de connectivité entre les machines



---

📌 **Auteur :** Fatima Zahra

📌 **Encadrant :** Prof. Azeddine KHIAT

📌 **Année universitaire :** 2025/2026
