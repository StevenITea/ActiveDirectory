# 🗂️ Active Directory — Infrastructure & Administration

> Documentation complète d'une infrastructure Active Directory mise en place sur Windows Server.  
> Ce projet couvre la conception, la configuration et la sécurisation d'un environnement AD de A à Z.

---

## 📌 Présentation du projet

Ce repo documente la mise en place d'une infrastructure **Active Directory** pour un centre de formation, incluant :

- La structure logique de l'AD (UO, groupes, utilisateurs)
- La gestion des droits NTFS et de partage sur un serveur de fichiers
- La configuration de profils itinérants et dossiers de base
- L'automatisation par scripts de mappage de lecteurs réseau

**Environnement technique :**
- Serveur : Windows Server 2019 — Contrôleur de domaine (`SRVAD`)
- Client : Windows 10 intégré au domaine
- Rôles installés : AD DS, DNS, Serveur de fichiers

---

## 📁 Structure du repo

```
AD-Infrastructure/
├── README.md                  ← Ce fichier
├── docs/
│   ├── 01-concepts-AD.md      ← Définitions et protocoles clés
│   ├── 02-structure-AD.md     ← UO, groupes, utilisateurs
│   ├── 03-droits-NTFS.md      ← Logique des droits NTFS et partage
│   └── 04-profils-scripts.md  ← Profils itinérants, dossier de base, scripts
├── scripts/
│   ├── mappage-lecteur.bat    ← Script de mappage lecteur réseau
│   └── README.md              ← Explication des scripts
└── tableaux/
    └── droits-NTFS.xlsx       ← Tableau de réflexion des droits
```

---

## 🏗️ Architecture mise en place

### Structure du domaine

```
domaine.local (Contrôleur de domaine : SRVAD)
│
├── UO : Formateurs
│   ├── UO : Formation Réseau      → Formateur : Jack Morrison
│   ├── UO : Formation Développement → Formateur : Roger Sterling
│   └── UO : Formation Graphisme   → Formateur : Malcolm Murray
│
└── UO : Apprenants
    ├── UO : Formation Réseau      → 5 stagiaires
    ├── UO : Formation Développement → 4 stagiaires
    └── UO : Formation Graphisme   → 4 stagiaires
```

### Arborescence du serveur de fichiers

```
C:\Partage\
├── Administration\
│   ├── Formation Développement\
│   ├── Formation Graphisme\
│   └── Formation Réseau\
├── Commun Stagiaire\
├── Développement\
│   ├── Formateur référents Dev\
│   └── Stagiaire Dev\
├── Graphisme\
│   ├── Formateur référents Graphisme\
│   └── Stagiaire Graphisme\
└── Réseau\
    ├── Formateur référents Réseau\
    └── Stagiaires Réseau\
```

---

## 🔐 Logique des droits appliquée

| Dossier | Groupe | Droit NTFS |
|---|---|---|
| `C:\Partage` | G_Toutes_Formations | Lecture/Exécution |
| `C:\Partage\Administration` | G_Formateurs_Ref | Lecture/Exécution |
| `C:\Partage\Administration\Formation Réseau` | G_Formateurs_Ref_Reseau | Modification |
| `C:\Partage\Réseau` | G_Reseau | Lecture/Exécution |
| `C:\Partage\Réseau\Stagiaires Réseau` | G_Stagiaires_Res | Écriture |
| `C:\Partage\Commun Stagiaire` | G_Toutes_Formations | Écriture |

> ℹ️ Le droit de partage est configuré **une seule fois** sur le dossier racine `Partage` (groupe `G_Toutes_Formations` en Modification). Les droits NTFS viennent ensuite **restreindre** finement les accès.

---

## 👤 Fonctionnalités de gestion utilisateurs

| Fonctionnalité | Description |
|---|---|
| **Profil itinérant** | Stocké sur `\\SRVAD\Profils\%username%` — l'environnement suit l'utilisateur |
| **Dossier de base** | Monté en `H:` depuis `\\SRVAD\HMDIR\%username%` — espace personnel centralisé |
| **Script de mappage** | Lecteur `P:` mappé automatiquement à l'ouverture de session via `script.bat` |
| **Plage horaire** | Connexion autorisée du lundi au vendredi, de 6h à 21h (formateurs) |
| **Expiration de compte** | Date d'expiration configurable par utilisateur (stagiaires temporaires) |

---

## 📚 Documentation

| Fichier | Contenu |
|---|---|
| [`docs/01-concepts-AD.md`](docs/01-concepts-AD.md) | DNS, LDAP, Kerberos, Schéma, Forêt, Domaine… |
| [`docs/02-structure-AD.md`](docs/02-structure-AD.md) | UO, groupes de sécurité, étendues, catalogue global |
| [`docs/03-droits-NTFS.md`](docs/03-droits-NTFS.md) | Droits NTFS vs droits de partage, héritage, tableau complet |
| [`docs/04-profils-scripts.md`](docs/04-profils-scripts.md) | Profil itinérant, dossier de base, script netlogon |

---

## 🛠️ Scripts

| Script | Rôle |
|---|---|
| [`scripts/mappage-lecteur.bat`](scripts/mappage-lecteur.bat) | Mappe le lecteur `P:` vers `\\SRVAD\partage` à l'ouverture de session |

---

## 📖 Concepts clés abordés

`Active Directory` `DNS` `LDAP` `Kerberos` `NTFS` `GPO` `UO` `Groupes de sécurité` `Profil itinérant` `Dossier de base` `Script Netlogon` `Serveur de fichiers` `Windows Server 2019`
