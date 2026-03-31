# 01 — Concepts fondamentaux d'Active Directory

> Définitions et protocoles clés de l'infrastructure `steventech.local`

---

## 🌐 DNS — Domain Name System

Le serveur **DNS** traduit les noms de domaine en adresses IP. C'est un rôle indispensable au fonctionnement du réseau et d'Active Directory.

Sans DNS, les machines du domaine ne peuvent pas se localiser entre elles. C'est pourquoi il est installé automatiquement avec le rôle AD DS lors de la promotion du serveur en contrôleur de domaine.

**Fonctionnement :**
1. Un client émet une requête avec un nom (`SRV-STEVEN.steventech.local`)
2. Le serveur DNS consulte son cache ou ses zones
3. Il retourne l'adresse IP correspondante

> Sur `SRV-STEVEN`, le rôle DNS est couplé au rôle AD DS. Toutes les machines du domaine pointent vers ce serveur comme DNS primaire.

---

## 🗂️ Active Directory (AD)

**Active Directory** est une base de données centralisée qui relie les utilisateurs aux ressources du réseau dont ils ont besoin.

Il permet de gérer depuis un seul endroit :

| Objet | Exemple chez StevenTech |
|---|---|
| Comptes utilisateurs | `l.leblanc`, `m.moulin` |
| Ordinateurs | `PC-TTALON`, `PC-KKOWALSKI` |
| Groupes | `GG_Comptabilite`, `GG_RH` |
| Droits & autorisations | Accès aux dossiers partagés |
| Imprimantes & ressources | Imprimante réseau partagée |

---

## 🔌 LDAP — Lightweight Directory Access Protocol

**LDAP** est le protocole utilisé pour communiquer avec Active Directory. Il permet d'interroger et de modifier l'annuaire.

- Port **389** (standard) / Port **636** (LDAPS, chiffré)
- Protocole **TCP**
- Utilisé par les clients pour se connecter au contrôleur de domaine `SRV-STEVEN`

**Opérations possibles :** recherche, ajout, modification, suppression d'entrées dans l'annuaire.

> Analogie : si AD est la base de données, LDAP est le langage qu'on utilise pour lui parler.

---

## 🔑 Kerberos — Protocole d'authentification

**Kerberos** est le protocole d'authentification par défaut dans un domaine Active Directory. Il fonctionne par système de **tickets**, sans que le mot de passe ne transite en clair sur le réseau.

**Étapes d'authentification :**

```
1. l.leblanc ouvre sa session sur PC-LLEBLAC
        ↓
2. Le client envoie son identifiant au KDC (hébergé sur SRV-STEVEN)
        ↓
3. Le KDC vérifie et délivre un TGT (Ticket-Granting Ticket)
        ↓
4. Le client présente son TGT au TGS pour accéder à une ressource
        ↓
5. Le TGS délivre un ticket de service → accès autorisé
```

**Caractéristiques :**
- Chiffrement symétrique
- Aucun mot de passe ne transite en clair
- Chaque contrôleur de domaine héberge un composant **KDC**

---

## 📋 Schéma, Classes et Objets AD

### Schéma AD
Structure de base de l'annuaire qui définit toutes les **classes** et tous les **attributs** autorisés. C'est le règlement général de l'AD.

### Classe AD
Catégorie d'objets partageant les mêmes attributs. Exemples : `User`, `Computer`, `Group`.

### Objet AD
Instance concrète d'une classe. Exemple : `l.leblanc` est un objet de classe `User`.

### Attributs
Propriétés d'un objet. Exemple pour `l.leblanc` : nom, prénom, login, email, groupe d'appartenance…

**Hiérarchie :**
```
Schéma → Classe → Objet → Attributs
  📋         📦       🔵       🏷️
(règles)  (catégorie) (instance) (propriétés)
```

---

## 🌲 Domaine, Forêt et Sites AD

### Domaine
Brique de base de l'AD. Regroupe des objets partageant une même base de données et des stratégies de sécurité communes.

Chez StevenTech : `steventech.local` est le **domaine racine**.

### Forêt
Ensemble d'arbres (domaine racine + ses sous-domaines) partageant un **schéma commun** et un **catalogue global**.

> Chez StevenTech, la forêt ne contient qu'un seul domaine : `steventech.local`. Une forêt multi-domaines serait utile si StevenTech ouvrait des filiales.

### Sites AD
Représentation géographique du réseau. Permet d'organiser la **réplication** entre contrôleurs de domaine.

- **Réplication intrasite** : entre DC du même site (automatique et fréquente)
- **Réplication intersite** : entre DC de sites différents (planifiée)

> Chez StevenTech, un seul site correspond au siège unique de l'entreprise.

---

## 🗃️ Catalogue global

Le **catalogue global** est un contrôleur de domaine spécial qui contient une copie partielle de **tous les objets de la forêt**.

**4 fonctions clés :**
1. Recherche d'objets dans toute la forêt
2. Résolution des noms UPN (`l.leblanc@steventech.local`)
3. Informations sur l'appartenance aux groupes universels
4. Vérification des références d'objets inter-domaines

> Chez StevenTech, `SRV-STEVEN` est à la fois contrôleur de domaine **et** serveur de catalogue global — configuration standard pour une PME avec un seul domaine.

---

## 📦 Groupe de travail vs Domaine AD

| | Groupe de travail | Domaine AD |
|---|---|---|
| Authentification | Locale (par machine) | Centralisée (via SRV-STEVEN) |
| Gestion des comptes | Sur chaque poste | Depuis l'AD |
| Adapté pour | < 5 postes | Toute taille d'entreprise |
| Chez StevenTech | ❌ Non retenu | ✅ Choix retenu |

---

*Suite → [`02-structure-AD.md`](02-structure-AD.md)*
