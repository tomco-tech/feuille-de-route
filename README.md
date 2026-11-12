# Feuille de route DT — V1.5

Widget Grist en lecture seule, à publier sur GitHub Pages. Cette version reprend la V1.4 et ajuste la navigation sans changer le modèle de données.

## Fichiers à publier

- `index.html` : code complet du widget (HTML, CSS, JavaScript).
- `.nojekyll` : fichier vide à conserver.
- `README.md` : documentation ; aucune donnée métier n'est embarquée dans le code.

Remplacer `index.html` dans le même dossier GitHub Pages. Aucune compilation ni nouveau fichier JavaScript n'est nécessaire. Dans l'URL du widget, remplacer le paramètre de version précédent par `?v=1.5` (ou `&v=1.5` si un autre paramètre existe déjà).

## Modifications V1.5

- **Portefeuille des chantiers** devient le premier bouton du bandeau de commandes, avant la recherche, les filtres et l'affichage. Le même ordre est conservé en projection.
- Le bouton utilise un fond **bleu foncé `#244D70`** et un texte blanc. Le survol et l'état ouvert restent bleu foncé.
- Un bouton **Périmètre suivant →** est placé à droite du titre du périmètre, en mode normal et en projection.
- Il avance d'un périmètre entier dans l'ordre du portefeuille, en tenant compte des filtres. Le premier chantier du périmètre suivant est sélectionné et le défilement vertical revient en haut. La période et l'échelle du calendrier sont conservées.
- Il est désactivé pendant le chargement, sans périmètre disponible ou au dernier périmètre de la sélection. Il ne revient pas automatiquement au début.
- Les boutons Sujet suivant, Retour et Détail ne sont pas réintroduits. Le reste de l'interface V1.4 est conservé : couleurs sauge / ambre / mauve, légende des hachures et heure d'actualisation à côté du titre.

## Navigation conservée

Le portefeuille reste organisé en **Pôle → Volet → Périmètre → Chantier**. Cliquer sur un périmètre affiche sa roadmap ; son chevron déplie les chantiers.

Le bouton Portefeuille reste disponible en projection. La projection conserve le résumé du chantier sélectionné en bas, mais aucun bouton Détail. Les outils secondaires (recherche, filtres, affichage, actualisation) sont masqués en projection ; l'heure de dernière actualisation, le portefeuille, la sortie de projection et le menu ⋯ restent accessibles.

Un clic sur un chantier le met en évidence. Un double-clic ou Entrée ouvre la fiche complète. Échap ferme les menus ou la fiche ; hors des panneaux, Échap quitte la projection. Les raccourcis clavier antérieurs restent disponibles : flèches gauche/droite pour les sujets, Page précédente/suivante pour les périmètres. Aucune notion de séance, de sujet abordé ou de bilan n'est réintroduite.

## Configuration Grist inchangée

Conserver la table source **Architecture** et l'autorisation **Lire la table sélectionnée** (`read table`). Rendre accessibles les colonnes suivantes dans les colonnes visibles du widget :

| Colonne | Rôle |
| --- | --- |
| Nom | Intitulé du chantier |
| Périmètre | Regroupement dans le portefeuille |
| Volet | Internes DT / Avec les autres directions |
| Phase | Fiche de détail et filtres |
| Urgence | Couleur de la barre |
| Responsable Projet | Fiche de détail et filtres |
| Commentaires | Fiche de détail ; résumé en projection |
| Date de début | Début |
| Précision début | Jour, Mois, Année |
| Date de fin | Fin |
| Précision fin | Jour, Mois, Année |
| Ordre | Ordre au sein d'un périmètre |
| Pôle (facultative) | Architecture, Lot 1, Lot 2, Lot 3… |

Les noms visibles accentués et les identifiants techniques usuels restent reconnus. La colonne Pôle est facultative : une valeur absente signifie Architecture. Aucun pôle n'est inféré d'un intitulé contenant « Lot 3 ».

Les pôles prévus sans données sont indiqués comme **Non alimenté**. Le widget ne lit que la table sélectionnée, pas d'autres tables de lots automatiquement.

## Règles et sécurité

La couleur représente l'urgence, pas la phase ou l'avancement. Les dates absentes ne sont pas inventées ; une date à l'année reste imprécise. La période affichée reste commune aux périmètres. Les données de Grist ne sont jamais écrites par le widget.

Le calendrier, les filtres, le portefeuille, l'export SVG et l'impression conservent leur fonctionnement. Hors Grist, le menu ⋯ permet de charger un CSV local ; son contenu n'est envoyé à aucun serveur et n'est pas intégré aux fichiers à publier.

## Vérifications de cette livraison

Vérifications réalisées avec Chromium et une API Grist simulée : navigation sur les 13 périmètres de l’export, respect des filtres, désactivation en fin de parcours ou sans résultat, mode projection, ouverture du portefeuille, réception de nouvelles données et absence d’erreur JavaScript. L’alignement et les débordements ont été contrôlés à 1920, 1440, 1366, 1280, 1152, 1024, 760 et 390 px. Syntaxe JavaScript vérifiée avec Node. Ces vérifications ne constituent pas un test dans le document Grist de production. Aucun enregistrement d’essai ni CSV métier n’est inclus dans l’archive à publier.
