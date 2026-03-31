# 🗂️ Active Directory — Infrastructure & Administration

> Documentation complète d'une infrastructure Active Directory mise en place sur Windows Server.  
> Projet réalisé par **StevenITea** — [github.com/StevenITea](https://github.com/StevenITea)

---

## 📌 Présentation du projet

Ce repo documente la mise en place d'une infrastructure **Active Directory** pour une PME fictive **StevenTech**, société de services informatiques de 8 personnes, incluant :

- La structure logique de l'AD (UO, groupes, utilisateurs)
- La gestion des droits NTFS et de partage sur un serveur de fichiers
- La configuration de profils itinérants et dossiers de base
- L'automatisation par scripts de mappage de lecteurs réseau

**Environnement technique :**
- Serveur : Windows Server 2019 — Contrôleur de domaine (`SRV-STEVEN`)
- Nom de domaine : `steventech.local`
- Client : Windows 10 intégré au domaine
- Administrateur domaine : `s.steven`
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

### Utilisateurs de la PME

| Nom | Poste | Login |
|---|---|---|
| Steven Saunier | Responsable IT (admin) | s.steven |
| Laura Leblanc | Comptable | l.leblanc |
| Thomas Talon | Comptable | t.talon |
| Julie Jobert | Chargée RH | j.jobert |
| Marc Moulin | Commercial | m.moulin |
| Sophie Sylvestre | Commerciale | s.sylvestre |
| Kevin Kowalski | Technicien IT | k.kowalski |
| Amélie Aubert | Assistante de direction | a.aubert |

### Structure des UO

```
steventech.local  (Contrôleur de domaine : SRV-STEVEN)
│
├── UO : _Admin
│   └── s.steven
│
├── UO : Comptabilite
│   ├── l.leblanc
│   └── t.talon
│
├── UO : RH
│   └── j.jobert
│
├── UO : Commercial
│   ├── m.moulin
│   └── s.sylvestre
│
├── UO : Informatique
│   └── k.kowalski
│
├── UO : Direction
│   └── a.aubert
│
└── UO : Ordinateurs
    ├── PC-LLEBLAC
    ├── PC-TTALON
    ├── PC-JJOBERT
    ├── PC-MMOULIN
    ├── PC-SSYLVESTRE
    ├── PC-KKOWALSKI
    └── PC-AAUBERT
```

### Groupes de sécurité

| Groupe | Membres |
|---|---|
| `GG_Comptabilite` | l.leblanc, t.talon |
| `GG_RH` | j.jobert |
| `GG_Commercial` | m.moulin, s.sylvestre |
| `GG_Informatique` | k.kowalski, s.steven |
| `GG_Direction` | a.aubert, s.steven |
| `GG_Tous` | Tous les utilisateurs du domaine |

### Arborescence du serveur de fichiers

```
C:\Partage\
├── Comptabilite\
│   ├── Factures\
│   └── Bilans\
├── RH\
│   ├── Contrats\
│   └── Fiches-de-paie\
├── Commercial\
│   ├── Devis\
│   └── Clients\
├── Informatique\
│   ├── Scripts\
│   └── Documentation\
├── Direction\
│   └── Rapports\
└── Commun\
```

---

## 🔐 Logique des droits appliquée

| Dossier | Groupe | Droit NTFS |
|---|---|---|
| `C:\Partage` | GG_Tous | Lecture/Exécution |
| `C:\Partage\Commun` | GG_Tous | Écriture |
| `C:\Partage\Comptabilite` | GG_Comptabilite | Modification |
| `C:\Partage\RH` | GG_RH | Modification |
| `C:\Partage\RH` | GG_Direction | Lecture/Exécution |
| `C:\Partage\Commercial` | GG_Commercial | Modification |
| `C:\Partage\Informatique` | GG_Informatique | Modification |
| `C:\Partage\Direction` | GG_Direction | Modification |

> ℹ️ Le droit de partage est configuré **une seule fois** sur le dossier racine `Partage` (groupe `GG_Tous` en Modification). Les droits NTFS viennent ensuite **restreindre** finement les accès par service.

---

## 👤 Fonctionnalités de gestion utilisateurs

| Fonctionnalité | Description |
|---|---|
| **Profil itinérant** | Stocké sur `\\SRV-STEVEN\Profils\%username%` — l'environnement suit l'utilisateur |
| **Dossier de base** | Monté en `H:` depuis `\\SRV-STEVEN\HMDIR\%username%` — espace personnel centralisé |
| **Script de mappage** | Lecteur `P:` mappé automatiquement à l'ouverture de session via `mappage-lecteur.bat` |
| **Plage horaire** | Connexion autorisée du lundi au vendredi, de 7h à 20h |
| **Expiration de compte** | Date d'expiration configurable (prestataires, comptes temporaires) |

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
| [`scripts/mappage-lecteur.bat`](scripts/mappage-lecteur.bat) | Mappe le lecteur `P:` vers `\\SRV-STEVEN\Partage` à l'ouverture de session |

---

## 📖 Concepts clés abordés

`Active Directory` `DNS` `LDAP` `Kerberos` `NTFS` `UO` `Groupes de sécurité` `Profil itinérant` `Dossier de base` `Script Netlogon` `Serveur de fichiers` `Windows Server 2019`
