# FICHE DE SITUATION PROFESSIONNELLE : BLOC 1 (ÉPREUVE E5)

## 1. IDENTIFICATION GÉNÉRALE

- **Titre de la réalisation** : Migration d'un contrôleur WiFi UniFi, d'un Raspberry Pi défaillant vers un mini PC NIPOGI, avec conteneurisation sous Docker
- **Cadre de réalisation** :
  - [x] Stage 1 (stage de 5 semaines du 25 mai au 26 juin 2026, agence de Toulouse de SNS Solutions, BTS SIO option SLAM)
  - [ ] Stage 2
  - [ ] Atelier de professionnalisation
  - [ ] Projet de cours
- **Période de réalisation** : du 28 mai (après-midi) au 2 juin 2026 (matin)
- **Modalité** :
  - [ ] Individuel
  - [x] En équipe (réalisé avec un technicien de l'agence de Toulouse pour certaines étapes, sous mon intervention directe sur le diagnostic, l'extraction de la configuration et le déploiement Docker)
- **Localisation / Organisation cliente** : SNS Solutions, agence de Toulouse (ESN spécialisée dans l'infogérance, les réseaux, les télécoms et la cybersécurité). Intervention pour le compte d'un client externe de l'agence.
- **Document de référence** : rapport de stage (sections 4.1, 4.2 et 5.1, annexe 8.1 : fichier `compose.yaml`)

---

## 2. CONTEXTE ET OBJECTIFS

- **Contexte organisationnel** : SNS SOLUTIONS, fondée en 2004, est une ESN positionnée comme une DSI externalisée pour des TPE et PME. Elle s'appuie sur cinq agences (Bordeaux, Marseille, Nice, Monaco et Toulouse) et organise son travail par client. L'agence de Toulouse est une équipe récemment constituée, en montée en charge sur les infrastructures locales. L'intervention concerne la maintenance de l'infrastructure d'un client externe.
- **Problématique / Besoin exprimé** :
  - Le contrôleur UniFi (Ubiquiti) du client était hébergé sur un Raspberry Pi physiquement instable, à cause d'un défaut récurrent d'alimentation électrique.
  - Ce contrôleur est critique : il centralise la configuration, le routage et la supervision de l'ensemble des bornes WiFi de l'entreprise cliente.
  - Il fallait donc remplacer ce matériel sans perdre la configuration existante.
- **Objectifs fixés** :
  - **Audit** : auditer l'ancien équipement et en extraire les données de configuration.
  - **Migration** : migrer le service vers un mini PC NIPOGI de remplacement, en isolant l'application dans un environnement conteneurisé (portabilité et maintenance simplifiée).
  - **Restauration** : réimporter la configuration (zones et topologie réseau) et remettre le contrôleur en production avec un adressage fixe.
  - **Continuité** : conduire la migration sans coupure de production pour les utilisateurs.

---

## 3. DÉMARCHE ET ENVIRONNEMENT TECHNIQUE

### Environnement technologique mobilisé

- **Systèmes d'exploitation** : système du Raspberry Pi d'origine (accès SSH et Webmin), Windows sur le mini PC NIPOGI (réinitialisé), Windows sur mon poste de travail
- **Logiciels, rôles ou paquets** :
  - Contrôleur : UniFi Network Server (image Docker `unifi-os-server`), fichier de sauvegarde `.unf`
  - Conteneurisation : Docker Desktop, WSL (Windows Subsystem for Linux), Docker Compose (`compose.yaml`)
  - Administration à distance : SSH, Webmin (port d'écoute non standard)
  - Diagnostic réseau : `nmap`, `ping`
  - Téléphonie : composant SBC 3CX (Session Border Controller), installé sur le mini PC pour sécuriser les flux VoIP du client
  - Documentation : outil de helpdesk interne SNS Solutions GPI (basé sur GLPI), qui a servi à retrouver les identifiants
- **Matériels ou éléments d'infrastructure concernés** :
  - Raspberry Pi du client (alimentation défaillante)
  - Mini PC NIPOGI de remplacement
  - Switch de laboratoire pour isoler l'ancien matériel, relié à mon poste de travail
  - Bornes WiFi du client gérées par le contrôleur
  - Adressage cible : IPv4 fixe, masque de sous-réseau, passerelle par défaut et DNS public 8.8.8.8, selon les schémas réseaux de l'entreprise cliente

### Démarche suivie étape par étape

1. **Analyse préalable et préparation**
   - Prise en charge du contrôleur UniFi hébergé sur le Raspberry Pi (jeudi 28 mai).
   - Isolement de l'ancien matériel sur un switch de laboratoire relié à mon poste, pour intervenir sans coupure de production.
   - Réseau sans serveur DHCP : configuration d'une adresse IPv4 statique sur mon poste pour me placer dans le même sous-réseau.
   - Validation de la connectivité IP par un scan `nmap` et un `ping`. L'interface Web restant inaccessible, recherche d'un autre accès (SSH).
2. **Configuration et mise en œuvre technique**
   - Récupération des identifiants d'administration dans la documentation GPI, puis connexion en SSH et accès à l'interface Webmin via un port non standard (vendredi 29 mai).
   - Sauvegarde de l'intégralité de la configuration du contrôleur dans un fichier compressé `.unf`.
   - Diagnostic d'un défaut d'alimentation rendant le Raspberry Pi non fiable, d'où la décision de le remplacer par un mini PC NIPOGI.
   - Réinitialisation complète de Windows sur le mini PC (compte existant suspecté d'être rattaché à un ancien domaine Active Directory), puis création d'un profil administrateur local (lundi 1er juin).
   - Installation du composant SBC 3CX pour sécuriser les flux VoIP du client.
   - Déploiement d'UniFi Network Server dans un conteneur : configuration de Docker Desktop adossé à WSL, rédaction et déploiement du fichier `compose.yaml`, lancement par `docker compose up -d` (avec `docker compose down` et `docker compose pull` pour rafraîchir l'environnement).
   - Réimport de la sauvegarde `.unf` après avoir retrouvé le compte UI d'origine dans la base GPI (mardi 2 juin), puis restauration des configurations de zones et de la topologie réseau.
   - Raccordement du mini PC à l'infrastructure de production et configuration de son adressage statique (IPv4 fixe, masque, passerelle, DNS 8.8.8.8).
3. **Tests d'acceptation et vérification du bon fonctionnement**
   - Validation de la connectivité IP de l'ancien matériel (`nmap`, `ping`) avant extraction.
   - Validation de l'accès à la nouvelle interface Web du contrôleur sur le port personnalisé 11443.
   - Restauration complète des configurations de zones et de la topologie réseau à partir du fichier `.unf`.
   - Résultat : un serveur de supervision stable, virtualisé et facilement sauvegardable pour le client.

### Gestion des imprévus / Incidents rencontrés

| Difficulté | Solution ou enseignement |
|---|---|
| Réseau isolé sans DHCP et interface Web du contrôleur inaccessible | Adresse IPv4 statique sur mon poste, scan `nmap` et `ping` pour confirmer la connectivité, puis connexion en SSH et rebond sur Webmin |
| Identifiants SSH inconnus au départ | Recherche dans la documentation du helpdesk GPI, qui sert de base de connaissances principale |
| Défaut d'alimentation du Raspberry Pi qui rend le matériel non fiable | Décision de remplacer le Raspberry Pi par un mini PC NIPOGI et de migrer le service vers un conteneur |
| Échec d'authentification sur le compte existant du mini PC, suspecté d'être rattaché à un ancien domaine Active Directory | Réinitialisation complète du système d'exploitation, puis création d'un profil administrateur local |
| Import de la sauvegarde bloqué par une restriction Ubiquiti, qui exige les identifiants du compte UI d'origine | Recherche approfondie dans l'historique documentaire de GPI, où le compte propriétaire a été retrouvé, ce qui a permis la restauration complète |

---

## 4. RÔLE ET CONTRIBUTION PERSONNELLE

- **Responsabilité précise** : stagiaire informatique au sein de l'équipe technique de l'agence de Toulouse, chargé de l'audit de l'ancien contrôleur et de la mise en place du nouveau, avec l'appui des techniciens de l'agence.
- **Actions menées individuellement** :
  - **Diagnostic** : configuration d'une IP statique sur mon poste, scan `nmap` et `ping`, puis recherche d'un accès alternatif quand l'interface Web restait inaccessible.
  - **Extraction** : connexion SSH, accès à Webmin et sauvegarde de la configuration au format `.unf`.
  - **Installation** : installation du composant SBC 3CX sur le mini PC.
  - **Conteneurisation** : configuration de Docker Desktop et de WSL, rédaction et déploiement du fichier `compose.yaml`, lancement et validation de l'accès sur le port 11443.
  - **Recherche documentaire** : recherche dans GPI des identifiants d'administration, puis du compte UI d'origine pour débloquer l'import.
- **Ce que je n'ai pas réalisé seul** : la décision de remplacer le Raspberry Pi, la réinitialisation du mini PC, la restauration finale et la mise en production chez le client ont été menées en équipe avec les techniciens de l'agence (le rapport emploie « nous » pour ces étapes). L'adressage statique a été fixé à partir des schémas réseaux de l'entreprise cliente.

---

## 5. LIVRABLES ET PREUVES ASSOCIÉES

- [x] **Procédure technique ou document d'exploitation** : description détaillée de la migration en quatre temps (contexte, audit et extraction, préparation de la plateforme, restauration et mise en production), dans le rapport de stage, section 5.1
- [ ] **Schémas d'architecture** : aucun schéma dans le rapport de stage
- [x] **Extraits de configurations ou scripts** : fichier `compose.yaml` complet (annexe 8.1) et commandes `docker compose up -d`, `docker compose down`, `docker compose pull`
- [ ] **Captures d'écran significatives** : non disponibles dans le rapport de stage
- [ ] **Code source versionné** : sans objet pour cette réalisation

---

## 6. COMPÉTENCES DU BLOC 1 MOBILISÉES

- [x] **B1.1 Gérer le patrimoine informatique** : remplacement d'un équipement défaillant pour garantir la continuité du service, sauvegarde de la configuration (fichier `.unf`) et restauration sur le nouveau support, adressage réseau statique cohérent avec les schémas de l'entreprise.
- [x] **B1.2 Répondre aux incidents et demandes d'assistance** : diagnostic d'un matériel instable (réseau isolé, interface inaccessible, défaut d'alimentation), recherche des informations dans la base de connaissances GPI, résolution par le remplacement du support et la migration du service.
- [ ] **B1.3 Développer la présence en ligne** : compétence non mobilisée dans cette mission.
- [ ] **B1.4 Travailler en mode projet** : compétence peu mobilisée dans cette mission (voir la fiche sur le serveur MDT).
- [x] **B1.5 Mettre à disposition des utilisateurs un service informatique** : déploiement du contrôleur dans un conteneur Docker (portabilité, maintenance simplifiée), mise en production chez le client avec un adressage fixe, validation de l'accès à l'interface Web.
- [x] **B1.6 Organiser son développement professionnel** : appropriation de technologies et d'outils d'infogérance que je n'avais pas l'habitude de côtoyer (Docker Compose, WSL, Webmin, helpdesk GPI), à partir de la documentation interne et de recherches en ligne.

---

## 7. BILAN RÉFLEXIF ET AUTO-ÉVALUATION

- **Points forts** :
  - Une méthode de diagnostic progressive en réseau isolé (IP statique, `nmap`, `ping`, SSH, Webmin) qui a permis d'extraire la configuration d'un matériel défaillant sans accès à l'interface Web.
  - Une migration conduite pour éviter toute coupure de production, avec une restauration complète des zones et de la topologie réseau.
  - Un service désormais conteneurisé, donc portable, plus simple à maintenir et facilement sauvegardable.
  - Une bonne exploitation de la documentation interne GPI pour retrouver les identifiants nécessaires.
- **Axes d'amélioration** :
  - **Documentation** : consigner dans GPI, dès la mise en service d'un contrôleur, le compte UI propriétaire, pour éviter la recherche qui a retardé l'import de la sauvegarde.
  - **Sauvegardes** : planifier des sauvegardes régulières du fichier `.unf` pour ne plus dépendre d'une extraction faite dans l'urgence.
  - **Capitalisation** : conserver le fichier `compose.yaml` et la procédure de migration dans la base de connaissances pour les prochaines interventions.
- **Compétences professionnelles consolidées** :
  - Techniques : diagnostic réseau (`nmap`, `ping`), accès distant (SSH, Webmin), conteneurisation avec Docker Desktop, WSL et Docker Compose, sauvegarde et restauration d'un contrôleur WiFi UniFi, paramétrage IPv4 (adresse statique, masque, passerelle, DNS).
  - Méthodologiques : intervention structurée sur l'infrastructure d'un client, recherche d'informations dans une base de connaissances, communication avec l'équipe technique de l'agence.