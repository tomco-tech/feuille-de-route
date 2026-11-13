# Feuille de route DT — V1.8

Widget **Grist en lecture seule**, destiné à être hébergé sur **GitHub Pages**.
Cette version conserve la navigation par périmètre, la recherche, le menu unique et les couleurs d’urgence de la V1.7. Elle supprime intégralement le mode projection et simplifie la consultation des chantiers.

## Publication

Publier les trois fichiers du dossier à la racine du site GitHub Pages :

- `index.html` : widget complet (HTML, CSS et JavaScript, sans compilation).
- `.nojekyll` : fichier vide à conserver.
- `README.md` : documentation.

Dans GitHub, remplacer le `index.html` existant par celui de cette archive ; ne pas modifier les données Grist. Pour forcer la mise à jour du cache, utiliser `?v=1.8` dans l’URL du widget, ou `&v=1.8` si l’URL possède déjà des paramètres.

## Changements V1.8

1. **Fiche chantier plus lisible** : la mention **« Ordre dans le périmètre »** n’est plus affichée. Le champ source `Ordre` reste lu par le widget pour trier les chantiers dans leur périmètre.
2. **Projection supprimée complètement** : plus de bouton Projection, d’état de projection, de bandeau inférieur dédié, ni de raccourci de sortie de projection.
3. **Portefeuille entièrement replié à l’ouverture** : les pôles et les périmètres démarrent fermés, y compris celui de la sélection courante. L’utilisateur peut les déplier, mais à chaque nouvelle ouverture du portefeuille tout est de nouveau replié.
4. **Fiche accessible au clic simple** : cliquer n’importe où sur une ligne de chantier (son nom, sa barre du calendrier ou une zone vide de sa ligne) affiche sa fiche. Le double-clic n’est plus requis ni associé à une action distincte. Un court délai de protection empêche le second clic d’un double-clic de fermer immédiatement la fenêtre par son fond. La fenêtre peut toujours être fermée par **×**, **Échap** ou par un clic sur le fond après ce délai.

## Interface conservée des V1.6 et V1.7

- Barre supérieure, de gauche à droite : **Recherche → Portefeuille des chantiers → Menu**.
- Menu unique : **Affichage**, **Filtres**, **Actualiser les données**, **Exporter le périmètre en SVG**, **Imprimer** et, dans l’aperçu autonome, **Ouvrir un CSV local**. Les sous-menus comportent une flèche de retour.
- Navigation dans les périmètres : bouton **←** (retour) et bouton **Périmètre suivant →** ; les boutons sont désactivés aux extrémités et respectent les filtres actifs.
- Couleurs d’urgence : **bleu** pour Mineur (`#478FC5`), **jaune translucide** pour Intermédiaire (`rgba(231,181,41,.70)`, 30 % transparent), **framboise translucide** pour Prioritaire (`rgba(197,92,120,.65)`, 35 % transparent), **gris bleuté** pour À qualifier (`#7E8FA3`). Les mêmes couleurs sont utilisées dans le calendrier, la légende et l’export SVG.
- Hachures pour les dates à la précision de l’année, pointillés lorsque l’une des dates est à préciser ; la couleur décrit l’urgence et non l’avancement.

## Utilisation

Cliquer sur une ligne de la feuille de route ouvre directement le détail. Le portefeuille est organisé en **Pôle → Volet → Périmètre → Chantier**. Déplier un pôle pour voir ses volets et périmètres, puis un périmètre pour voir ses chantiers. Cliquer sur le nom du périmètre affiche son calendrier.

Les raccourcis clavier **flèche gauche/droite** (chantier précédent/suivant), **Page précédente/suivante** (périmètre précédent/suivant), **Entrée** et **Échap** restent disponibles lorsque le focus est hors d’un champ de saisie ou d’un panneau. Pour les chantiers, **Entrée** ou **Espace** ouvrent également la fiche depuis un élément accessible au clavier.

## Table Grist et permissions

Sélectionner la table **Architecture** et autoriser **« Lire la table sélectionnée »** (`read table`). Rendre visibles les colonnes suivantes dans les options du widget :

| Colonne | Usage |
| --- | --- |
| Nom | Nom du chantier |
| Périmètre | Regroupement et navigation |
| Volet | Regroupement |
| Phase | Fiche et filtre |
| Urgence | Code couleur |
| Responsable Projet | Fiche et filtre |
| Commentaires | Fiche de détail |
| Date de début | Début du calendrier |
| Précision début | Jour, Mois ou Année |
| Date de fin | Fin du calendrier |
| Précision fin | Jour, Mois ou Année |
| Ordre | Classement interne, **non affiché dans la fiche** |
| Pôle (facultative) | Architecture, Lot 1, Lot 2, Lot 3… |

Les variantes usuelles d’intitulés/identifiants techniques sont reconnues. Si la colonne Pôle est absente, le pôle `Architecture` est utilisé. Les pôles futurs sans données sont indiqués comme « Non alimenté ». Les données Grist ne sont jamais modifiées par le widget.

## Vérifications

- **Syntaxe JavaScript :** les deux scripts passent `node --check`.
- **CSS :** analyse sans erreur.
- **Tests Chromium avec données Grist simulées :** 75 vérifications réussies aux tailles **1440 × 850**, **760 × 800** et **390 × 800**. Elles couvrent les menus et leurs retours, la suppression de la projection, la navigation par périmètre, le portefeuille replié à chaque ouverture, l’ouverture au clic sur le nom ou une zone de la ligne, le double-clic non fermant, la fermeture par Échap, l’absence du champ Ordre dans la fiche, la recherche et l’absence d’erreur JavaScript.

Ces vérifications ne remplacent pas un test dans le document Grist réel. Aucun chantier réel, CSV métier ou identifiant sensible n’est distribué dans cette archive.
