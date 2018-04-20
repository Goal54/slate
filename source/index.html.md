---
title: API MyClementine

language_tabs: # must be one of https://git.io/vQNgJ
  - Tous les languages

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

Bienvenue sur la documentation de l'API MyClementine. 

Vous pouvez utiliser notre API pour accéder à des points essentiels de MyClementine, pour obtenir des informations sur vos données bancaires, vos clients, votre compte etc...

Nous vous invitons à utiliser [Postman](https://www.getpostman.com/) pour faciliter votre développement, notre API y est compatible !

# Authentification

> Pour obtenir le jeton d'authentification :

```shell
curl "https://myclementine.fr/api/users/token
```
>Exemple de paramètres :

```json
{
    "username": "jeandupont",
    "password": "jd65790ayu!"
}
```

>Cet requête vous retourne une chaîne json contenant des champs comme ci-dessous : 

```json
{
"success": "true",
  "data": {
     "token" : "Zt7er43T3gm7qBE79jU9KXjK8F2r4iRxi3L3p9m3",
     "type_compta" : "0 en comptabilité trésorerie, 1 en comptabilité engagement",
     "society_id" : "12309011",
     "user": {
       "toutes les informations de l'utilisateur"
     }
  }
}

```

Pour pouvoir faire des requêtes à l'API, vous devez vous authentifier afin d'obtenir un token. Ce token devra être inclus dans chaque header des différentes requêtes.

En plus de recevoir un token, vous allez recevoir le type de comptabilité de l'utilisateur. 

Si l'utilisateur a une [comptabilité trésorerie](https://www.compta-facile.com/comptabilite-de-tresorerie/), vous recevrez 0.

Si l'utilisateur a une [comptabilité en engagement](https://www.compta-facile.com/comptabilite-d-engagement/), vous recevrez 1.

Ce paramètre est important. En effet certaines fonctions ne sont disponibles qu'en trésorerie et d'autres qu'en engagement.

### Requête HTTP

`POST https://myclementine.com/api/users/token`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json

### Paramètres

Champ | Type | Description
--------- | ------- | -----------
username | String | Identifiant de l'utilisateur.
password | String | Mot de passe de l'utilisateur

<aside class="notice">
N'oubliez pas de garder en mémoire le token qui devra être utilisé pour chaque prochaine requête ainsi que le type de comptabilité qui déterminera quelle fonction vous pouvez utiliser.
L'identifiant de la société (society_id) vous sera également utile pour certains types de requête.
</aside>

# AccountingFile

Liste de fonctions permettant d'effectuer des actions sur les documents comptables.

##getDetailAccountingFile
<aside class="warning">Disponible seulement en comptabilité Engagement.</aside>
Retourne le détail d'une facture avec le tiers, la catégorie et la TVA associé à celle-ci.

> Requête type :

```shell
curl "https://myclementine.fr/api/accounting_file/getDetailAccountingFile/:society_id/:file_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/accounting_file/getDetailAccountingFile/12309011/12345678987"
```

> Vous recevez en réponse un tableau JSON :

```json
{
"success": "true",
  "data": {
     "..." : "..."
  }
}
```

### Requête HTTP

`GET https://myclementine.fr/api/accounting_file/getDetailAccountingFile/:society_id/:file_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
society_id | String | Identifiant de la société.
file_id | String | Identifiant de la facture.

##getTvasByAccountingFile
<aside class="warning">Disponible seulement en comptabilité Engagement.</aside>
Retourne la liste des TVAS présentes sur MyClémentine et celles associés à la facture.

> Requête type :

```shell
curl "https://myclementine.fr/api/accounting_file/getTvasByAccountingFile/:file_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/accounting_file/getTvasByAccountingFile/3411"
```

> Vous recevez en réponse un tableau JSON :

```json
{
"success": "true",
  "data": {
     "tvas" : {
      //liste des TVAS
     }
     "eaf": {
        //détail de la facture
     }
  }
}
```

### Requête HTTP

`GET https://myclementine.fr/api/accounting_file/getTvasByAccountingFile/:file_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
file_id | String | Identifiant de la facture.

##listFilesPurchase
<aside class="warning">Disponible seulement en comptabilité Trésorerie.</aside>
Retourne la liste des factures d'achats en fonction d'un mois. Si aucune date n'est passée en paramètre, cela retourne les factures du mois actuel.

> Requête type :

```shell
curl "https://myclementine.fr/api/accounting_file/listFilesPurchase/:society_id/:month/:year"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/accounting_file/listFilesPurchase/123567/01/2018"
```

> Vous recevez en réponse un tableau JSON :

```json
{
"success": "true",
  "data": {
     //liste des factures d'achats
  }
}
```

### Requête HTTP

`GET https://myclementine.fr/api/accounting_file/listFilesPurchase/:society_id/:month/:year`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
society_id | int | Identifiant de la société.
month | int | Mois de la recherche.
year | int | Année de la recherche.

##listFilesSales
<aside class="warning">Disponible seulement en comptabilité Trésorerie.</aside>
Retourne la liste des factures de vente en fonction d'un mois. Si aucune date n'est passée en paramètre, cela retourne les factures du mois actuel.

> Requête type :

```shell
curl "https://myclementine.fr/api/accounting_file/listFilesSales/:society_id/:month/:year"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/accounting_file/listFilesSales/123567/01/2018"
```

> Vous recevez en réponse un tableau JSON :

```json
{
"success": "true",
  "data": {
     //liste des factures de vente
  }
}
```

### Requête HTTP

`GET https://myclementine.fr/api/accounting_file/listFilesSales/:society_id/:month/:year`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
society_id | int | Identifiant de la société.
month | int | Mois de la recherche.
year | int | Année de la recherche.

##listFilesVarious
<aside class="warning">Disponible seulement en comptabilité Trésorerie.</aside>
Retourne l'ensemble des documents divers.

> Requête type :

```shell
curl "https://myclementine.fr/api/accounting_file/listFilesVarious/:society_id/"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/accounting_file/listFilesVarious/123567"
```

> Vous recevez en réponse un tableau JSON :

```json
{
"success": "true",
  "data": {
     //liste des documents divers
  }
}
```

### Requête HTTP

`GET https://myclementine.fr/api/accounting_file/listFilesVarious/:society_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
society_id | int | Identifiant de la société.

##saveFileForTransaction
<aside class="warning">Disponible seulement en comptabilité Trésorerie.</aside>
Permet d'associer un fichier à une transaction bancaire.

> Requête type :

```shell
curl "https://myclementine.fr/api/accounting_file/saveFileForTransaction/:society_id/:transaction_id/:file_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/accounting_file/saveFileForTransaction/123567/1234567/098765432"
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true"
}
```

### Requête HTTP

`GET https://myclementine.fr/api/accounting_file/saveFileForTransaction/:society_id/:transaction_id/:file_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
society_id | int | Identifiant de la société.
transaction_id | int | Identifiant de la transaction.
file_id | int | Identifiant du fichier

##setAccount
<aside class="warning">Disponible seulement en comptabilité Engagement.</aside>
Associe une catégorie à une facture. Si une TVA est rattaché à la catégorie, on l'associera également à la facture.

> Requête type :

```shell
curl "https://myclementine.fr/api/accounting_file/setAccountEnga/:file_id/:account_id/"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/accounting_file/setAccountEnga/5175281/1/"
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true"
}
```

### Requête HTTP

`GET https://myclementine.fr/api/accounting_file/saveFileForTransaction/:society_id/:transaction_id/:file_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
society_id | int | Identifiant de la société.
transaction_id | int | Identifiant de la transaction.
file_id | int | Identifiant du fichier

#Tiers

##listAllTiers
Changement d'identifiant d'un compte utilisateur.
<aside class="notice">
L'identifiant de l'utilisateur peut être récupéré lors de la phase de récupération du token.
</aside>

> Requête type :

```shell
curl "https://myclementine.fr/api/users/editMyIdentifiant/:user_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/users/editMyIdentifiant/20562"
```

> Exemple de paramètres :

```json
{
  "firstname": "Jean",
  "lastname": "Dupont",
  "email": "jean.dupont@compta-clementine.fr"
}
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true"
}
```

### Requête HTTP

`POST https://myclementine.fr/api/users/editMyIdentifiant/:user_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
user_id | int | Identifiant de l'utilisateur.

### Paramètres

Champ | Type | Description
--------- | ------- | -----------
firstname | String | Nouveau prénom de l'utilisateur.
lastname | String | Nouveau nom de l'utilisateur.
email | String | Nouveau email de l'utilisateur.



#Users

##editMyIdentifiant
Changement d'identifiant d'un compte utilisateur.
<aside class="notice">
L'identifiant de l'utilisateur peut être récupéré lors de la phase de récupération du token.
</aside>

> Requête type :

```shell
curl "https://myclementine.fr/api/users/editMyIdentifiant/:user_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/users/editMyIdentifiant/20562"
```

> Exemple de paramètres :

```json
{
  "firstname": "Jean",
  "lastname": "Dupont",
  "email": "jean.dupont@compta-clementine.fr"
}
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true"
}
```

### Requête HTTP

`POST https://myclementine.fr/api/users/editMyIdentifiant/:user_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
user_id | int | Identifiant de l'utilisateur.

### Paramètres

Champ | Type | Description
--------- | ------- | -----------
firstname | String | Nouveau prénom de l'utilisateur.
lastname | String | Nouveau nom de l'utilisateur.
email | String | Nouveau email de l'utilisateur.

##editMyLogo
Changement du logo de la société.

> Requête type :

```shell
curl "https://myclementine.fr/api/users/editMyLogo/:society_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/users/editMyLogo/1254691"
```

> Exemple de paramètres :

```json
{
  "image_r": "my_logo.png"
}
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true",
}
```

### Requête HTTP

`POST https://myclementine.fr/api/users/editMyLogo/:society_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
society_id | int | Identifiant de la société.

### Paramètres

Champ | Type | Description
--------- | ------- | -----------
image_r | File | Nouveau logo de la société.


##editMyPassword
Changement de mot passe d'un compte utilisateur. 
<aside class="notice">
L'identifiant de l'utilisateur peut être récupéré lors de la phase de récupération du token.
</aside>

> Requête type :

```shell
curl "https://myclementine.fr/api/users/editMyPassword/:user_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/users/editMyPassword/20562"
```

> Exemple de paramètres :

```json
{
  "password": "motdepasse",
  "password_confirm": "motdepasse"
}
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true"
}
```

### Requête HTTP

`POST https://myclementine.fr/api/users/editMyPassword/:user_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
user_id | int | Identifiant de l'utilisateur.

### Paramètres

Champ | Type | Description
--------- | ------- | -----------
password | String | Nouveau mot de passe de l'utilisateur.
password_confirm | String | Confirmation mot de passe de l'utilisateur.

##editMySociety
Changement des informations d'une société.
<aside class="notice">
L'ensemble des informations modifiables d'une société sont disponibles, lors de la phase de récupération du token.
L'identifiant de l'utilisateur peut être récupéré lors de la phase de récupération du token.
</aside>

> Requête type :

```shell
curl "https://myclementine.fr/api/users/editMySociety/:user_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/users/editMySociety/:user_id"
```

> Exemple de paramètres :

```json
{
  "phone": "0123456789",
  "exemple": "exemple"
}
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true",
  "society": {
    "..." : "..."
  }
}
```

### Requête HTTP

`POST https://myclementine.fr/api/users/editMySociety/:user_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
user_id | int | Identifiant de l'utilisateur.

### Paramètres

Champ | Type | Description
--------- | ------- | -----------
society | Array | Nouvelles informations de la société.

##getProfil
Retourne les informations d'un utilisateur.
<aside class="notice">
L'identifiant de l'utilisateur peut être récupéré lors de la phase de récupération du token.
</aside>

> Requête type :

```shell
curl "https://myclementine.fr/api/users/getProfil/:user_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/users/getProfil/120903"
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true",
  "data": {
    "user" : {
      "..." : "..."
    }
  }
}
```

### Requête HTTP

`POST https://myclementine.fr/api/users/getProfil/:user_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
user_id | int | Identifiant de l'utilisateur.

##getSociety
Retourne les informations d'une société.
<aside class="notice">
L'identifiant de la société peut être récupéré lors de la phase de récupération du token.
</aside>

> Requête type :

```shell
curl "https://myclementine.fr/api/users/getSociety/:society_id"
```

> Exemple de requête :

```shell
curl "https://myclementine.fr/api/users/getSociety/189622"
```

> Vous recevez en réponse un tableau JSON :

```json
{
  "success": "true",
  "data": {
    //Informations sur la société
  }
}
```

### Requête HTTP

`POST https://myclementine.fr/api/users/getProfil/:user_id`

### Header

Champ | Type | Description
--------- | ------- | -----------
Accept | String | application/json
Authorization | String | Bearer :token

### Paramètres URL

Champ | Type | Description
--------- | ------- | -----------
society_id | int | Identifiant de la société.
