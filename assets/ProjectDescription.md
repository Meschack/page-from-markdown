# Description

Le projet porte sur une application de gestion des congés administratifs et des demandes d'absence dans le ministère du plan et du développement.

Les agents des services du ministère doivent être capable de faire des demandes de congé ainsi que des demandes d'absence sur la plateforme. Il y a en tout 18 services différents.

Chaque agent a un solde de congé qui augmente de 30 jours chaque année et qui est de 0 pour la première année.

> Ça veut dire que si un agent prend du service le 01/01/2021, il ne pourra pas prendre de congés avant le 01/01/2022. Date à laquelle son solde de congés passera à 30 et il pourra prendre des congés s'il le souhaite. Le 01/01/2023, son solde sera de 30 jours plus son solde restant en 2022.

Il y a trois types de congés : les congés administratifs, les congés de paternité et les congés de maternité.

- Les congés administratifs sont déductibles du solde de congé de l'agent
- Les congés de paternité sont de trois jours et ne sont pas déductibles du solde de congé
- Les congés de maternité sont de quatorze semaine (soit 6 semaines avant le jour prévu pour l'accouchement et 8 semaines après celui ci) et ne sont pas déductibles du solde de congé

Concernant les absences, elles sont déductibles du solde de congé de l'agent.

À noter qu'un agent, quelque soit son solde, n'a droit qu'à 90 jours de congés au maximum dans une année.

Informations recueillies lors d'une demande de congé :

- Nom et Prénom de l'agent
- Corps / fonction de l'agent
- Structure de l'agent
- Le type de congé
- La date de départ
- La durée en jours du congé

À noter qu'un agent ne peut pas faire de demande de congé avant sa date d'ouverture du droit au congé (un an après sa prise de fonction)

Concernant les fonctionnalités à inclure dans l'application, il y a entre autres :

- la demande de congé ou d'absence par un agent
- le suivi de sa demande par un agent
- une consultation de son historique de demande
- la gestion (accord / refus) des demandes de congé ou d'absence par l'administrateur
- une génération d'un PDF récapitulant les informations d'une demande accordée
- une visualisation de l'ensemble des demandes congés et d'absence accordés par service

## Dictionnaire de données

Voici les dictionnaires de données pour chacune des tables sous la forme de tableaux :

### Table _Agent_

| Champ              | Type de données | Longueur | Description                              |
| ------------------ | --------------- | -------- | ---------------------------------------- |
| **id** 🔑          | Integer         |          | Identifiant unique de l'agent            |
| **nom**            | Varchar         | 255      | Nom de l'agent                           |
| **prenom**         | Varchar         | 255      | Prénom de l'agent                        |
| **corps_fonction** | Varchar         | 255      | Corps/fonction de l'agent                |
| **structure**      | Varchar         | 255      | Structure à laquelle l'agent est affilié |

### Table _Service_

| Champ           | Type de données | Longueur | Description                   |
| --------------- | --------------- | -------- | ----------------------------- |
| **id** 🔑       | Integer         |          | Identifiant unique du service |
| **nom_service** | Varchar         | 255      | Nom du service                |

### Table _Conge_

| Champ         | Type de données | Longueur | Description                 |
| ------------- | --------------- | -------- | --------------------------- |
| **id** 🔑     | Integer         |          | Identifiant unique du congé |
| **nom_conge** | Varchar         | 255      | Nom du congé                |

### Table _DemandeConge_

| Champ           | Type de données | Longueur | Description                                      |
| --------------- | --------------- | -------- | ------------------------------------------------ |
| **id** 🔑       | Integer         |          | Identifiant unique de la demande de congé        |
| **id_agent** 🗝️ | Integer         |          | Identifiant de l'agent lié à la demande de congé |
| **date_depart** | Date            |          | Date de départ de la demande de congé            |
| **duree**       | Integer         |          | Durée de la demande de congé en jours            |
| **statut**      | Varchar         | 255      | Statut de la demande de congé                    |

### Table _DemandeAbsence_

| Champ            | Type de données | Longueur | Description                                       |
| ---------------- | --------------- | -------- | ------------------------------------------------- |
| **id** 🔑        | Integer         |          | Identifiant unique de la demande d'absence        |
| **id_agent** 🗝️  | Integer         |          | Identifiant de l'agent lié à la demande d'absence |
| **date_depart**  | Date            |          | Date de départ de la demande d'absence            |
| **duree**        | Integer         |          | Durée de la demande d'absence en jours            |
| **raison**       | Varchar         | 255      | Raison de l'absence                               |
| **justificatif** | Varchar         | 255      | Chemin vers le justificatif de l'absence          |
| **statut**       | Varchar         | 255      | Statut de la demande d'absence                    |

## Modélisation

### Cas d'utilisation

#### Acteurs

- **Agent** : L'agent est l'utilisateur principal du système. Il est responsable de la gestion de ses propres demandes de congé et d'absence. Cet acteur interagit avec le système pour effectuer des demandes, suivre leur statut et consulter son historique de demandes.

- **Administrateur** : L'administrateur est un utilisateur ayant des privilèges supplémentaires par rapport aux agents. Il est responsable de la gestion globale des demandes de congé et d'absence. Cet acteur interagit avec le système pour gérer les demandes, les approuver ou les rejeter, générer des rapports et visualiser les statistiques.

#### Cas d'utilisation

- **Faire une demande de congé** : Permet à un agent de soumettre une demande de congé, en fournissant les informations nécessaires telles que la date de départ, la durée, le type de congé, etc. Cette fonctionnalité permet à l'agent de demander un congé ou une absence.

- **Suivre l'état d'une demande** : Permet à un agent de suivre l'état de ses demandes de congé et d'absence, en vérifiant si elles sont en attente, approuvées ou rejetées. Cela permet à l'agent de connaître l'avancement de ses demandes et de planifier en conséquence.

- **Consulter l'historique des demandes** : Permet à un agent de consulter l'historique de ses demandes de congé et d'absence précédentes. Cela permet à l'agent de vérifier les congés accordés précédemment et de faire référence à ses demandes passées.

- **Gérer les demandes de congé (administrateur)** : Permet à l'administrateur de gérer les demandes de congé soumises par les agents. Cela comprend la visualisation de toutes les demandes, l'approbation ou le rejet des demandes, ainsi que la mise à jour du statut des demandes.

- **Générer un rapport de demande de congé accordée (administrateur)** : Permet à l'administrateur de générer un rapport récapitulatif des demandes de congé approuvées. Ce rapport peut inclure des informations telles que les dates, les durées, les types de congé et les informations sur l'agent concerné.

- **Visualiser les statistiques des demandes par service (administrateur)** : Permet à l'administrateur de visualiser les statistiques globales des demandes de congé et d'absence par service. Cela peut inclure le nombre de demandes par service, les types de congé les plus courants, etc.

La justification de ces acteurs et cas d'utilisation est basée sur les besoins fonctionnels du système. L'acteur **_Agent_** est essentiel car c'est l'utilisateur principal qui interagit avec le système pour gérer ses propres demandes. L'acteur **_Administrateur_** est important pour la gestion globale du processus de demande de congé et d'absence. Les cas d'utilisation couvrent les fonctionnalités clés attendues, telles que la soumission des demandes, le suivi de l'état, la gestion des demandes par l'administrateur, la génération de rapports et la visualisation des statistiques.

![](../images/DiagrammeDeCasDUtilisation.jpg)

## Liste des tâches

### Mise en place des APIs

**Catégorie : Authentification et Autorisation**

- Mettre en place le système d'authentification pour les agents et l'administrateur
- Configurer les rôles et les permissions pour les différents utilisateurs
- Implémenter les middlewares pour vérifier les autorisations d'accès aux routes

**Catégorie : Gestion des demandes de congé**

- Créer les routes pour la gestion des demandes de congé (CRUD)
- Mettre en place les contrôleurs et les services correspondants
- Implémenter les fonctionnalités de création, lecture, mise à jour et suppression des demandes de congé
- Valider les demandes en fonction des règles spécifiées (date d'ouverture du droit au congé, solde, etc.)
- Gérer le suivi de l'état des demandes (en attente, approuvée, refusée)
- Générer les PDF récapitulatifs pour les demandes de congé approuvées

**Catégorie : Fonctionnalités administratives**

- Créer les routes pour les fonctionnalités administratives (génération de rapports, statistiques, etc.)
- Mettre en place les contrôleurs et les services correspondants
- Implémenter les fonctionnalités d'accès aux données statistiques (demandes accordées par service, etc.)
- Gérer les fonctionnalités d'approbation ou de rejet des demandes de congé par l'administrateur

---

**Gestion des agents :**

- Créer un modèle "Agent" avec les attributs appropriés.

**Gestion des services :**

- Créer un modèle "Service" avec les attributs appropriés.

**Gestion des types de congé :**

- Créer un modèle "Congé" avec les attributs appropriés.

**Gestion des demandes de congé :**

- Créer un modèle "DemandeCongé" avec les attributs appropriés.
- Mettre en place les API pour la création, la récupération et la mise à jour des demandes de congé.
- Implémenter la logique de gestion des demandes de congé (validation des dates, contraintes de solde, etc.).
- Implémenter les fonctionnalités de suivi de l'état des demandes de congé et de consultation de l'historique des demandes.

**Gestion des demandes par l'administrateur :**

- Mettre en place les API pour la gestion des demandes de congé par l'administrateur (approbation, refus, mise à jour du statut).
- Implémenter les fonctionnalités de génération de PDF récapitulatif des demandes de congé approuvées.

**Gestion des statistiques et des rapports :**

- Mettre en place les API pour la récupération des demandes de congé et d'absence accordées par service.
- Implémenter les fonctionnalités de visualisation des statistiques globales des demandes par service.

**Authentification et autorisation :**

- Mettre en place l'authentification basée sur un jeton (token-based authentication) pour sécuriser les API.
- Mettre en place les rôles d'accès (agent, administrateur) pour gérer les autorisations sur les différentes fonctionnalités.
