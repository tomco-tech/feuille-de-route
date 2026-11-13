# Feuille de route DT — V1.7

Widget Grist en lecture seule, à publier sur GitHub Pages. Cette version reprend la V1.6 et ajuste les couleurs, l'ordre des commandes et le bouton de retour, sans changer le modèle de données.

## Fichiers à publier

- `index.html` : code complet du widget (HTML, CSS, JavaScript).
- `.nojekyll` : fichier vide à conserver.
- `README.md` : documentation ; aucune donnée métier n'est embarquée dans le code.

Remplacer `index.html` dans le même dossier GitHub Pages. Aucune compilation ni nouvelle dépendance n'est nécessaire. Dans l'URL du widget, remplacer le paramètre de version précédent par `?v=1.7` (ou `&v=1.7` si un autre paramètre existe déjà).

## Interface V1.7

### Navigation dans les deux sens

Les boutons **←** et **Périmètre suivant →** sont regroupés à droite du titre, en mode normal et en projection. Le retour affiche uniquement une flèche ; son libellé accessible et son infobulle indiquent le périmètre précédent. Sur un écran étroit, les boutons passent sous le titre.

Ils parcourent les périmètres dans l'ordre du portefeuille, en tenant compte des filtres. Le premier chantier du périmètre de destination est sélectionné et le défilement vertical revient en haut. La période et l'échelle du calendrier sont conservées. Le bouton précédent est désactivé au premier périmètre ; le suivant l'est au dernier. Les deux sont désactivés pendant le chargement ou sans périmètre disponible. Le parcours ne boucle pas.

### Couleurs des urgences

L'orange devient un jaune doux, avec 70 % d'opacité (30 % de transparence). Le framboise conserve sa teinte RVB d'origine et s'éclaircit grâce à une opacité de 65 % (35 % de transparence). Le bleu et le gris restent inchangés.

| Urgence | Couleur | Valeur |
| --- | --- | --- |
| Mineur | Bleu | `#478FC5` |
| Intermédiaire | Jaune doux | `rgba(228, 201, 56, 0.70)` |
| Prioritaire | Framboise éclairci | `rgba(197, 92, 120, 0.65)` |
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

Le bandeau supérieur suit l'ordre **Projection → Recherche → Portefeuille des chantiers → Menu**, à l'écran comme au clavier. En projection, la recherche est masquée ; la sortie de projection, le portefeuille, le menu commun et l'heure d'actualisation restent accessibles.

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

Les vérifications portent sur la syntaxe des deux scripts JavaScript et sur le code source : couleurs, ordre des commandes, bouton de retour et conservation du menu unique et du bandeau de projection sans dates. Aucun jeu de données de test n'est inclus dans les fichiers à publier.

Le rendu visuel dans un navigateur et l'intégration dans le document Grist de production n'ont pas pu être validés dans l'environnement de cette livraison. Ils restent à confirmer après remplacement du fichier.
