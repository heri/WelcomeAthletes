# WelcomeAthletes
WelcomeAthletes est une plateforme réunissant des athlètes amateurs et professionnels, ainsi que leurs ami(e)s et communautés. WelcomeAthletes propose de:
* rendre possible l’hébergement solidaire en acceptant d’une part les propositions d’hébergement dans les lieux de compétition, et en prenant d’autre part les demandes d’hébergement provenant d’athlètes,
* offrir une communauté active en ligne avec des nouvelles, discussions où l’entre-aide et l’esprit amical (et sportif) prime

# Projet
* Design 1 semaine
* Developpement 3.75 semaines
* Test 2 semaines
* Serveurs & maintenance 1.45 mois


# Architecture Technique
L’architecture sera une implémentation standardisée Ruby On Rails.
* Framework Ruby on Rails v 4.2
* Base de données PostgreSQL
* unicorn comme serveur applicatif, nginx en front
* jQuery / Turbolinks / Bootstrap. Dans le futur, un framework front-end comme React pourrait etre utilisé
* Sidekick pour gestion de la file d’emails
* Devise pour identification
* paperclip pour les images, avec potentiellement Amazon S3 pour hébergement
* NewRelic pour le suivi de performance (temps de chargements etc.)
* tests unitaires, fonctionnelles et intégrations avec rspec et Factory Girl
* Airbrake pour les erreurs (bugs)
* Un hébergement DigitalOcean, deuxième hébergeur mondial. On choisira un centre de données à Toronto pour commencer. Le serveur sera agrandi au fur et à mesure des besoins en mémoire vive et latence.
* github/zenhub pour l’hébergement des codes, suivi des “issues“ et gestion de projet

# Fonctionalités

## GESTION DE COMPTE

* Les usagers doivent être capables de créer un nouveau compte :
    * Création de compte avec Email et mot de passe - NON suffisant pour envoyer une requête d’hébergement
    * Création d’un compte ‘Général’ avec une liste de champs supplémentaires à compléter
    * Les usagers doivent  être capables de se connecter avec leur identifiant + Mot de passe (s’ils sont déjà inscrits). + Possibilité de récupérer son Identifiant et Mot de Passe si oublié.
* Les usagers doivent obligatoirement être en accord avec les termes d’utilisation qui spécifient notamment la non-responsabilité du site
* Un compte a des identifiants (email, mot de passe).  Un compte contient aussi certaines informations d’ordre Général (compte Général). Certaines informations de ce compte Général sont spécifiques si l’usager décide d’être Hébergeur .  Information du compte Général :
    * Photo usager,
    * nom, affiché seulement aux autres usagers quand un hébergement a été finalisé
    * prénom,
    * Date de naissance (ou tranche d’âge ?), jamais affiché mais utilisé pour des fins internes
    * Sexe
    * sport(s) pratiqués
    * numéro de membre Triathlon Québec - optionnel et apparait seulement quand le sport triathlon a été choisi
    * numéro d’autre fédération que l’usager pourra nommer avant d’inscrire son numéro de membre - optionnel
    * biographie (texte libre, pourquoi je m’inscris à WA) - optionnel
    * Lien vers profil externe (Strava, Facebook..) - optionnel
    * INFORMATIONS SPÉCIFIQUES HÉBERGEMENT - optionnel
* Un internaute non inscrit autant que les membres connectés (compte Simple ou Général) peuvent initier une recherche d’hébergement, voir les résultats et les fiches détaillées. Mais impossible d’envoyer une requête si pas connecté et inscrit avec un compte Général . Appelés ‘usager’ ci après
* L’usager peut mettre à jour ses informations Générales et Spécifiques dans une page dédiée accessible lorsqu’il est connecté
* L’usager peut uniquement avoir un profil actif type Athlète (donc sans offre d’hébergement)
* Il peut effectuer une requête privée d’hébergement avec ce profil. Par contre il doit avoir complété son profil GÉNÉRAL ( le compte Simple : Email + Mot de Passe ne suffit pas) afin que l’hébergeur puisse consulter sa fiche s’il  le souhaite et avoir un minimum d’informations sur lui. Sont notamment obligatoires : nom, prénom, photo, sport, tranche d’age, biographie (Avec un mimum de caractères)
* L’usager peut compléter et activer son profil Hébergeur lorsqu’il le souhaite, ou le désactiver. Les changements se font instantanément.
* L’usager peut rajouter ou effacer des journées/périodes disponibles, dans un calendrier graphique comme Google calendar.  Les changements se font instantanément. Le moteur viendra chercher les informations de disponibilité d’un hébergeur dans ce calendrier  et fera ainsi apparaitre ou non sa fiche dans les résultats.

## PROCESS DE RECHERCHE D’HÉBERGEMENT PAR UN USAGER
* Un usager initie sa recherche sur la page d’accueil en complétant LIEU uniquement. (Date ? Nb de personnes ? A discuter, le but étant d’optimiser l’expérience utilisateur)
* Le système proposera les localités automatiquement à partir des premières lettres  tapées dans un moteur déroulant (cf Airbnb, Couchsurfing..)
Les résultats de la recherche apparaitront sous forme de CARDS dont la forme reste à déterminer, ainsi que sur l’API Google Maps sur la droite de l’écran ( type Airbnb)
* Les Cards donneront ces informations :
    * ( Photo du logement (taille modérée)
    * information minime à propos de l’hébergeur (à finaliser)
    * Texte
    * Localité mais pas l’adresse exacte
    * A priori pas d’autre texte (A valider..peut être capacité d’accueil ?)
    * Le nombre de cards affiché initialement sera à déterminer, de même que leur agencement sur la page.
    
* Une carte Google Maps apparaitra sur le côté droit, avec les résultats sous forme d’épingle. Le niveau de zoom sera à determiner ( = si le zoom est resserré sur le lieu de recherche de l’usager, tous les résultats n’apparaitront peut-être pas sur la carte, cela dépendra des paramètres choisis).
* Possibilité de zoomer et dézoomer la carte Maps
* Rendre la carte et la liste de résultats interactive (des Cards apparaissent ou disparaissent en fonction du niveau de zoom de la carte).
* L’usager peut affiner la liste de résultats en utilisant des filtres :
    * Date (un calendrier apparait). FILTRE OBLIGATOIRE ( si pas directement sur la recherche initiale)
    * Nombre de personnes (préciser athlète/accompagnant/enfant ?)
    * Ces filtres seront du même genre que ceux d’AirbnB, visibles directement (ne pas avoir à cliquer sur un bouton du genre ‘ Recherche avancée’, et les plus réactifs possible (refresh automatiquement idéalement, en évitant le bouton ‘OK’)
* Les recherches sont faites dans un rayon de quelques kilomètres autour du lieu mentionné dans la recherche initiale (dont l’adresse est convertie en coordonnées GPS). Ex : si nous définissons le rayon = 1 km, toutes les cards dont l’adresse GPS est dans un rayon de 1km de l’adresse GPS souhaitée par l’usager apparaitront dans la liste de résultats initiale et sur la carte Maps.  C’est en dézoomant la carte Maps qu’on fera apparaitre de nouveaux résultats (on élargit le champ) et en zoomant qu’on en fera disparaitre (on rétrécit le champ)
* En cliquant sur  une épingle sur Maps, cela montre plus de détails
* Un usager peut voir plus de détails pour une offre en cliquant  sur la card correspondante
* S’il est satisfait, il peut faire une requête privée par un bouton sur sa fiche ‘Envoyer une demande d’hébergement’.
* Un compte  complété de façon Général sera nécessaire pour accéder à cette étape.  Et bien sur d’être connecté sur ce compte.
* La requête privée ressemble à un pop up type ‘Requete Couchsurfing’, ou apparaitront :
    * Nom et Prénom de l’usager sont pré-remplies (provenant du compte Général)
    * Les dates spécifiées par l’usager sont pré-remplies et il ne peut pas les changer.
    * Champ à compléter obligatoire  : nombre de personnes ( detail adultes/enfants/athletes ??) . (ou pré remplies s’il a complété l’info dans les filtres. Mais il faut que ce soit conforme à ce que propose l’hébergeur, sinon ce dernier refusera..mauvaise UX)
    * Champ à compléter obligatoire : entrainement / Course (case à cocher), avec un champ libre pour détail et mot d’accompagnement

* la requête  privée crée un email que recevra l’hébergeur et que l’usager pourra visualiser dans sa messagerie interne
* Il est possible de contacter n’importe quel hébergeur sans nécessairement envoyer une requête d’hébergement par un bouton sur sa fiche de type ‘ Contacter l’hébergeur’. Pour cela il faut avoir un compte Général complété.
* La messagerie interne sera aussi simple que possible et permettra de conserver les emails envoyés (dont les requêtes d’hébergement et les Contacts hébergeurs) et les emails reçus.  Il n’y aura pas de possibilité de créer des nouveaux messages à partir de la messagerie interne – ce sera une continuité du message de départ.
* Il y a des va-et-vient dans la messagerie jusqu'à ce que l’hébergeur accepte ou refuse la demande.
* L’hébergeur devra avoir accès au bouton d’acceptation de la demande facilement (pour bonne expérience utilisateur).
* L’hébergeur doit pouvoir consulter la fiche de l’usager ayant fait la requête. Il aura donc accès à son profil Général (Si possible, certaines informations pourraient rester en tout temps confidentielles – ex : date de naissance. Sera plutôt présenter en groupe d’âge).
* Lorsque l’hébergeur valide la requête en appuyant sur le bouton ‘Accepter la demande’, l’usager et l’hébergeur reçoivent un courriel de confirmation reprenant les détails de l’hébergement et des 2 parties.
* L’usager doit pouvoir annuler sa requête en tout temps, avant acceptation ou après acceptation de l’hébergeur. En cliquant sur le bouton ‘Annuler la demande’ et ‘Annuler l’hébergement préalablement confirmé’ (si déjà accepté), l’hébergeur est informé de l’annulation de l’usager.
* L’hébergeur doit pouvoir annuler aussi son offre d’hébergement ???? ( idéalement non… est ce que cela doit passer par l’administration ?)

## INFORMATIONS SPECIFIQUES D’UN COMPTE GÉNÉRAL + HÉBERGEUR
* Parmi la liste de champs à compléter pour remplir son profil d’hébergeur, certains doivent pouvoir être obligatoire (à déterminer lesquels)
    * Photo de l’hébergeur
    * Si pas de photos de l’hébergeur, choix par défaut dans une banque
    * Nom
    * Prénom
    * Numéro de membre et fédération (une case spécifique pour TQ)
    * Sports pratiqués
    * Photos du logement
    * Si pas de photo pour le logement, mettre une photo par défaut parmi une banque.
    * Liste des champs de données et des textes libres (non exhaustif) :
    * Nombre de personnes pouvant être accueillies
    * Chambres fermée/ouverte (cases à cocher)
    * Lit double/simple/canapé lit (cases à cocher)
    * Accès cuisine
    * Accès Internet
    * Texte libre de présentation
    * Reviews:
    * Etoiles
    * Commentaires (2 premières lignes visibles, cliquer pour voir le reste)
    * Nombre de fois que ce profil a hébergé quelqu’un (c’est a dire quand une demande d’hébergement a été accepté et que la date est passé)
    * Nombre de fois que ce profil a été hébergé (quand une demande d’hébergement a été accepté

## VISUEL
* La page d’accueil fera apparaitre (avant scroll) :
    * Logo WA + La mention à TQ (Logo renvoyant vers le site TQ)
    * Du  texte ( ex Airbnb, Nextdoor, Lafourchette, Couchsurfing, Strava)
    * Des champs (avec  du texte à l’intérieur, ex sites ci dessus) :
        * Lieu
        * Dates
    * Menus principal : Onglets vers Term of use, Règlements et Sécurité, Qui sommes nous, On parle de nous,  Contactez nous, Plan de site,
    * Menu secondaire : Sign in, FR/ENG
    * Une ‘étiquette’ mentionnant que ceci est la version Alpha du site WelcomeAthletes.com
* En scrollant vers le bas, nous aurons accès à ces informations :
    * 1,2,3 (How It works)
    * Exemple de Cards
    * Bloc :   Nouvelles fonctionnalités à venir, notre stratégie (texte en dur).  Possibilité de sondage pour améliorer le sentiment d’appartenance (dites nous ce que vous aimeriez)
    * Bloc : Feedback utilisateur /  Messages diverses à l’attention de WA ?? ( widget)
    * Bloc : Espaces partenaires (logo, publicités)

## Gestion des donations
* Via un onglet sur la page d’accueil
* Lorsqu’une requête est validée
* Utilisation de Paypal (widget) pour membership

## GESTION DES EMAILS
* Possibilités d’envoyer des courriels à certaines cibles bien déterminées, depuis une adresse type contact@welcomeathletes.com  (suppose la création des adresses en @welcomeathletes.com
    * Tous les membres
    * Membre sans profil d’hébergeur
    * Membre avec profil d’hébergeur
    * Membres ayant utilisé le service d’hébergements
    * Ayant fait une donation
    * N’ayant pas fait une donation
    * Membres habitant dans une certaines regions donnée ( possible ?)

## TRADUCTION
* Le système reconnaitra la langue du navigateur et de l’OS. Si la personne a par exemple mis son Google Chrome en anglais, le site sera en anglais. Sauf si après il clique sur français.  A discuter
* Tous les textes seront traduits sauf les textes rentrés par les usagers.

## GESTION DES COMPTES PAR L’ADMINISTRATION
* Possibilité de nettoyer des comptes utilisateurs (propos déplacés, fautes d’orthographe, absence de photo).
