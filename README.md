# Pilotage DT — Feuille de route pôle architecture

Widget Grist en lecture seule, fondé sur la table **Architecture**.
Version 1.0 — 12 novembre 2026.

## Fichiers à publier

- `index.html` : code complet HTML, CSS et JavaScript, sans compilation.
- `.nojekyll` : désactive le traitement Jekyll de GitHub Pages.
- `README.md` : ce guide.

**Ne publiez pas les exports CSV ni le PDF métier dans le dépôt.** Le code n'embarque aucun chantier, responsable, commentaire ou planning opérationnel. Les intitulés de périmètre utilisés pour leur ordre d'affichage sont configurés dans `SCOPE_ORDER` ; les lignes métier sont toujours lues depuis Grist.

## Installation sur GitHub Pages

1. Créer un dépôt ou un dossier dédié à ce nouveau widget, pour ne pas remplacer les dashboards existants.
2. Placer les trois fichiers à la racine de l'emplacement à publier.
3. Dans GitHub : **Settings → Pages → Deploy from a branch**, choisir la branche publiée, généralement `main`, et le répertoire approprié.
4. Utiliser l'URL de la page web publiée, et non l'URL du fichier sur github.com. Exemple de forme :

   ```text
   https://VOTRE_COMPTE.github.io/VOTRE_DEPOT/?v=1.0
   ```

## Installation dans Grist

1. Ajouter un widget **Personnalisé / Custom**.
2. Sélectionner la table **Architecture** comme source. Éviter une table de synthèse ou un lien de sélection qui réduirait involontairement le nombre de lignes.
3. Renseigner l'URL GitHub Pages.
4. Accorder **Lire la table sélectionnée / Read selected table**. **L'accès complet au document n'est pas nécessaire.**
5. Dans les paramètres des colonnes du widget, rendre **les 12 colonnes ci-dessous visibles/accessibles**. Elles ne sont pas toutes affichées à l'écran, mais le code en a besoin pour dessiner les barres et les détails.

Le widget utilise les identifiants des colonnes et leurs alias, sans imposer un type de colonne et sans écran de configuration métier. Il n'exige pas de mapping supplémentaire quand les noms correspondent au modèle fourni.

| Libellé de l'export | Identifiant reconnu |
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

Les noms visibles avec accents et espaces sont également reconnus. La comparaison des noms de colonnes ignore la casse, les accents et la ponctuation. Si un identifiant a été renommé avec un nom différent ou un suffixe, compléter son alias dans `RoadmapCore.FIELDS` au début du JavaScript.

Les valeurs de références sont demandées à Grist avec `expandRefs: true` : on lit leur valeur d'affichage, pas un numéro de ligne. Textes, choix et listes de choix sont acceptés. Une référence non développée ou une erreur de formule est signalée, pas interprétée comme un nom ou une urgence valide.

## Organisation de la feuille de route

Le modèle de présentation reprend le PDF **FeuilleRouteArchi_09102026(1).pdf**, pages 2 à 4 :

- premier niveau : `Volet` — Internes DT / Avec les autres directions ;
- deuxième niveau : `Périmètre` ;
- une ligne par chantier `Nom` ;
- tri par `Ordre` à l'intérieur du périmètre ;
- couleurs des barres fondées sur `Urgence` ;
- repère vertical de date.

L'ordre des 13 périmètres de la présentation est conservé grâce à `SCOPE_ORDER`. Les nouveaux périmètres sont ajoutés après les périmètres connus. Les nouveaux volets restent visibles.

Les 39 lignes du CSV de validation sont réparties entre 25 chantiers internes DT et 14 chantiers avec les autres directions. Ces chiffres ne sont pas codés dans le widget.

### Source de vérité

**Architecture est la source des valeurs**, le PDF est la référence de présentation.

- La phase n'est pas remplacée par un statut inventé.
- Une date de fin passée ne signifie pas automatiquement que le chantier est terminé ou en retard.
- L'urgence « À qualifier » reste neutre, même si le PDF utilise une autre couleur.
- Un calendrier vide reste « Calendrier à préciser », même si une barre existe dans le PDF.
- Aucun pourcentage d'avancement n'est calculé à partir du temps écoulé.

## Dates et précision

Formats supportés :

- dates Grist et DateTime, horodatages Unix et valeurs encodées `['d', secondes]` / `['D', secondes, ...]` ;
- textes `JJ/MM/AAAA`, `MM/AAAA`, `AAAA`, `AAAA-MM`, `AAAA-MM-JJ` et ISO avec heure ;
- espaces ordinaires ou insécables autour des valeurs ;
- `?`, `tbd`, valeurs vides : date inconnue.

**Un texte JJ/MM sans année n'est pas complété arbitrairement.** Il est signalé comme date à vérifier.

`Précision début` et `Précision fin` sont pris en compte :

- **Jour** : date exacte ;
- **Mois** : le mois est utilisé comme intervalle graphique, sans afficher un jour fictif ;
- **Année** : l'année est représentée avec des hachures pour signaler l'imprécision. La fin de l'année n'est pas présentée comme une échéance validée.

Si la valeur source ne contient qu'une année, la précision ne peut pas être artificiellement affinée en mois ou en jour.

Les mois de fin sont inclus dans l'affichage de la barre. Une fin antérieure au début entraîne « Dates à vérifier » ; le widget n'inverse pas les dates.

### Dates manquantes

- Début connu, fin absente : marqueur de début, petit trait pointillé indicatif et « fin à préciser ». Ce trait **ne représente pas une durée estimée**.
- Fin connue, début absent : marqueur de fin et « Début à préciser ».
- Aucune date : le chantier reste visible, sans barre de durée.

La période initiale couvre les années connues de toute la table (2025 à 2028 dans l'export fourni). Modifier un filtre ne change pas automatiquement cette échelle. Les dates de période, le bouton ↔ et les modes d'échelle permettent de changer la fenêtre affichée.

Le repère vertical est la date du jour à l'ouverture, calculée dans le navigateur. Le champ **Repère** peut être placé au **08/10/2026** pour comparer à la présentation jointe. Il n'est pas figé sur cette date historique.

### Durées consolidées

Option désactivée par défaut. Les pointillés des en-têtes représentent l'enveloppe **calculée à partir des dates connues des chantiers visibles** : premier début et dernière fin connus. Un `?` signale que certaines dates manquent. Ce n'est ni un engagement de livraison supplémentaire, ni une colonne à créer dans Grist.

## Interactions

- Switchs Tous les chantiers / Internes DT / Autres directions.
- Recherche textuelle insensible aux accents et à la casse, couvrant nom, périmètre, phase, responsable et commentaires.
- Recherches avec suggestions pour Périmètre, Phase, Urgence et Responsable projet. Les options se croisent avec les autres filtres. Flèches, Entrée et Échap sont disponibles.
- Compteurs : chantiers, périmètres, prioritaires, fins à préciser. Les deux derniers sont cliquables.
- Groupes repliables et boutons Tout déplier / Tout replier.
- Vue d'ensemble, échelle par mois, échelle par trimestres, période personnalisée.
- Clic sur un libellé ou une barre : fiche du chantier en lecture seule. Fermeture par ×, clic extérieur ou Échap.
- En-têtes et libellés fixés pendant le défilement du planning. Sur petit écran, le calendrier défile horizontalement.
- Les données se mettent à jour à réception de `grist.onRecords` ; Actualiser force une relecture.
- Aucune API de modification de données n'est utilisée.

## Export

**Exporter SVG** enregistre les chantiers correspondant aux filtres, tous les groupes dépliés, pour la période affichée. Le fichier inclut le repère et les dates connues, et peut être inséré dans une présentation. L'export contient naturellement les données visibles : le diffuser avec les précautions habituelles.

**Imprimer** prépare une version paysage. Choisir A3 paysage et l'option d'impression des arrière-plans/couleurs dans le navigateur. Le rendu exact dépend des réglages du navigateur ; l'export SVG est préférable pour une reprise dans une diapositive.

## Aperçu hors Grist

À l'ouverture de `index.html` en dehors d'une iframe, le widget propose **Ouvrir un export Architecture (.csv)**. La lecture se fait localement, sans upload et sans modification du CSV. Les CSV à virgules ou points-virgules, champs entre guillemets et commentaires multilignes sont acceptés.

Aucun CSV d'exemple contenant les données du projet n'est inclus dans ce dépôt. L'aperçu CSV est clairement signalé et ne se présente pas comme une connexion Grist en direct.

## Confidentialité et dépendances

- Aucun token/API key à mettre dans GitHub.
- Aucun envoi des données métier vers GitHub, un service graphique ou un serveur tiers par le code.
- Pas de bibliothèque de graphique, pas de police distante, pas de télémétrie.
- Seul script externe chargé dans Grist : `https://docs.getgrist.com/grist-plugin-api.js`.
- Aucun stockage des données métier dans localStorage ou dans des cookies.
- Les valeurs métier sont échappées avant affichage HTML/SVG.
- L'autorisation minimale requise est la lecture de la table sélectionnée. Comme pour tout widget personnalisé, le propriétaire du code doit être de confiance.

## Contrôles effectués avant livraison

Tests sur l'export Architecture fourni et dans Chromium avec l'API Grist simulée :

- 39 chantiers / 13 périmètres / 2 volets ;
- 9 urgences Prioritaire ; 6 fins manquantes ;
- dates françaises, années seules, dates Grist encodées, références et listes développées ;
- recherche, filtres croisés, volets, réinitialisation et repli des groupes ;
- panneau de détail et fermeture clavier ;
- export SVG avec contrôle XML ;
- rendu sur des largeurs de 1600, 1366, 1024, 768 et 390 pixels ;
- absence d'erreur JavaScript pendant ces tests ;
- mise à jour après notification `onRecords` et relecture manuelle.

Le widget n'a pas été connecté au document Grist de production de l'utilisateur. Une vérification d'intégration est donc à faire dans ce document après publication, notamment des colonnes visibles, des permissions et des filtres Grist amont.

## Références techniques

- Widgets personnalisés et niveaux d'accès : https://support.getgrist.com/widget-custom/
- API `onRecords`, `ready`, `fetchSelectedTable` : https://support.getgrist.com/code/modules/grist_plugin_api/
- Formats des données et développement des références : https://support.getgrist.com/code/interfaces/grist_plugin_api.FetchSelectedOptions/
