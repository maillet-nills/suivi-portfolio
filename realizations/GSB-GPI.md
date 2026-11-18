# FICHE DE SITUATION PROFESSIONNELLE : BLOC 1 (ÉPREUVE E5)

## 1. IDENTIFICATION GÉNÉRALE

- **Titre de la réalisation** : GSB-GPI, application de bureau de gestion du matériel informatique et des incidents
- **Cadre de réalisation** :
  - [ ] Stage 1
  - [ ] Stage 2
  - [ ] Atelier de professionnalisation
  - [x] Projet de cours (BTS SIO option SLAM, Lycée Henri Matisse, Toulouse, enseignante référente : Mme Exposito)
- **Période de réalisation** : du 12 octobre au 15 novembre 2026 (environ 5 semaines)
- **Modalité** : 
  - [ ] Individuel
  - [x] En équipe (3 personnes : Nills Maillet, chef de projet et développeur, Ali Hassan Karar, développeur, Mattéo Dauriac, développeur)
- **Localisation / Organisation cliente** : Laboratoire Galaxy Swiss Bourdin (GSB), organisation fictive du cas d'étude, projet réalisé au Lycée Henri Matisse (Toulouse)
- **Lien GitHub** : https://github.com/maillet-nills/GSB-GPI


---

## 2. CONTEXTE ET OBJECTIFS

- **Contexte organisationnel** : le laboratoire GSB est né de la fusion entre le groupe américain Galaxy et le laboratoire européen Swiss Bourdin. Il compte plus de 500 collaborateurs répartis en métropole et outre-mer. Son parc informatique est vaste et hétérogène : plus de 350 postes de travail, des tablettes, des serveurs, des équipements réseau et du matériel sous location.
- **Problématique / Besoin exprimé** :
  - L'inventaire du matériel est difficile à tenir à jour, car les équipements sont dispersés sur plusieurs sites et relèvent de statuts différents (propriété, location, garantie).
  - Le traitement des incidents manque de rapidité et de traçabilité, faute d'outil commun aux techniciens.
  - GSB a donc besoin d'une solution centralisée, dans la lignée d'outils comme GLPI.
- **Objectifs fixés** :
  - **Inventaire** : tenir un inventaire précis et à jour du parc (postes de travail, serveurs, équipements réseau), avec les données de location, de garantie et de fournisseur.
  - **Incidents** : assurer le suivi des tickets de leur déclaration à leur résolution (objet, demandeur, état, urgence, niveau d'intervention).
  - **Habilitations** : adapter les fonctionnalités au profil (utilisateur simple, technicien, responsable).
  - **Objectif pédagogique** : mener en équipe un développement complet, de la conception à la documentation, en appliquant l'architecture MVVM et en apprenant de nouvelles technologies.

---

## 3. DÉMARCHE ET ENVIRONNEMENT TECHNIQUE

### Environnement technologique mobilisé

- **Systèmes d'exploitation** : Windows (postes de développement et postes clients cibles, publication en win-x64)
- **Logiciels, rôles ou paquets** :
  - Langage et framework : C#, .NET 10, Avalonia UI (fichiers .axaml)
  - Architecture : MVVM avec CommunityToolkit.Mvvm (propriétés observables, RelayCommand)
  - Accès aux données : Entity Framework Core avec le fournisseur Npgsql, requêtes LINQ
  - Sécurité : hachage des mots de passe avec BCrypt.Net
  - Base de données : PostgreSQL hébergé sur Supabase
  - Outils : JetBrains Rider, Git et GitHub (dépôt GSB-GPI), Figma, Canva, GanttProject, Notion
  - Sauvegarde : utilitaire pg_dump de PostgreSQL piloté par un script .bat
- **Matériels ou éléments d'infrastructure concernés** :
  - Base de données distante hébergée sur Supabase (service cloud)
  - Postes clients Windows qui exécutent GSB-GPI.exe (exécutable autonome)
  - Parc géré par l'application et utilisé comme jeu de données de test : postes de travail Dell Yoga, serveur Dell R570, pare-feu Stormshield Z456

### Démarche suivie étape par étape

1. **Analyse préalable et préparation de la maquette**
   - Analyse du cahier des charges : trois profils d'utilisateurs, modules d'authentification, de tableau de bord, d'inventaire et de tickets.
   - Planification en mode projet : huit tâches réparties en trois phases (conception, développement, documentation), avec durées, ressources et dépendances, suivies sous GanttProject et Notion.
   - Conception de l'architecture MVVM, avec une classe statique DbToolBox qui centralise les accès à la base, et réalisation d'un diagramme de classes.
   - Conception des interfaces sous Figma (LoginView, RegisterView, DashboardView, InventoryView, etc.), avec prototype de navigation et variables de couleurs.
   - Conception de la base de données : MCD puis MLD (entités Equipment avec héritage Server, NetEquipment et Workstation, User, Technician, Ticket et Log).
2. **Configuration et mise en œuvre technique**
   - Création des tables sur Supabase conformément au MLD.
   - Développement des models : une classe C# par table, annotée pour Entity Framework Core ([Table], [Key], [Column]).
   - Configuration du contexte GsbGPIDbContext (DbSet, connexion via OnConfiguring, contraintes et conversions via OnModelCreating).
   - Développement de la DbToolBox : Login avec vérification BCrypt, CreateTicket, UpdateUser, DeleteTicket, journalisation AddLog, etc.
   - Développement des Views Avalonia, des ViewModels et de la navigation, avec bindings et commandes à la place des événements de clic, et un menu qui s'adapte au profil connecté.
   - Mise en place des logs (type, message, adresse IP publique, utilisateur, horodatage) dans une table dédiée, et création du logo sous Canva.
   - Déploiement : compilation en Release, publication autonome (self-contained, win-x64, fichier unique) depuis Rider, qui produit GSB-GPI.exe sans installation du SDK .NET.
   - Sauvegarde : script .bat basé sur pg_dump, avec format compressé et horodatage du fichier.
3. **Tests d'acceptation et vérification du bon fonctionnement**
   - Vérification fonctionnelle de chaque module sur un jeu de données de test (équipements, tickets, comptes des trois profils : Client, Technicien, Admin), avec contrôle que chaque profil ne voit que les écrans et les données qui lui sont destinés.
   - Contrôle de la traçabilité : chaque connexion, création, modification ou suppression génère une ligne dans la table des logs.

### Gestion des imprévus / Incidents rencontrés

| Difficulté | Solution ou enseignement |
|---|---|
| Absence d'un membre pendant toute la conception, d'où un décalage dans l'appropriation de l'architecture | Conception menée à deux, puis développement des models à trois pour partager la même compréhension du modèle de données |
| Chevauchement de plusieurs tâches début novembre, d'où des surcharges ponctuelles (200 % de charge le 05/11 pour moi, du 04 au 05/11 pour Ali Hassan Karar) | Surcharges absorbées sans retard sur les jalons. Le planning prévisionnel avait prévu une répartition qui les évitait. Leçon : répartir plus tôt les responsabilités |
| Travail simultané de plusieurs développeurs sur des briques interdépendantes (models, interfaces, DbToolBox) | Dépôt GitHub comme point de rencontre et vigilance constante lors des synchronisations |
| Récupération de l'adresse IP publique pour les logs, qui peut échouer ou ralentir l'application | Appel HTTP avec délai maximal de 3 secondes et valeur nulle en cas d'échec, pour ne jamais bloquer l'application |
| Documentation à rédiger en 3 jours en fin de projet | Répartition de la rédaction par volets (gestion de projet, technique, manuel utilisateur), puis harmonisation du style et vérification de la cohérence entre schémas, captures et code |

---

## 4. RÔLE ET CONTRIBUTION PERSONNELLE

- **Responsabilité précise** : chef de projet et développeur. J'ai piloté la planification et le suivi du projet, et j'ai codé les interfaces et la navigation de l'application.
- **Actions menées individuellement** :
  - **Gestion de projet** : découpage du projet en huit tâches et trois phases, construction du planning prévisionnel et du planning effectif sous GanttProject, suivi de l'avancement et de la charge de chaque membre, organisation de l'espace de travail Notion.
  - **Conception (en binôme avec Ali Hassan Karar)** : architecture logicielle MVVM, maquettes Figma, conception de la base de données (MCD et MLD).
  - **Développement des models** (avec les deux autres membres).
  - **Développement des interfaces et de la navigation** : Views Avalonia (.axaml) et ViewModels, dont la fenêtre principale et la navigation (MainWindowViewModel, ShellViewModel), l'écran de connexion et d'inscription, la liste des tickets et la fiche d'un ticket, avec bindings et RelayCommand.
  - **Documentation** : participation à la rédaction du rapport et production des schémas associés (diagramme de Gantt, charge de travail, architecture MVVM, diagramme de classes, MCD et MLD).
  - **Répartition réelle de l'effort** : 28,0 jours-ressource, soit 44,9 % du total de l'équipe (62,4 jours).
- **Ce que je n'ai pas réalisé seul** : la DbToolBox et la base de données ont été développées principalement par Ali Hassan Karar. Je l'ai conçue avec lui et je l'appelle depuis les ViewModels. Les logs et le logo ont été pris en charge par Mattéo Dauriac.

---

## 5. LIVRABLES ET PREUVES ASSOCIÉES

- [x] **Procédure technique ou document d'exploitation** : documentation utilisateur par profil (connexion et inscription, navigation, déclaration et traitement d'un incident, inventaire, administration des utilisateurs) et procédure de compilation autonome sous Rider
- [x] **Schémas d'architecture** : Diagramme 1 (architecture MVVM), Diagramme 2 (diagramme de classes), Diagramme 3 (MCD), Diagramme 4 (MLD)
- [x] **Planification** : diagrammes de Gantt prévisionnel et effectif (Figures 1 et 2), répartition de l'effort et des tâches (Figures 3 et 4), affectation des ressources (Figure 5), charge journalière (Figure 6)
- [x] **Extraits de configurations ou scripts commentés** : structuration des models (Code 1), GsbGPIDbContext (Code 2), méthodes Login, DeleteTicket, CreateTicket et UpdateUser de la DbToolBox, AddLog, commande Delete du ViewModel et son binding .axaml, script de sauvegarde pg_dump (annexe)
- [x] **Captures d'écran significatives** : tables Supabase, maquette Figma de la LoginView, connexion, inscription, tableau de bord, liste des incidents, fiche d'un ticket, inventaire, fiche équipement, gestion des utilisateurs, logo
- [x] **Code source versionné** : dépôt GitHub `https://github.com/maillet-nills/GSB-GPI`

---

## 6. COMPÉTENCES DU BLOC 1 MOBILISÉES

- [x] **B1.1 Gérer le patrimoine informatique** : le module d'inventaire recense le parc (postes de travail, serveurs, équipements réseau) avec marque, modèle, date d'achat, fournisseur, location, garantie et affiliation à un utilisateur. Les niveaux d'habilitation par profil, le hachage des mots de passe et le script de sauvegarde pg_dump relèvent aussi de cette compétence.
- [x] **B1.2 Répondre aux incidents et demandes d'assistance** : le module de tickets couvre toute la chaîne, de la déclaration (objet, équipement concerné, urgence) à la clôture, avec attribution à un technicien, niveau d'intervention (N1 à N3) et états successifs (enregistré, traitement, résolu, clôturé). Les logs assurent la traçabilité des actions.
- [ ] **B1.3 Développer la présence en ligne** : compétence peu mobilisée dans ce projet.
- [x] **B1.4 Travailler en mode projet** : découpage en tâches, diagrammes de Gantt prévisionnel et effectif, affectation des ressources, analyse des surcharges, suivi sous Notion, travail collaboratif sur GitHub, rédaction d'un rapport de projet et d'un bilan.
- [x] **B1.5 Mettre à disposition des utilisateurs un service informatique** : publication d'un exécutable autonome déployable sur les postes clients sans SDK, documentation utilisateur illustrée par profil, interface adaptée à chaque habilitation, procédure de sauvegarde de la base.
- [x] **B1.6 Organiser son développement professionnel** : appropriation de technologies nouvelles (Avalonia UI, CommunityToolkit.Mvvm, Entity Framework Core, Supabase) à partir de documentations officielles référencées (Microsoft Learn, Avalonia, Supabase, PostgreSQL), et bilan critique du projet avec pistes d'amélioration.

---

## 7. BILAN RÉFLEXIF ET AUTO-ÉVALUATION

- **Points forts** :
  - Une conception rigoureuse en amont (MCD, MLD, maquettes Figma, diagramme de classes) qui a sécurisé le développement et limité les retours en arrière.
  - Une architecture MVVM stricte et une classe DbToolBox qui centralise les accès aux données, ce qui allège les ViewModels et facilite la maintenance et le débogage.
  - Une sécurité de base solide : mots de passe hachés avec BCrypt, requêtes LINQ via l'ORM (qui protège contre l'injection SQL), journalisation des actions sensibles avec horodatage et adresse IP.
  - Des délais globaux maîtrisés : projet livré à la date prévue, avec les trois objectifs opérationnels atteints.
- **Axes d'amélioration** :
  - **Sécurité** : sortir les identifiants et la chaîne de connexion du code source (fichier de configuration externe non versionné) et activer le Row Level Security sur Supabase en complément des restrictions côté client.
  - **Automatisation** : intégration continue avec GitHub Actions (compilation et tests unitaires sur les utilisateurs, équipements et tickets) et automatisation planifiée des sauvegardes, aujourd'hui lancées via un script.
  - **Calendrier** : répartir plus tôt et plus équitablement les tâches de développement pour éviter les surcharges de début novembre, et mieux intégrer un membre absent pendant la conception.
  - **Fonctionnel** : interface de consultation des logs avec filtres, notifications pour les tickets urgents, pièces jointes aux tickets, indicateurs statistiques et exports.
- **Compétences professionnelles consolidées** :
  - Techniques : structuration d'une application de bureau complète en MVVM avec Avalonia, accès aux données avec Entity Framework Core, LINQ et PostgreSQL, modélisation relationnelle avec héritage, sécurité applicative et journalisation, publication d'un exécutable autonome.
  - Méthodologiques : conduite de projet en équipe (planification, suivi, gestion de la charge), versionnement avec Git, rédaction d'une documentation technique et utilisateur.
