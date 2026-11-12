# Feuille de route DT — V1.3

Widget Grist en lecture seule, destiné à parcourir les chantiers par périmètre.

## Contenu à publier

- `index.html` : interface, styles, traitement des données et connexion Grist.
- `.nojekyll` : désactive le traitement Jekyll sur GitHub Pages.
- `README.md` : ce guide ; aucune donnée de test n'est intégrée au widget.

Remplacer le `index.html` du widget précédent. Aucun fichier `app.js`, aucune compilation et aucune nouvelle bibliothèque ne sont nécessaires. Utiliser temporairement `?v=1.3` dans l'URL configurée dans Grist pour éviter de charger une ancienne copie.

## Changements de cette version

- Titre : **Feuille de route DT**.
- Sous-titre correspondant au pôle du périmètre affiché : **Architecture**, puis **Lot 1**, **Lot 2**, etc. lorsque leurs données sont présentes.
- Une seule présentation : un périmètre à la fois, avec sélection du sujet et navigation séquentielle.
- Suppression du commutateur Réunion / Vue d'ensemble et de tout le suivi de séance : sujets abordés, marques, compteurs, bilan, réinitialisation de séance.
- Le bouton **Portefeuille des chantiers** remplace l'ordre du jour. Il reste visible et utilisable en projection.
- Les filtres **Internes DT / Autres directions** sont placés dans le menu **Filtres**.
- Les commandes **←** et **Sujet suivant →** sont placées dans l'en-tête du périmètre. Le passage entre périmètres se fait automatiquement en parcourant les sujets. Le dernier sujet désactive « Sujet suivant », sans ouvrir de bilan.
- Suppression du bandeau inférieur en mode normal. En projection uniquement, une fiche compacte du chantier sélectionné reste affichée, sans suivi de séance ni compteur.
- Les informations complètes restent disponibles via **Détail**, un double-clic sur la ligne ou la touche Entrée.

## Structuration du portefeuille

```text
Pôle Architecture
    Internes DT
        Périmètre
            Chantier
    Avec les autres directions
        Périmètre
            Chantier
Pôle Lot 1
Pôle Lot 2
Pôle Lot 3
...
```

Cliquer sur le nom d'un périmètre affiche sa roadmap. Le chevron déplie la liste de ses chantiers ; cliquer sur un chantier le sélectionne directement. Les totaux indiquent des volumes de chantiers, jamais un avancement de séance.

**Tous les chantiers de la source Architecture actuelle restent dans le Pôle Architecture**, y compris le périmètre nommé « Chantier DT lot 3 ». Aucun pôle n'est déduit des mots « Lot 1 », « Lot 2 » ou « Lot 3 » dans un nom ou un périmètre.

Les pôles Lot 1, Lot 2 et Lot 3 sont préparés dans l'arborescence. Tant qu'ils n'ont pas de lignes, ils portent la mention **Non alimenté** et ne produisent aucune barre ni aucun volume de travail fictif.

### Alimenter les autres pôles

Le widget fonctionne immédiatement avec les 12 colonnes actuelles de la table Architecture. La colonne supplémentaire **Pôle** (identifiant `Pole`) est **facultative** pour cette version.

Pour gérer plusieurs pôles dans la même source :

1. Ajouter une colonne `Pôle` à la table transmise au widget.
2. La rendre accessible parmi les colonnes visibles du widget Grist.
3. Renseigner `Architecture`, `Lot 1`, `Lot 2`, `Lot 3`, ou un autre nom de pôle. Les libellés `Pôle Architecture`, `Pôle Lot 3` sont également reconnus.

Les valeurs absentes sont rattachées par défaut à `Architecture`, pour la compatibilité avec la table existante. Lors de la mise en place d'une source multi-pôles, il faut donc renseigner `Pôle` sur les lignes qui n'appartiennent pas à Architecture. Le nom par défaut et les pôles préparés sont définis par `DEFAULT_POLE` et `PLANNED_POLES` dans le code.

Un filtre Pôle est proposé dans le menu Filtres dès que plusieurs pôles possèdent des données. La recherche inclut aussi le nom du pôle.

**Le widget lit uniquement la table sélectionnée dans Grist** : il ne va pas chercher automatiquement des données dans d'autres tables Lot 1 / Lot 2 / Lot 3. Si les futurs pôles sont stockés dans des tables séparées, une source consolidée ou une évolution de la connexion sera nécessaire.

## Configuration Grist

Conserver la configuration du widget précédent :

- Type de widget : Personnalisé.
- Table source actuelle : Architecture.
- Autorisation : **Lire la table sélectionnée** (`read table`).
- Colonnes transmises : les 12 colonnes existantes ; ajouter `Pôle` lorsqu'elle est créée.

| Colonne | Fonction |
| --- | --- |
| Nom | Intitulé du chantier |
| Périmètre | Regroupement affiché dans la roadmap |
| Volet | Internes DT / Avec les autres directions |
| Phase | Détail du chantier et filtre |
| Urgence | Couleur de la barre et filtre |
| Responsable Projet | Détail et filtre |
| Commentaires | Détail ; résumé en projection lorsque renseigné |
| Date de début | Début de la barre |
| Précision début | Jour, Mois ou Année |
| Date de fin | Fin de la barre |
| Précision fin | Jour, Mois ou Année |
| Ordre | Ordre dans un périmètre |
| Pôle — facultative | Niveau de regroupement supérieur |

Les libellés accentués et les identifiants techniques usuels sont reconnus. La connexion conserve `keepEncoded: true`, `expandRefs: true` et `includeColumns: 'shown'`. Ne pas envoyer uniquement les colonnes Nom et Périmètre : les autres champs sont nécessaires au calendrier et aux filtres.

Documentation de l'API de lecture : https://support.getgrist.com/code/interfaces/grist_plugin_api.FetchSelectedOptions/

## Navigation

- **Portefeuille des chantiers** : accès direct à un pôle, un volet, un périmètre et un chantier.
- **← / Sujet suivant →** : sélection du sujet précédent / suivant dans le portefeuille filtré, avec passage au périmètre suivant si nécessaire.
- **Flèches gauche / droite** : même navigation, lorsque le focus est dans le widget et non dans un champ.
- **Page précédente / Page suivante** (Page Up / Page Down) : changer de périmètre.
- **Entrée**, **Détail** ou double-clic : ouvrir la fiche du chantier.
- **Échap** : fermer la fiche ou le menu ; hors de ces panneaux, quitter la projection.
- **Projection** : agrandir la lecture en masquant les outils secondaires. Le portefeuille reste accessible.

Le mode Projection est une adaptation de la mise en page à l'intérieur du widget, pas un appel au plein écran du navigateur.

## Filtres et affichage

Le menu Filtres contient le volet, le périmètre, l'urgence, la phase, le responsable et la restriction aux fins à préciser ; Pôle apparaît si plusieurs pôles sont alimentés. Tous les filtres se combinent. Le portefeuille est limité aux filtres en cours ; les pôles sans données correspondant aux filtres sont explicitement signalés.

Le menu Affichage conserve l'échelle ajustée à l'écran, mensuelle ou trimestrielle, la période, le repère de date et le détail des lignes. La période reste commune lorsque l'on parcourt les périmètres : elle ne change pas silencieusement à chaque sujet.

Le menu ⋯ conserve l'export du périmètre visible en SVG et l'impression. Hors de Grist, il propose aussi l'ouverture locale d'un CSV ; ce fichier n'est envoyé à aucun serveur.

## Règles conservées

- La couleur d'une barre indique l'urgence, pas l'avancement.
- Une fin passée ne permet pas de déclarer automatiquement un chantier terminé ou en retard.
- Les dates manquantes restent à préciser ; aucune fin n'est inventée.
- Une date à l'année représente une précision annuelle, affichée par hachures.
- Un début connu sans fin comporte un marqueur et une portion pointillée, pas une durée supposée.
- Le widget n'écrit ni dans Grist, ni dans le stockage local ; il ne stocke plus de suivi de séance.
- Les textes métier injectés dans l'interface sont échappés avant affichage.

## Vérifications effectuées

Contrôles avec Chromium et une API Grist simulée : lecture des 39 lignes de l'export, navigation entre sujets et périmètres, filtres de volet, portefeuille, mode projection, popups, absence de commandes de séance, API en lecture seule, export SVG, import CSV, dimensions 1920 / 1440 / 1366 / 1024 / 390 px, ajout simulé des pôles Lot 1 / Lot 2 / Lot 3, regroupements homonymes séparés par pôle, dates typées Grist et échappement HTML.

Les tests ont été effectués localement ; aucun accès au document Grist de production n'a été utilisé. Les lignes artificielles de test ne sont pas présentes dans les fichiers livrés.
