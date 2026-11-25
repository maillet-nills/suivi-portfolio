# FICHE DE SITUATION PROFESSIONNELLE : BLOC 1 (ÉPREUVE E5)

## 1. IDENTIFICATION GÉNÉRALE

- **Titre de la réalisation** : Mise en place d'un serveur de déploiement MDT (Microsoft Deployment Toolkit) virtualisé sous Proxmox VE
- **Cadre de réalisation** :
  - [x] Stage 1 (stage de 5 semaines du 25 mai au 26 juin 2026, agence de Toulouse de SNS Solutions, BTS SIO option SLAM)
  - [ ] Stage 2
  - [ ] Atelier de professionnalisation
  - [ ] Projet de cours
- **Période de réalisation** : du 17 juin (après-midi) au 25 juin 2026
- **Modalité** :
  - [ ] Individuel
  - [x] En équipe (réalisé avec les techniciens de l'agence de Toulouse, avec une part importante d'installation et de configuration menée par moi)
- **Localisation / Organisation cliente** : SNS Solutions, agence de Toulouse (ESN spécialisée dans l'infogérance, les réseaux, les télécoms et la cybersécurité). Projet d'infrastructure interne à l'agence, destiné à être harmonisé avec l'agence de Marseille.
- **Document de référence** : rapport de stage (sections 4.4, 4.5 et 5.2)

---

## 2. CONTEXTE ET OBJECTIFS

- **Contexte organisationnel** : SNS SOLUTIONS, fondée en 2004, est une ESN positionnée comme une DSI externalisée pour des TPE et PME. Elle s'appuie sur un réseau de cinq agences (Bordeaux, Marseille, Nice, Monaco et Toulouse). L'agence de Toulouse, récemment constituée, monte en charge sur les infrastructures locales. Elle prépare régulièrement de nouveaux postes pour ses clients, comme les ordinateurs portables Terra configurés pendant mon stage.
- **Problématique / Besoin exprimé** :
  - La préparation de nouveaux postes clients et le reconditionnement de flottes d'envergure étaient chronophages lorsqu'ils étaient réalisés manuellement ou de manière isolée.
  - Il fallait standardiser les systèmes d'exploitation, garantir la conformité des configurations logicielles et rationaliser le temps technique.
  - Une infrastructure d'industrialisation est donc apparue indispensable.
- **Objectifs fixés** :
  - **Déploiement automatisé** : disposer d'un environnement centralisé pour le déploiement d'images système (masters) standardisées avec MDT.
  - **Infrastructure** : héberger la solution sur l'hyperviseur de type 1 Proxmox VE, installé sur un serveur Dell PowerEdge recyclé.
  - **Validation** : tester la chaîne complète jusqu'à un amorçage réseau PXE sur un poste physique.
  - **Harmonisation** : importer à terme le Deployment Share de l'agence de Marseille pour unifier les pratiques des deux sites.

---

## 3. DÉMARCHE ET ENVIRONNEMENT TECHNIQUE

### Environnement technologique mobilisé

- **Systèmes d'exploitation** : Proxmox VE (hyperviseur), Windows Server 2022 (machine virtuelle dédiée à MDT), image de boot WinPE (LiteTouch), Windows Server d'origine des serveurs récupérés (audités avant réaffectation)
- **Logiciels, rôles ou paquets** :
  - Virtualisation : Proxmox VE, dépôt libre `pve-no-subscription` (fichier de sources `/etc/apt/sources.list.d/pve-enterprise.list` modifié), mises à jour par `apt update && apt upgrade -y`
  - Déploiement : Windows ADK (Assessment and Deployment Kit), module WinPE, MDT (Deployment Share, séquences de tâches), rôle WDS (Windows Deployment Services)
  - Création du support d'installation : Rufus (image ISO de Proxmox VE sur support USB bootable)
  - Images produites : images de boot LiteTouch au format `.wim` et `.iso`
- **Matériels ou éléments d'infrastructure concernés** :
  - Serveurs Dell PowerEdge : un R710 audité puis écarté, un R510 retenu, avec le contrôleur de stockage PERC et la carte de gestion iDRAC
  - Disque SSD externe sauvegardé avant de servir de support d'installation
  - Ordinateur portable Dell et commutateur réseau de test pour l'essai PXE
  - Plan d'adressage de l'agence (adresses IP statiques, masque, passerelle)

### Démarche suivie étape par étape

1. **Analyse préalable et préparation**
   - Expression du besoin : standardiser et accélérer la préparation des postes clients (contexte détaillé en section 2).
   - Récupération d'un premier serveur Dell PowerEdge R710 et état des lieux physique et logique (mercredi 17 juin) : ventilateur châssis absent (surventilation de sécurité), deux disques non détectés, Windows Server 2019 vierge. Mise à jour du firmware de l'iDRAC, mais accumulation de défaillances matérielles qui réserve l'usage en production de ce châssis.
   - Récupération d'un second serveur pour remplacer le premier (vendredi 19 juin), avec audit logique initial : aucune donnée ni rôle critique, donc réaffectation validée.
   - Configuration réseau du serveur (adresse IP statique, masque, passerelle). Tentative d'accès à l'iDRAC6 pour un monitoring hors-bande, abandonnée au profit d'une installation et d'une administration en local.
2. **Configuration et mise en œuvre technique**
   - Sauvegarde d'un disque SSD externe, puis création avec Rufus d'un support bootable contenant l'ISO de Proxmox VE (lundi 22 juin).
   - Accès au BIOS pour vérifier l'activation de la virtualisation matérielle (Intel VT-x) et régler l'ordre d'amorçage sur le support USB.
   - Installation en local et configuration initiale de Proxmox VE (mardi 23 juin) : nom de domaine pleinement qualifié (FQDN), adresse IP statique, masque de sous-réseau.
   - Modification des dépôts de paquets pour contourner l'absence de souscription payante (passage au dépôt `pve-no-subscription`), puis mises à jour du système.
   - Disques absents de l'interface : ils n'étaient pas rattachés à un disque virtuel RAID. Remplacement des unités défectueuses, puis création d'un nouveau disque virtuel (Virtual Disk) depuis l'utilitaire du contrôleur PERC au démarrage.
   - Création d'une machine virtuelle dédiée sous Proxmox avec Windows Server 2022, et configuration réseau statique cohérente avec le plan d'adressage de l'agence.
   - Installation et configuration de Windows ADK, du module WinPE, de la console MDT et du rôle WDS.
   - Initialisation d'un Deployment Share local de test, import d'une image système générique et création d'une séquence de tâches d'installation basique (mercredi 24 juin).
   - Génération des images de boot LiteTouch `.wim` et `.iso`, puis injection d'un système d'exploitation de référence associé à une séquence de tâches personnalisée.
3. **Tests d'acceptation et vérification du bon fonctionnement**
   - Premier test de déploiement sur une machine virtuelle témoin, via le montage direct du fichier ISO (mercredi 24 juin).
   - Test d'amorçage réseau PXE sur un ordinateur portable Dell raccordé au même segment de réseau (jeudi 25 juin) : la carte réseau intercepte la diffusion DHCP, contacte le rôle WDS du serveur, télécharge l'environnement de boot LiteTouch généré par MDT, et l'assistant de déploiement se charge correctement.
   - Mise en production de l'infrastructure locale validée à l'issue de ce test, dans l'attente de l'import du Deployment Share de l'agence de Marseille.

### Gestion des imprévus / Incidents rencontrés

| Difficulté | Solution ou enseignement |
|---|---|
| Premier serveur (R710) en mauvais état : ventilateur absent, deux disques non détectés, défaillances matérielles accumulées | Récupération d'un second serveur (R510) pour le remplacer. Leçon : auditer l'état matériel dès la récupération d'un serveur, avant de planifier un projet dessus |
| Carte de gestion iDRAC6 inaccessible malgré un `ping` positif et un assouplissement des restrictions TLS de Firefox | Abandon du monitoring hors-bande, installation et administration du serveur en local |
| Plusieurs disques physiques non visibles dans Proxmox | Constat qu'ils n'étaient pas rattachés à un volume RAID, remplacement des unités défectueuses et création d'un disque virtuel depuis l'utilitaire PERC |
| Dépôts Proxmox soumis à souscription payante | Désactivation du dépôt entreprise et passage au dépôt libre `pve-no-subscription` |
| Configuration de référence de l'agence de Marseille pas encore reçue | Initialisation d'un Deployment Share local de test avec une image générique, pour valider la génération des fichiers ISO et WIM en attendant l'import |

---

## 4. RÔLE ET CONTRIBUTION PERSONNELLE

- **Responsabilité précise** : stagiaire informatique au sein de l'équipe technique de l'agence de Toulouse, chargé de l'installation et de la configuration de l'hyperviseur et du serveur de déploiement, avec l'appui des techniciens de l'agence.
- **Actions menées individuellement** :
  - **État des lieux** : audit physique et logique des serveurs récupérés, mise à jour du firmware de l'iDRAC.
  - **Proxmox VE** : sauvegarde du SSD externe, création du support bootable avec Rufus, vérification de la virtualisation dans le BIOS, installation, configuration réseau et FQDN, modification des dépôts et mises à jour.
  - **Stockage** : configuration d'un nouveau disque virtuel depuis l'utilitaire PERC.
  - **Serveur de déploiement** : téléchargement de l'ISO de Windows Server 2022, création de la machine virtuelle, configuration réseau statique, installation et configuration d'ADK, WinPE, MDT et WDS.
  - **MDT** : initialisation du Deployment Share de test, génération des images de boot `.wim` et `.iso`, injection d'un système de référence et d'une séquence de tâches, tests sur machine virtuelle puis par PXE.
- **Ce que je n'ai pas réalisé seul** : la récupération des serveurs, le choix de l'hyperviseur Proxmox VE et le remplacement des disques ont été décidés et menés en équipe avec les techniciens de l'agence (le rapport emploie « nous »). L'import du Deployment Share de l'agence de Marseille dépend de cette agence et n'était pas encore réalisé à la fin du stage.

---

## 5. LIVRABLES ET PREUVES ASSOCIÉES

- [x] **Procédure technique ou document d'exploitation** : description détaillée de la mise en place en sept étapes (contexte, récupération du serveur, Proxmox VE, Windows Server 2022, ADK, WinPE, MDT et WDS, essai PXE, import de Marseille), dans le rapport de stage, section 5.2
- [ ] **Schémas d'architecture** : aucun schéma dans le rapport de stage
- [ ] **Extraits de configurations ou scripts** : limités dans le rapport à la commande `apt update && apt upgrade -y` et au fichier de sources `/etc/apt/sources.list.d/pve-enterprise.list`
- [ ] **Captures d'écran significatives** : non disponibles dans le rapport de stage
- [ ] **Code source versionné** : sans objet pour cette réalisation

---

## 6. COMPÉTENCES DU BLOC 1 MOBILISÉES

- [x] **B1.1 Gérer le patrimoine informatique** : état des lieux physique et logique des serveurs récupérés, recyclage d'un serveur après vérification qu'il ne contient aucune donnée critique, standardisation des images système (masters) pour garantir la conformité des configurations des postes.
- [ ] **B1.2 Répondre aux incidents et demandes d'assistance** : compétence non mobilisée directement dans cette mission (les diagnostics matériels réalisés relèvent de l'installation de l'infrastructure).
- [ ] **B1.3 Développer la présence en ligne** : compétence non mobilisée dans cette mission.
- [x] **B1.4 Travailler en mode projet** : projet d'infrastructure découpé en étapes successives (état des lieux, hyperviseur, machine virtuelle, composants MDT, tests, import de Marseille), avec un objectif de standardisation, un remplacement de ressource matérielle en cours de route et une coordination prévue avec l'agence de Marseille.
- [x] **B1.5 Mettre à disposition des utilisateurs un service informatique** : mise en place d'un service de déploiement automatisé, tests d'intégration (machine virtuelle témoin, puis amorçage PXE sur un poste physique) et mise en production de l'infrastructure locale.
- [x] **B1.6 Organiser son développement professionnel** : appropriation de technologies que je n'avais pas l'habitude de côtoyer (Proxmox VE, MDT, WDS, contrôleur RAID PERC), à partir de recherches en ligne et de la pratique en équipe.

---

## 7. BILAN RÉFLEXIF ET AUTO-ÉVALUATION

- **Points forts** :
  - Une chaîne de déploiement validée de bout en bout : de l'hyperviseur à l'amorçage PXE d'un poste physique, avec génération des images LiteTouch.
  - Une démarche de diagnostic matériel méthodique (ventilateur, disques, RAID, iDRAC) qui a permis de décider rapidement de changer de serveur.
  - Une infrastructure virtualisée sous Proxmox VE, sur un serveur recyclé, qui offre un socle réutilisable pour les déploiements de l'agence.
  - Une installation et une configuration menées en grande partie par moi, de l'installation du système jusqu'à la configuration de MDT et de WDS.
- **Axes d'amélioration** :
  - **Audit préalable** : vérifier l'état matériel complet d'un serveur récupéré (ventilation, disques, contrôleur RAID) avant de planifier le projet, pour éviter de changer de châssis en cours de route.
  - **Supervision** : l'iDRAC6 étant inaccessible, le monitoring hors-bande n'a pas pu être mis en place, ce qui reste à rétablir ou à remplacer par une autre solution.
  - **Harmonisation** : importer le Deployment Share de l'agence de Marseille (applications, pilotes, séquences de tâches avancées) pour déployer des postes identiques à la charte technique de l'entreprise.
  - **Capitalisation** : documenter la procédure et les paramètres retenus dans la base de connaissances de l'agence.
- **Compétences professionnelles consolidées** :
  - Techniques : virtualisation avec Proxmox VE, gestion du stockage et des volumes RAID (PERC), installation de Windows Server 2022, chaîne de déploiement Windows (ADK, WinPE, MDT, WDS), amorçage réseau PXE, configuration réseau statique.
  - Méthodologiques : conduite d'un projet d'infrastructure par étapes, diagnostic matériel, coordination avec une autre agence, esprit d'équipe avec les techniciens.