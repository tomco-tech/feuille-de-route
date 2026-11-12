# Pilotage DT — Feuille de route architecture · V1.1

Widget Grist en lecture seule. Version allégée : le planning occupe toute la largeur et la hauteur restante du widget. Aucune donnée métier n'est intégrée au code.

## Mise à jour depuis V1

Remplacer `index.html` dans le dépôt ou le dossier GitHub Pages existant. Les réglages Grist et le modèle de la table `Architecture` ne changent pas. Ajouter temporairement `?v=1.1` à l'URL du widget pour distinguer cette version.

Le dossier à publier contient uniquement :

- `index.html` : interface, styles et logique de lecture ;
- `.nojekyll` : fichier vide pour GitHub Pages ;
- `README.md` : cette documentation.

Ne pas publier les exports CSV ou les documents métier sur GitHub. Le widget reçoit les données depuis Grist.

## Nouvelle interface

L'écran conserve en permanence le titre, les trois volets **Tous / Internes DT / Autres directions**, la recherche par mot-clé et les boutons **Filtres** et **Affichage**. Les grands indicateurs, sous-titres, explications permanentes et pieds de page ont été retirés de la vue principale.

Le bouton **Filtres** ouvre un panneau flottant avec les recherches Périmètre, Phase, Urgence et Responsable projet. Une case permet de ne garder que les fins à préciser. Un badge indique le nombre de filtres avancés actifs ; le bouton × près d'Affichage apparaît lorsqu'un filtre ou une recherche est appliqué. Les panneaux ne réduisent pas la hauteur du planning.

Le bouton **Affichage** regroupe l'échelle, la période, la date du repère vertical, les durées consolidées et les commandes pour replier ou déplier les périmètres. L'option **Afficher les phases et les dates sur chaque ligne** restitue la vue détaillée ; elle est désactivée au chargement. La vue d'ensemble adopte des graduations trimestrielles lorsque des mois seraient trop serrés. L'échelle Mois conserve un défilement horizontal pour une lecture plus précise.

Les lignes sont plus compactes. Les noms de chantiers restent fixés à gauche et l'axe du temps reste visible pendant le défilement. Un clic sur un chantier ou sa barre affiche sa fiche détaillée, avec phase, dates, précision, urgence, responsable et commentaires. Fermer par la croix, par clic à l'extérieur ou avec Échap.

Le menu **⋯** contient l'export SVG et l'impression. Hors Grist, il propose également l'ouverture d'un CSV local. Cette prévisualisation n'est pas une connexion à Grist et le CSV n'est pas envoyé à un serveur.

## Table et accès Grist

Table source : `Architecture`. Choisir **Lire la table sélectionnée / Read selected table** et rendre les 12 colonnes accessibles au widget :

| Nom visible | Identifiant usuel |
| --- | --- |
| Nom | Nom |
| Périmètre | Perimetre |
| Volet | Volet |
| Phase | Phase |
| Urgence | Urgence |
| Responsable Projet | Responsable_Projet |
| Commentaires | Commentaires |
| Date de début | Date_de_debut |
| Précision début | Precision_debut |
| Date de fin | Date_de_fin |
| Précision fin | Precision_fin |
| Ordre | Ordre |

Le rapprochement des colonnes ignore les accents, la casse, les espaces et les underscores. Le widget conserve la lecture des références développées et des valeurs encodées de Grist, sans écrire dans le document. Il n'a pas besoin de l'accès complet au document.

## Règles conservées

- Regroupement par Volet, puis Périmètre ; tri par Ordre à l'intérieur des périmètres.
- Les couleurs représentent l'urgence, et non le statut d'avancement : Mineur, Intermédiaire, Prioritaire, À qualifier/non renseignée.
- Les dates au mois et à l'année conservent leur précision. Une année seule est représentée par une zone hachurée.
- Une date de fin absente reste à préciser ; le widget n'invente aucune date de livraison.
- Un calendrier absent reste sans barre. Les dates invalides ou incohérentes sont signalées.
- Le repère vertical indique le jour courant par défaut. Il peut être modifié dans Affichage.
- Le nombre de lignes, les dates et les calculs ne sont pas altérés par la simplification visuelle.

## Vérifications réalisées

Tests en navigateur Chromium avec l'export Architecture fourni et une API Grist simulée : 39 chantiers, filtres, recherche, volets, fenêtres de détail, groupes repliables, affichage détaillé optionnel, périodes, échelles, export SVG, actualisation des lignes et chargement CSV local.

Vérification de la mise en page sur des fenêtres 1920×1080, 1440×900, 1366×768, 1024×768, 768×600, 390×844 et 1440×500. Sur les formats bureau testés, le calendrier occupe environ 80 à 86 % de la hauteur de la fenêtre, en affichage normal et sans message d'erreur. Sur les petits écrans, le défilement reste à l'intérieur du planning.

Le moteur de normalisation des données et des dates est identique à V1. Pas de validation directe dans votre document Grist de production.
