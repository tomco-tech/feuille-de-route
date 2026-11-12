# Feuille de route architecture — version réunion 1.2

Widget Grist en lecture seule, à publier sur GitHub Pages. Le mode **Réunion** est la vue initiale ; le mode **Vue d’ensemble** conserve le planning complet. Le fichier `index.html` contient le HTML, les styles, le modèle de lecture et le JavaScript. Aucun outil de compilation n’est requis.

## Installation / mise à jour

1. Remplacer le précédent `index.html` par celui de ce dossier. Conserver `.nojekyll` et ce README dans le dépôt.
2. Publier le dossier par GitHub Pages comme la version précédente.
3. Dans Grist, conserver **Architecture** comme table source du widget personnalisé.
4. Autoriser **Lire la table sélectionnée / Read selected table** et transmettre les 12 colonnes ci-dessous dans les colonnes accessibles au widget. L’accès complet au document n’est pas demandé.
5. Ajouter temporairement `?v=1.2` à l’URL du widget pour distinguer cette publication de l’ancienne. Si l’URL comporte déjà un paramètre, ajouter `&v=1.2`.

Ne pas publier le CSV métier dans GitHub : il sert uniquement aux essais locaux. Aucune ligne métier n’est incluse dans le code livré. Les copies d’écran fournies séparément sont des aperçus réalisés à partir de l’export utilisateur, pas des données de démonstration embarquées.

## Utilisation en réunion

### Un périmètre à la fois

Le mode Réunion suit les volets et périmètres de la feuille de route, puis la colonne `Ordre` à l’intérieur de chaque périmètre. L’ordre des périmètres connus reprend la présentation initiale. Les nouveaux périmètres sont ajoutés après les périmètres connus de leur volet, dans l’ordre reçu de Grist.

Le titre affiche le périmètre présenté et sa position dans la revue. Les boutons de droite passent au périmètre précédent ou suivant. **Ordre du jour** permet de choisir directement un périmètre sans faire défiler tout le planning.

La roadmap conserve une période commune entre les périmètres, pour éviter de comparer à tort des barres dessinées à des échelles différentes. Sa zone utilise la hauteur disponible ; les lignes s’adaptent à cette hauteur avec une taille minimale lisible. Si un périmètre contient beaucoup de sujets, le défilement est interne au planning et le sujet choisi est ramené dans la zone visible.

### Passer d’un sujet au suivant

Un clic sur un nom ou une barre met le chantier en évidence et affiche une fiche synthétique sous le planning : nom complet, urgence, phase, responsable et dates. Les commentaires apparaissent lorsqu’ils existent ; ils peuvent être lus intégralement avec **Voir le détail**.

Les boutons **Précédent / Suivant** parcourent les sujets de la sélection courante. Après le dernier sujet d’un périmètre, « Suivant » passe au premier sujet du périmètre suivant. La navigation ne marque pas automatiquement un sujet comme abordé.

Raccourcis lorsque le widget a le focus et qu’aucun champ n’est en cours de saisie :

- **← / →** : sujet précédent / suivant.
- **Page précédente / Page suivante** : périmètre précédent / suivant.
- **Entrée** sur le planning ou un chantier : ouvrir le détail.
- **Échap** : fermer la fenêtre ou le panneau actif ; sortir du mode Projection quand aucun panneau n’est ouvert.

Les raccourcis ne s’appliquent pas pendant la saisie dans un champ de recherche. Le focus doit être dans le widget et non dans une autre section de Grist.

### Repères de séance, distincts de l’avancement métier

**Marquer abordé** ajoute un repère à un sujet explicitement discuté. Un deuxième clic retire le repère. Le compteur affiche le nombre de sujets abordés parmi ceux répondant aux filtres actuels. Les repères des sujets temporairement hors filtres restent en mémoire.

Le menu **⋯ → Bilan de la séance** récapitule les sujets abordés et ceux restant à parcourir. Le bilan est également accessible après le dernier sujet. Chaque sujet du bilan est cliquable pour reprendre la revue à cet endroit.

**Important :** « Abordé » ne veut pas dire « validé », « livré » ou « terminé ». Ces repères sont conservés uniquement en mémoire dans cette page, sans `localStorage`, sans table supplémentaire, sans écriture Grist. Ils disparaissent au rechargement de la page, lors d’un nouvel import CSV ou avec **⋯ → Nouvelle séance**. Ils ne constituent pas un compte rendu enregistré ou partagé.

### Projection et vue d’ensemble

**Projection** masque les filtres et outils secondaires et agrandit la typographie des lignes pour un partage d’écran. Ce bouton optimise l’espace dans le widget ; il ne force pas le navigateur ou Grist à passer en plein écran. **Quitter la projection** rétablit l’interface. Les boutons de navigation restent visibles.

**Vue d’ensemble** affiche tous les chantiers filtrés, groupés et repliables, pour retrouver une vision transversale. Cliquer sur un chantier ramène en mode Réunion sur ce sujet. Les filtres, repères de séance et réglages de période sont conservés lors du changement de mode.

## Modèle lu

| Colonne visible | Identifiant reconnu |
|---|---|
| Nom | `Nom` |
| Périmètre | `Perimetre` |
| Volet | `Volet` |
| Phase | `Phase` |
| Urgence | `Urgence` |
| Responsable Projet | `Responsable_Projet` |
| Commentaires | `Commentaires` |
| Date de début | `Date_de_debut` |
| Précision début | `Precision_debut` |
| Date de fin | `Date_de_fin` |
| Précision fin | `Precision_fin` |
| Ordre | `Ordre` |

Les libellés et identifiants sont reconnus en ignorant accents, casse et séparateurs. L’API Grist est chargée depuis `https://docs.getgrist.com/grist-plugin-api.js`. L’abonnement est réalisé avec `grist.onRecords`, avec `keepEncoded:true`, `expandRefs:true`, `includeColumns:'shown'`. La lecture se limite aux lignes et colonnes que Grist transmet à cette vue. Les restrictions et filtres Grist continuent de s’appliquer.

## Lecture des données et dates

Les règles de la version épurée sont conservées :

- La couleur de la barre correspond à **Urgence**, jamais à un pourcentage d’avancement.
- Mineur : bleu clair ; Intermédiaire : bleu moyen ; Prioritaire : bleu foncé ; À qualifier ou valeur non reconnue : gris.
- Les dates au jour, au mois et à l’année conservent leur précision. L’année seule est représentée avec des hachures, pas transformée en un engagement à une date précise.
- Les dates Grist encodées et les valeurs `MM/AAAA`, `AAAA`, `JJ/MM/AAAA` et ISO sont reconnues.
- Une date absente n’est pas inventée. Un début seul a une extension courte en pointillés, qui ne constitue pas une durée prévue. Sans début ni fin, la ligne indique « Calendrier à préciser ».
- Une fin antérieure au début ou une date invalide reste signalée.
- La date du repère est la date du navigateur, modifiable dans **Affichage**. Aucun retard ou achèvement n’est déduit de cette date.
- La phase et les commentaires sont repris des données, sans création automatique de décisions, d’actions ou de statuts de réunion.

## Réglages secondaires et export

**Filtres** : périmètre, urgence, phase, responsable, et fins à préciser. Recherche par nom ou mot-clé disponible dans le bandeau hors mode Projection.

**Affichage** : période, échelle, repère vertical, détails des lignes ; durées consolidées et pliage des périmètres en Vue d’ensemble. Les fenêtres flottent au-dessus du planning sans réduire sa surface permanente.

**⋯ → Exporter la vue en SVG** : export du périmètre présenté en mode Réunion, ou de tous les chantiers filtrés en Vue d’ensemble. L’export contient les données métiers visibles ; ne pas le publier dans un dépôt public. **Imprimer** utilise le rendu d’impression du navigateur.

Hors Grist, le menu permet d’ouvrir un export CSV local. Le fichier est lu dans le navigateur et ne fait l’objet d’aucun envoi par le widget.

## Vérifications réalisées

Tests navigateur automatisés sur l’export Architecture fourni (39 lignes, 13 périmètres), avec une API Grist simulée : navigation complète et clavier, changement de périmètre, ordre du jour, filtres et résultat vide, retour vue d’ensemble, projection, repères réversibles, actualisation sans perte de sélection, bilan, import CSV local, dates encodées et précision annuelle, export SVG valide.

Rendus contrôlés en 1920×1080, 1440×900, 1366×768, 1024×768, 768×1024 et 390×844. Le téléphone utilise un défilement horizontal interne au calendrier afin de ne pas rendre les noms et dates illisibles.

Ces tests ne remplacent pas une vérification dans le document Grist réel. Aucun accès au Grist de production n’a été utilisé. Après publication, vérifier la réception des données, un changement de périmètre, l’ouverture d’un détail et le mode Projection.

Références API :
- https://support.getgrist.com/widget-custom/
- https://support.getgrist.com/code/interfaces/grist_plugin_api.FetchSelectedOptions/
