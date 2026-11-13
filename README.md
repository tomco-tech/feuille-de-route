# Feuille de route DT — V1.6

Widget Grist en lecture seule, à publier sur GitHub Pages. Cette version reprend la V1.5 et ajuste la navigation, les couleurs et les commandes, sans changer le modèle de données.

## Fichiers à publier

- `index.html` : code complet du widget (HTML, CSS, JavaScript).
- `.nojekyll` : fichier vide à conserver.
- `README.md` : documentation ; aucune donnée métier n'est embarquée dans le code.

Remplacer `index.html` dans le même dossier GitHub Pages. Aucune compilation ni nouvelle dépendance n'est nécessaire. Dans l'URL du widget, remplacer le paramètre de version précédent par `?v=1.6` (ou `&v=1.6` si un autre paramètre existe déjà).

## Modifications V1.6

### Navigation dans les deux sens

Les boutons **← Périmètre précédent** et **Périmètre suivant →** sont regroupés à droite du titre, en mode normal et en projection. Sur un écran étroit, ils passent sous le titre.

Ils parcourent les périmètres dans l'ordre du portefeuille, en tenant compte des filtres. Le premier chantier du périmètre de destination est sélectionné et le défilement vertical revient en haut. La période et l'échelle du calendrier sont conservées. Le bouton précédent est désactivé au premier périmètre ; le suivant l'est au dernier. Les deux sont désactivés pendant le chargement ou sans périmètre disponible. Le parcours ne boucle pas.

### Couleurs des urgences

La palette sauge / ambre / mauve est remplacée par des couleurs plus franches, sans effet fluorescent :

| Urgence | Couleur | Valeur |
| --- | --- | --- |
| Mineur | Bleu | `#478FC5` |
| Intermédiaire | Orange | `#D26E32` |
| Prioritaire | Framboise | `#C55C78` |
| À qualifier / non renseignée | Gris bleuté | `#7E8FA3` |

Ces couleurs sont communes à la légende, aux barres du calendrier, aux pastilles du portefeuille et des détails, ainsi qu'à l'export SVG. Les libellés et les hachures restent présents.

### Menu unique

Un seul bouton **Menu** rassemble :

- **Affichage** : échelle, période, repère vertical, détails des lignes et informations à préciser.
- **Filtres** : pôle, volet, périmètre, urgence, phase, responsable et fins à préciser.
- **Effacer tous les filtres**, lorsqu'un filtre ou une recherche est actif.
- **Actualiser les données**.
- **Exporter le périmètre en SVG** et **Imprimer**.
- **Ouvrir un CSV local**, dans l'aperçu hors Grist.

Une flèche de retour permet de revenir au menu depuis Affichage ou Filtres. Le nombre de filtres actifs apparaît sur le bouton Menu. Échap ou le bouton de fermeture referme le panneau et rend le focus au bouton Menu. Les flèches haut/bas et les touches Début/Fin parcourent les actions du menu.

Le bouton **Portefeuille des chantiers**, la recherche et la commande **Projection** restent directement accessibles dans le bandeau supérieur. En projection, la recherche est masquée ; le portefeuille, le menu commun, la sortie de projection et l'heure d'actualisation restent accessibles.

### Bandeau de détail en projection

Les dates de début et de fin sont retirées du bandeau inférieur du chantier sélectionné. Il conserve le titre, l'urgence, la phase, le responsable et le commentaire, selon la place disponible. Les dates restent consultables dans le calendrier et dans la fiche complète du chantier.

## Navigation conservée

Le portefeuille reste organisé en **Pôle → Volet → Périmètre → Chantier**. Cliquer sur un périmètre affiche sa roadmap ; son chevron déplie les chantiers.

Un clic sur un chantier le met en évidence. Un double-clic ou Entrée ouvre la fiche complète. Échap ferme les menus ou la fiche ; hors des panneaux, Échap quitte la projection. Les flèches gauche/droite parcourent les sujets ; les touches Page précédente/suivante parcourent les périmètres. Aucune notion de séance, de sujet abordé ou de bilan n'est réintroduite.

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

Hors Grist, **Menu → Ouvrir un CSV local** permet de prévisualiser un export. Son contenu n'est envoyé à aucun serveur et n'est pas intégré aux fichiers à publier.

## Vérifications de cette livraison

La syntaxe des deux scripts JavaScript et la structure HTML ont été vérifiées. Les contrôles de logique avec des données fictives portent sur la navigation, les filtres, les états du menu et les dates du bandeau de projection. Aucun jeu de données de test n'est inclus dans les fichiers à publier.

Le rendu visuel dans un navigateur et l'intégration dans le document Grist de production n'ont pas pu être validés dans l'environnement de cette livraison. Ils restent à confirmer après remplacement du fichier.
