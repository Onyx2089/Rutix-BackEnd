# Endpoints

Les endpoints, ou points de terminaison, sont des URL spécifiques auxquelles une application peut envoyer des requêtes pour accéder aux ressources ou services fournis par un serveur. Ils jouent un rôle crucial dans les architectures API (Interface de Programmation d'Application) en permettant la communication entre différents systèmes et applications.

# **Authentification des utilisateurs**

## **1. Créer un utilisateur**

Méthode : POST
URL : /user/register/
Description: Création d’un compte d’utilisateur avec les champs nécessaire 
Authentification : En tant que ROLE_USER ou ROLE_ADMIN

| **Champs** | **Type** | **Obligatoire** | **Description** |
| --- | --- | --- | --- |
| username | string | oui | nom d’utilisateur |
| password | string | oui | mot de passe |
| firstname | string | oui | prénom |
| lastname | string | oui | nom de famille |
| email | string | oui | adresse mail |
| numberphone | string | non | numéro de téléphone |
| country | string | non | pays |
| postalcode | string | non | code postal |
| city | string | non | ville |
| adress | text | non | adresse |

Exemple de json pour l’appel API : 

```json
{
  "username": "johndoe",
  "password": "securepassword",
  "firstname": "John",
  "lastname": "Doe",
  "email": "johndoe@example.com",
  "numberphone": "1234567890",
  "country": "France",
  "postalcode": "75001",
  "city": "Paris",
  "adress": "1 Rue de Rivoli"
}
```

**Réponse en cas de succès :** 
Code : 201
Description : Le compte d’utilisateur à bien été créé

```json
{
    "id": 1,
    "username": "johndoe",
    "roles": [
        "ROLE_USER"
    ],
    "password": "$2y$13$AOYek3XF30uEu7wKqQ38rO2AGe.L7ZiWL1APksTC4bddWbECi3Sum",
    "firstname": "John",
    "lastname": "Doe",
    "email": "johndoe@example.com",
    "phonenumber": null,
    "country": "France",
    "postalcode": "75001",
    "city": "Paris",
    "adress": "1 Rue de Rivoli",
    "CreationDate": {
        "date": "2024-07-04 14:42:47.184061",
        "timezone_type": 3,
        "timezone": "UTC"
    },
    "UpdatedDate": {
        "date": "2024-07-04 14:42:47.184063",
        "timezone_type": 3,
        "timezone": "UTC"
    }
}
```

**Réponse en cas d’erreur :** 
Code : 400
Description : Le système à évité une duplication de l’adresse mail

```json
{
    "error": "An exception occurred while executing a query: SQLSTATE[23000]: Integrity constraint violation: 1062 Duplicate entry 'johndoe@example.com' for key 'UNIQ_8D93D649E7927C74'"
}
```

 

## **2. Authentification d'un utilisateur**

Méthode : POST
URL : /user/authenticate/ 
Description: Récupération d’un token d’autentification d’un compte utilisateur
Authentification : En tant que ROLE_USER ou ROLE_ADMIN

| **Champs** | **Type** | **Obligatoire** | **Description** |
| --- | --- | --- | --- |
| username | string | nom d’utilisateur ou email | nom d’utilisateur |
| email | string | email ou nom d’utilisateur | adresse mail |
| password | string | oui | mot de passe |

Exemple de json pour l’appel API : 

```json
{
  "username": "johndoe",
  "password": "securepassword"
}
```

```json
{
  "email": "johndoe@example.com",
  "password": "securepassword"
}
```

**Réponse en cas de succès :** 
Code : 200
Description : L’utilisateur à bien été authentifié et le token envoyé

```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpYXQiOjE3MTk1ODYzNDMsImV4cCI6MTcxOTU4OTY0OSwicm9sZXMiOlsiUk9MRV9VU0VSIl0sInVzZXJuYW1lIjoiam9obmRvZSJ9.aI7eiP3lCV74fcp8TbuxyYQ3pj5DjMZsXD_h7XAzuREA_aAp4gxsuQnBmlLxOaRuBn5RE9TH5CV6Pa4UiYwIrHljcT6CISIE_ORj9NfQUgX7Gxy5RstzAxyoMQjU5pcZIcTFId1McpLJ6wKtqt656ZOClDnfG0i9AKFZRZFLPf3AGAcVGbmHxL9fexF-Qtx7ZCtiX_kCShUNqXsJ5hBBx_CdRQhtp63QqqqpZridkkB0NQvJq7utRvUbqJVb-9QKjFjk3qFrXwU8aRtwgkK_yvnz-06uGlh3fxt0FjRmngZKH8yK3iAaQls3N47XFUnWUKv8y7reHRiJaxm8PrCZ41I8wcBY9tEJSM75vEccZQdimJCWKmYDllLfrdJg1_tfaamqJWrmQ8gpHS5JR4OU7QDwi800eA9_2BCs1lPOnuCmThzqlDxR0GVYR0SbJhBTRNEpcigVux3yH3UT6ui6ZCf2Hb_jRAuKRQmrcW7tMu-MgtIyOTDXfjkO6WaN58nKpeJaNywUmoZjESbsuqm8cKHaU0BTZeS2jGXDT43EXpEZXGezySiCozjy6mgjjbUm6a7kFPI8uQXE9ts6-FlGgTwznlcUOaAajISAGIESwkoIa7gnBAkcbECoOxAnD0fNhE4-QmpZeoJtWhDAKr21Xc4VAfO_UGwhSFaWTNkOHzI"
}
```

**Réponse en cas d’erreur :** 
Code : 401
Description : Des informations de connexion sont manquantes

```json
{
    "error": "Missing informations"
}
```

# **Gestion des utilisateurs**

## **1. Afficher un utilisateur**

Méthode : GET
URL : /user/{id}
Description: Récupération d’un utilisateur selon l’id de l’url
Authentification : En tant que ROLE_USER ou ROLE_ADMIN

| **Champs** | **Type** | **Obligatoire** | **Description** |
| --- | --- | --- | --- |
| id | integer | oui | id de l’utilisateur |

**Réponse en cas de succès :** 
Code : 200
Description : L’utilisateur à bien été trouvé et envoyé

```json
{
    "id": 1,
    "username": "johndoe",
    "roles": [
        "ROLE_USER"
    ],
    "password": "$2y$13$IRE6t/Nb1oBxvbttUiwNHucW73EazgQxe.05Qg77AJYhbm4moPvaC",
    "firstname": "John",
    "lastname": "Doe",
    "email": "johndoe@example.com",
    "phonenumber": null,
    "country": "France",
    "postalcode": "75001",
    "city": "Paris",
    "adress": "1 Rue de Rivoli",
    "CreationDate": {
        "date": "2024-06-28 14:36:43.000000",
        "timezone_type": 3,
        "timezone": "UTC"
    },
    "UpdatedDate": {
        "date": "2024-06-28 14:36:43.000000",
        "timezone_type": 3,
        "timezone": "UTC"
    }
}
```

**Réponse en cas d’erreur :** 
Code : 404
Description : L’utilisateur n’a pas été trouvé

```json
{
    "msg": "Not found"
}
```

 

## **2. Lister les utilisateurs**

Méthode : GET
URL : /user/list
Description: Récupération de tous les utilisateurs 
Authentification : En tant que ROLE_USER ou ROLE_ADMIN

**Réponse en cas de succès :** 
Code : 200
Description : Les utilisateurs ont bien été trouvés et envoyés

```json
[
    {
        "id": 3,
        "username": "johndoe",
        "roles": [
            "ROLE_USER"
        ],
        "password": "$2y$13$IRE6t/Nb1oBxvbttUiwNHucW73EazgQxe.05Qg77AJYhbm4moPvaC",
        "firstname": "John",
        "lastname": "Doe",
        "email": "johndoe@example.com",
        "phonenumber": null,
        "country": "France",
        "postalcode": "75001",
        "city": "Paris",
        "adress": "1 Rue de Rivoli",
        "CreationDate": {
            "date": "2024-06-28 14:36:43.000000",
            "timezone_type": 3,
            "timezone": "UTC"
        },
        "UpdatedDate": {
            "date": "2024-06-28 14:36:43.000000",
            "timezone_type": 3,
            "timezone": "UTC"
        }
    }
]
```

**Réponse en cas d’erreur :** 
Code : 404
Description : Il n’y a aucun utilisateurs dans le système

```json
{
    "msg": "Zero users"
}
```

## **3. Modification d’un utilisateur**

Méthode : PUT
URL : /user/update/{id}
Description: Mise à jour d’un utilisateur selon les nouvelles données envoyés
Authentification : En tant que ROLE_USER ou ROLE_ADMIN

| **Champs** | **Type** | **Obligatoire** | **Description** |
| --- | --- | --- | --- |
| id | integer | oui | id de l’utilisateur |
| username | string | oui | pseudo d’utilisateur |
| password | string | oui | mot de passe |
| firstname | string | oui | prénom |
| lastname | string | oui | nom de famille |
| email | string | oui | adresse mail |
| phonenumber | string | non | numéro de téléphone |
| country | string | non | pays |
| postalcode | string | non | code postal |
| city | string | non | ville |
| adress | text | non | adresse |

Exemple de json pour l’appel API : 

```json
{
  "username": "janedoe",
  "password": "anothersecurepassword",
  "firstname": "Jane",
  "lastname": "Doe",
  "email": "janedoe@example.com",
  "numberphone": "0987654321",
  "country": "Canada",
  "postalcode": "H3Z 2Y7",
  "city": "Montreal",
  "adress": "123 Maple Street"
}
```

 
**Réponse en cas de succès :** 
Code : 200
Description : L’utilisateur à bien été mis à jour

```json
{
    "id": 3,
    "username": "janedoe",
    "roles": [
        "ROLE_USER"
    ],
    "password": "$2y$13$.zvO5FiPzmrOJamuCCpQy.ydZsAM3C90RIWkl/YIyDZC.7hOqloBG",
    "firstname": "Jane",
    "lastname": "Doe",
    "email": "janedoe@example.com",
    "phonenumber": null,
    "country": "Canada",
    "postalcode": "H3Z 2Y7",
    "city": "Montreal",
    "adress": "123 Maple Street",
    "CreationDate": {
        "date": "2024-06-28 14:36:43.000000",
        "timezone_type": 3,
        "timezone": "UTC"
    },
    "UpdatedDate": {
        "date": "2024-07-01 14:04:04.145159",
        "timezone_type": 3,
        "timezone": "UTC"
    }
}
```

 **Réponse en cas d’erreur :** 
Code : 400
Description : Une erreur à été détecté lors de la modification et le système à été arrêté 

```json
{
    "error": "EREUR"
}
```

Code : 404
Description : L’utilisateur n'à pas été trouvé

```json
{
    "msg": "not found"
}
```

## **4. Modification du rôle d’un utilisateur**

Méthode : PUT
URL : /user/update-role/{id}
Description: Mise à jour du rôle d’un utilisateur 
Authentification : En tant que ROLE_USER ou ROLE_ADMIN

| **Champs** | **Type** | **Obligatoire** | **Description** | **Valeur** |
| --- | --- | --- | --- | --- |
| id | integer | oui | id de l’utilisateur |  |
| role | string | oui | rôle de l’utilisateur | “user” ou “admin” |

Exemple de json pour l’appel API : 

```json
{
    "role": "admin"
}
```

```json
{
    "role": "user"
}
```

**Réponse en cas de succès :** 
Code : 
Description : L’utilisateur à bien été modifié en tant qu’administrateur

```json
{
    "id": 3,
    "username": "janedoe",
    "roles": [
        "ROLE_ADMIN"
    ],
    "password": "$2y$13$.zvO5FiPzmrOJamuCCpQy.ydZsAM3C90RIWkl/YIyDZC.7hOqloBG",
    "firstname": "Jane",
    "lastname": "Doe",
    "email": "janedoe@example.com",
    "phonenumber": null,
    "country": "Canada",
    "postalcode": "H3Z 2Y7",
    "city": "Montreal",
    "adress": "123 Maple Street",
    "CreationDate": {
        "date": "2024-06-28 14:36:43.000000",
        "timezone_type": 3,
        "timezone": "UTC"
    },
    "UpdatedDate": {
        "date": "2024-07-01 14:27:53.427862",
        "timezone_type": 3,
        "timezone": "UTC"
    }
}
```

**Réponse en cas d’erreur :** 
Code : 404
Description : L’utilisateur n’a pas été trouvé

```json
{
    "msg": "not found"
}
```

Code : 404
Description : Le champs du rôle à détecté un valeur non autorisé, le champs rôle peut être “user” ou “admin”

```json
{
    "msg": "Choose beetwen role user or admin"
}
```

## **5. Modification de l’avatar d’un utilisateur**

Méthode : PUT
URL : /user/update-avatar/{avatarFile}
Description: Mise à jour du rôle d’un utilisateur 
Authentification : En tant que ROLE_USER ou ROLE_ADMIN

| **Champs** | **Type** | **Obligatoire** | **Description** | **Valeur** |
| --- | --- | --- | --- | --- |
| avatarFile | string | non | avatar de l’utilisateur | *.png |

Exemple pour l’appel API : 

```bash
http://localhost:8090/user/update-avatar/9cb497f79fb15740b389564c5bbf269b.png
```

**Réponse en cas de succès :** 
Code : 
Description : L’utilisateur à bien été modifié en tant qu’administrateur

```json
{
    "id": 3,
    "username": "janedoe",
    "roles": [
        "ROLE_ADMIN"
    ],
    "password": "$2y$13$.zvO5FiPzmrOJamuCCpQy.ydZsAM3C90RIWkl/YIyDZC.7hOqloBG",
    "firstname": "Jane",
    "lastname": "Doe",
    "email": "janedoe@example.com",
    "phonenumber": null,
    "country": "Canada",
    "postalcode": "H3Z 2Y7",
    "city": "Montreal",
    "adress": "123 Maple Street",
    "avatarFile": "9cb497f79fb15740b389564c5bbf269b.png",
    "CreationDate": {
        "date": "2024-06-28 14:36:43.000000",
        "timezone_type": 3,
        "timezone": "UTC"
    },
    "UpdatedDate": {
        "date": "2024-07-01 14:27:53.427862",
        "timezone_type": 3,
        "timezone": "UTC"
    }
}
```

## **6. Suppression d’un utilisateur**

Méthode : DELETE
URL : /user/delete/{id}
Description: Suppression d’un utilisateur définitive du système
Authentification : En tant que ROLE_USER ou ROLE_ADMIN

| **Champs** | **Type** | **Obligatoire** | **Description** |
| --- | --- | --- | --- |
| id | integer | Oui | id de l’utilisateur |

**Réponse en cas de succès :** 
Code : 200
Description : L’utilisateur à bien été supprimé du système

```json
{
    "success": true
}
```

**Réponse en cas d’erreur :** 
Code : 500
Description : Une erreur à été rencontré lors de la suppression de l'utilisateur 

```json
{
    "success": false
}
```

Code : 404
Description : L’utilisateur n’a pas été trouvé

```json
{
    "msg": "not found"
}
```

 

# **Gestion des avatars**

## **1. Lister les avatars**

Méthode : GET
URL : /avatar/list
Description: Récupération de tous les avatars

**Réponse en cas de succès :** 
Code : 200
Description : Liste des avatars dans les fichier static

```json
{
    "049ec39b121d26f40a9bf5620a821857.png": "/img/avatar/049ec39b121d26f40a9bf5620a821857.png",
    "932bf794d8f336aaf1b64286b55c2d13.png": "https://localhost:8090/img/avatar/932bf794d8f336aaf1b64286b55c2d13.png",
    "9cb497f79fb15740b389564c5bbf269b.png": "https://localhost:8090/img/avatar/9cb497f79fb15740b389564c5bbf269b.png",
    "afd7e2d528ac4fe93beccf6d90babb99.png": "https://localhost:8090/img/avatar/afd7e2d528ac4fe93beccf6d90babb99.png"
}
```

# Arbre des questions

## **1. Récupérer la première question**

Méthode : GET
URL : /user-response/first-question
Description: Récupération de la première question avec les réponses possibles

**Réponse en cas de succès :** 
Code : 200
Description : Première question, liste des réponses et end-point à appeler

```json
{
    "question": "À quel moment de la journée as-tu le plus de mal à te concentrer ?",
    "answer": [
        "Matin",
        "Après-midi",
        "Soir"
    ],
    "url": "user-response/next-question/1"
}
```

## **2. Récupérer la prochaine question**

Méthode : POST
URL : /user-response/next-question/1
Description: Récupération de la question suivante avec les réponses possibles

Exemple de json pour l’appel API : 

```json
{
    "answer": "Matin"
}
```

**Réponse en cas de succès :** 
Code : 200 ou 201
Description : Première question, liste des réponses et end-point à appeler

```json
{
    "question": "As-tu du mal à te réveiller le matin ?",
    "answer": [
        "Oui",
        "Non"
    ],
    "url": "user-response/next-question/2"
}
```

**Réponse en cas de succès :** 
Code : 200
Description : Il n’y a plus de question à répondre

```json
{
    "question": null,
    "answer": null,
    "url": null
}
```

# User routine

## **1. Récupérer des routines**

Méthode : GET
URL : /routine/get-by-user
Description: Récupération des routines de l’utilisateur avec un token

**Réponse en cas de succès :** 
Code : 200
Description : Liste des routines de l’utilisateur

```json
[
    {
        "id": 1,
        "name": "deconnexion_numerique",
        "description": "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        "days": [
            1,
            2,
            3,
            4,
            5
        ],
        "taskTime": "1970-01-01T20:00:00+00:00",
        "isAllTaskGenerated": true,
        "creationDate": "2025-06-15T20:30:43+00:00",
        "updatedDate": "2025-06-15T20:30:43+00:00"
    },
    {
        "id": 2,
        "name": "entretien_chien",
        "description": "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        "days": [
            1,
            2,
            3,
            4,
            5
        ],
        "taskTime": "1970-01-01T20:00:00+00:00",
        "isAllTaskGenerated": true,
        "creationDate": "2025-06-15T20:33:02+00:00",
        "updatedDate": "2025-06-15T20:33:02+00:00"
    }
]
```

# User task

## **1. Récupérer les tâches**

Méthode : GET
URL : /user-task/get-by-user
Description: Récupération des tâches de l’utilisateur avec un token

**Réponse en cas de succès :** 
Code : 200
Description : Liste des routines de l’utilisateur

```json
[
    {
        "id": 1,
        "name": "deconnexion_numerique",
        "description": "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        "taskDateTime": "2025-06-16T20:00:00+00:00",
        "status": "0",
        "creationDate": "2025-06-15T20:30:43+00:00",
        "updatedDate": "2025-06-15T20:30:43+00:00"
    },
    {
        "id": 2,
        "name": "deconnexion_numerique",
        "description": "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        "taskDateTime": "2025-06-17T20:00:00+00:00",
        "status": "0",
        "creationDate": "2025-06-15T20:30:43+00:00",
        "updatedDate": "2025-06-15T20:30:43+00:00"
    },
    ...
]
```

## **2. Récupérer les tâches selon la date et l’heure**

Méthode : GET
URL : /user-task/get-by-user-and-datetime/{dateTime}
Description: Récupération des tâches de l’utilisateur avec la date, l’heure et un token

Exemple d’appel API : 

```bash
/user-task/get-by-user-and-datetime/2025-07-14_20:00:00
```

**Réponse en cas de succès :** 
Code : 200
Description : Liste des routines de l’utilisateur

```json
[
    {
        "id": 1,
        "name": "deconnexion_numerique",
        "description": "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        "taskDateTime": "2025-07-14T20:00:00+00:00",
        "status": "0",
        "creationDate": "2025-06-15T20:30:43+00:00",
        "updatedDate": "2025-06-15T20:30:43+00:00"
    },
    {
        "id": 6,
        "name": "entretien_chien",
        "description": "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        "taskDateTime": "2025-07-14T20:00:00+00:00",
        "status": "0",
        "creationDate": "2025-06-15T20:33:02+00:00",
        "updatedDate": "2025-06-15T20:33:02+00:00"
    }
]
```

## **3. Récupérer les tâches selon la date**

Méthode : GET
URL : /user-task/get-by-user-and-date/{date}
Description: Récupération des tâches de l’utilisateur avec la date avec un token

Exemple d’appel API : 

```bash
/user-task/get-by-user-and-date/2025-07-14
```

**Réponse en cas de succès :** 
Code : 200
Description : Liste des routines de l’utilisateur

```json
[
    {
        "id": 1,
        "name": "reveil_stimulant",
        "description": "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        "taskDateTime": "2025-07-09T07:30:00+00:00",
        "status": 0,
        "creationDate": "2025-07-09T17:45:38+00:00",
        "updatedDate": "2025-07-09T17:45:38+00:00"
    },
    {
        "id": 7,
        "name": "entretien_chien",
        "description": "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        "taskDateTime": "2025-07-09T20:00:00+00:00",
        "status": 0,
        "creationDate": "2025-07-09T17:47:36+00:00",
        "updatedDate": "2025-07-09T17:47:36+00:00"
    }
]
```

## **4. Création d’une tâche**

Méthode : GET
URL : /user-task/create
Description: Création d’une tâche pour un utilisateur avec le nom, la description, la dateTime un token

Exemple d’appel API : 

```bash
/user-task/update/v2/15
```

Avec le json : 

```json
{
    "name": "Sortie du chien",
    "description": "Balade dans le parc",
    "dateTime": "2025-07-10 20:00"
}
```

**Réponse en cas de succès :** 
Code : 200
Description : Liste des routines de l’utilisateur

```json
{
    "id": 16,
    "name": "Sortie du chien",
    "description": "Balade dans le parc",
    "taskDateTime": "2025-07-10T20:00:00+00:00",
    "status": 0,
    "creationDate": "2025-07-09T18:48:38+00:00",
    "updatedDate": "2025-07-09T18:48:38+00:00"
}
```

## **5. Modification d’une tâche**

Méthode : GET
URL : /user-task/update/{task}

Description: Création d’une tâche pour un utilisateur avec le nom, la description, la dateTime un token

Exemple de json pour l’appel API : 

```json
{
    "name": "Sortie de Max",
    "description": "Balade près de la rivière",
    "dateTime": "2025-07-12 10:00",
    "status": false
}
```

**Réponse en cas de succès :** 
Code : 200
Description : Liste des routines de l’utilisateur

```json
{
    "id": 16,
    "name": "Sortie du chien",
    "description": "Balade près de la rivière",
    "taskDateTime": "2025-07-12T10:00:00+00:00",
    "status": 0,
    "creationDate": "2025-07-09T18:48:38+00:00",
    "updatedDate": "2025-07-09T18:48:38+00:00"
}
```