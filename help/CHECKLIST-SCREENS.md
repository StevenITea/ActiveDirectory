# 📸 Checklist — Screenshots à capturer sur les VMs

> Suivez cette liste dans l'ordre. Chaque screen sera intégré dans la documentation du repo.  
> Nommez vos fichiers exactement comme indiqué pour correspondre aux liens dans les docs Markdown.  
> Dossier de destination : `screens/`

---

## 🗂️ 01 — Structure AD (UO, Groupes, Utilisateurs)

> Console : **Utilisateurs et Ordinateurs Active Directory** (`dsa.msc`)

- [ ] `01-vue-generale-ad.png` — Vue générale de l'arborescence AD avec toutes les UO visibles
- [ ] `01-uo-admin.png` — Contenu de l'UO `_Admin` avec `s.steven`
- [ ] `01-uo-comptabilite.png` — Contenu de l'UO `Comptabilite` avec `l.leblanc` et `t.talon`
- [ ] `01-uo-rh.png` — Contenu de l'UO `RH` avec `j.jobert`
- [ ] `01-uo-commercial.png` — Contenu de l'UO `Commercial` avec `m.moulin` et `s.sylvestre`
- [ ] `01-uo-informatique.png` — Contenu de l'UO `Informatique` avec `k.kowalski`
- [ ] `01-uo-direction.png` — Contenu de l'UO `Direction` avec `a.aubert`
- [ ] `01-uo-ordinateurs.png` — Contenu de l'UO `Ordinateurs` avec tous les PC
- [ ] `01-groupes-liste.png` — Liste de tous les groupes `GG_` dans leur UO
- [ ] `01-groupe-membres.png` — Onglet **Membres** d'un groupe (ex : `GG_Comptabilite`) montrant ses membres
- [ ] `01-user-proprietes.png` — Propriétés d'un utilisateur (ex : `l.leblanc`) onglet **Général**
- [ ] `01-user-groupes.png` — Onglet **Membre de** du même utilisateur montrant ses groupes

---

## 🌐 02 — DNS

> Console : **Gestionnaire DNS** (`dnsmgmt.msc`)

- [ ] `02-dns-zones.png` — Vue des zones DNS avec `steventech.local` visible
- [ ] `02-dns-enregistrements.png` — Enregistrements A de la zone `steventech.local` (SRV-STEVEN visible)

---

## 📁 03 — Arborescence du serveur de fichiers

> Explorateur Windows sur `SRV-STEVEN`

- [ ] `03-arborescence-partage.png` — Vue complète de `C:\Partage\` avec tous les sous-dossiers dépliés

---

## 🔐 04 — Droits de partage

> Clic droit sur `C:\Partage` → Propriétés → Partage avancé → Autorisations

- [ ] `04-partage-autorisation.png` — Fenêtre des autorisations de partage avec `GG_Tous` en Modification

---

## 🔒 05 — Droits NTFS

> Clic droit sur chaque dossier → Propriétés → Sécurité

- [ ] `05-ntfs-partage-racine.png` — Droits NTFS sur `C:\Partage` (`GG_Tous` en Lecture/Exécution)
- [ ] `05-ntfs-comptabilite.png` — Droits NTFS sur `C:\Partage\Comptabilite` (`GG_Comptabilite` en Modification)
- [ ] `05-ntfs-rh.png` — Droits NTFS sur `C:\Partage\RH` (`GG_RH` + `GG_Direction`)
- [ ] `05-ntfs-commercial.png` — Droits NTFS sur `C:\Partage\Commercial`
- [ ] `05-ntfs-informatique.png` — Droits NTFS sur `C:\Partage\Informatique`
- [ ] `05-ntfs-direction.png` — Droits NTFS sur `C:\Partage\Direction`
- [ ] `05-ntfs-commun.png` — Droits NTFS sur `C:\Partage\Commun` (`GG_Tous` en Écriture)

---

## 👤 06 — Profil itinérant

> Console AD → Propriétés utilisateur → Onglet **Profil**

- [ ] `06-profil-itinerant-config.png` — Onglet Profil d'un utilisateur avec le chemin `\\SRV-STEVEN\Profils\%username%` renseigné
- [ ] `06-profil-dossier-serveur.png` — Dossier `E:\Profils\` sur le serveur avec le sous-dossier de l'utilisateur créé après connexion
- [ ] `06-profil-partage.png` — Autorisations de partage du dossier `Profils`

---

## 🏠 07 — Dossier de base

> Console AD → Propriétés utilisateur → Onglet **Profil** → section Dossier de base

- [ ] `07-dossier-base-config.png` — Onglet Profil avec lecteur `H:` et chemin `\\SRV-STEVEN\HMDIR\%username%` renseigné
- [ ] `07-dossier-base-client.png` — Explorateur Windows côté **client** avec le lecteur `H:` visible dans "Ce PC"
- [ ] `07-dossier-base-serveur.png` — Dossier `E:\HMDIR\` sur le serveur avec le sous-dossier utilisateur créé

---

## 📜 08 — Script de mappage (Netlogon)

> Explorateur Windows sur le serveur

- [ ] `08-script-contenu.png` — Contenu du fichier `mappage-lecteur.bat` ouvert dans le Bloc-notes
- [ ] `08-script-emplacement.png` — Emplacement du script dans `C:\Windows\SYSVOL\sysvol\steventech.local\scripts\`
- [ ] `08-script-config-user.png` — Onglet Profil d'un utilisateur avec le nom du script renseigné dans **Script d'ouverture de session**
- [ ] `08-script-lecteur-client.png` — Explorateur Windows côté **client** avec le lecteur `P:` monté automatiquement

---

## ⏰ 09 — Plage horaire

> Console AD → Propriétés utilisateur → Onglet **Compte** → Heures d'ouverture de session

- [ ] `09-plage-horaire-config.png` — Fenêtre des heures d'ouverture de session configurée (lundi-vendredi, 7h-20h)
- [ ] `09-plage-horaire-refus.png` — Message d'erreur côté client lors d'une tentative de connexion hors plage

---

## ⏳ 10 — Expiration de compte

> Console AD → Propriétés utilisateur → Onglet **Compte** → Date d'expiration du compte

- [ ] `10-expiration-config.png` — Onglet Compte avec une date d'expiration renseignée
- [ ] `10-expiration-refus.png` — Message d'erreur côté client avec un compte expiré

---

## ✅ Récapitulatif

| Section | Nombre de screens |
|---|---|
| Structure AD | 12 |
| DNS | 2 |
| Arborescence fichiers | 1 |
| Droits de partage | 1 |
| Droits NTFS | 7 |
| Profil itinérant | 3 |
| Dossier de base | 3 |
| Script Netlogon | 4 |
| Plage horaire | 2 |
| Expiration de compte | 2 |
| **Total** | **37 screens** |

---

> 💡 **Conseil** : faites vos screens en **1920x1080**, fenêtres bien ouvertes et lisibles.  
> Évitez les informations personnelles visibles en arrière-plan.  
> Une fois tous les screens capturés, placez-les dans le dossier `screens/` du repo.
