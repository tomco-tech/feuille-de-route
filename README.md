# Feuille de route DT — V1.4

Widget Grist en lecture seule, à publier sur GitHub Pages. Cette version reprend la V1.3 et simplifie son interface sans changer le modèle de données.

## Fichiers à publier

- `index.html` : code complet du widget (HTML, CSS, JavaScript).
- `.nojekyll` : fichier vide à conserver.
- `README.md` : documentation ; aucune donnée métier n'est embarquée dans le code.

Remplacer `index.html` dans le même dossier GitHub Pages. Aucune compilation ni nouveau fichier JavaScript n'est nécessaire. Dans l'URL du widget, remplacer le paramètre de version précédent par `?v=1.4` (ou `&v=1.4` si un autre paramètre existe déjà).

## Modifications V1.4

- Suppression des boutons **Sujet suivant**, **Retour**, **Détail** et **Voir le détail**, en vue normale comme en projection.
- En-tête unique : recherche, Filtres, Affichage, Projection, Actualiser, menu ⋯ et Portefeuille sont alignés sur la même ligne et ont une hauteur de 34 px sur les écrans bureau. Sur un petit écran, les commandes passent à la ligne plutôt que d'être tronquées.
- Bouton **Portefeuille des chantiers** à droite, sur un fond lavande discret.
- **Actualisé à HH:MM** à gauche, à côté du titre. L'heure correspond à la réception des données ; elle n'est pas modifiée par une simple sélection, un filtre ou un changement d'affichage. En aperçu CSV, le survol précise l'origine locale des données.
- Nouvelles couleurs d'urgence, communes au calendrier, aux pastilles, à la légende et à l'export SVG :
  - Mineur : vert sauge `#A6BFAE`.
  - Intermédiaire : ambre doux `#D7B67A`.
  - Prioritaire : mauve discret `#AB98BC`.
  - À qualifier : gris `#BDC5CE`.
- Définition visible dans la légende : **Hachures : date connue à l’année**. Une hachure signale une précision annuelle du début ou de la fin, pas un état d'avancement.
- Les lignes restent stables lors de la sélection, pour que le double-clic sur un chantier ouvre correctement sa fiche sans bouton Détail.

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

Tests locaux avec Chromium et une API Grist simulée, sur les 39 chantiers de l'export : chargement, alignement des commandes et absence de débordement à 1920, 1440, 1366, 1280, 1152, 1024, 820, 760 et 390 px ; portefeuille et regroupements ; filtres ; recherche ; projection ; double-clic et fermeture du détail ; actualisation ; notification de nouvelles données ; export SVG et nouvelle palette. Contrôle de la syntaxe JavaScript avec Node.

Ces vérifications ne constituent pas un test dans le document Grist de production. Aucun enregistrement d'essai, CSV ou capture métier n'est inclus dans l'archive à publier.
