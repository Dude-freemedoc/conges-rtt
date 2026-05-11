# Gestion Congés / RTT

Application web légère (HTML/CSS/JS, sans framework, sans build) pour la gestion
collaborative des congés et RTT des équipes Support N1 et N2 sur la période
**Juin 2026 → Mai 2027**.

L'intégralité de l'application tient dans un seul fichier `index.html`
(styles et scripts inlines). Aucune dépendance npm, aucun outil de build.

---

## Fonctionnalités

- Planning mensuel par collaborateur (lignes) × jours du mois (colonnes)
- Sélection en 2 étapes : **durée** (Journée / Matinée / Après-midi) puis **type** (CP, RTT, FR, CSS, FAM, PM, MAL, DIV, RC, plus une variante « provisoire » pour CP et RTT)
- Demi-journées affichées sous forme de **cellule divisée** (code en haut pour la matinée, en bas pour l'après-midi) — décompte 0.5
- Totaux mensuels par collaborateur et lignes **Présents** par équipe + global
- Récap annuel avec soldes initiaux (CP, RTT, FR), reliquats, soldes restants
- Gestion des collaborateurs (ajout / modification / suppression)
- Export CSV du mois courant
- Jours fériés français de la période identifiés et non modifiables
- Samedis / dimanches grisés et non modifiables

## Lancement

Ouvrir `index.html` dans un navigateur moderne (Chrome, Edge, Firefox).

## Déploiement

L'application est hébergée sur **GitHub Pages** :
- Repo : `dude-freemedoc/conges-rtt`
- URL : <https://dude-freemedoc.github.io/conges-rtt/>

Pour déployer une nouvelle version, pousser le fichier `index.html` modifié
sur la branche `main`.

---

## Mode collaboratif (JSONbin.io)

Les données sont partagées entre tous les utilisateurs via l'API publique
[JSONbin.io](https://jsonbin.io) (plan gratuit). La configuration est
embarquée directement dans `index.html` :

```js
const JSONBIN_BIN_ID     = "69f9ec02856a682189abd3e2";
const JSONBIN_ACCESS_KEY = "$2a$10$ZBhZnz4ly/b06u2KC8ly3uJalID..HIv8ac/qqwj7/4PnTzm1dfjS";
```

- `JSONBIN_BIN_ID` : identifiant du bin partagé (l'« espace de stockage »)
- `JSONBIN_ACCESS_KEY` : clé en lecture seule, intégrée dans le code et
  visible dans le navigateur — elle ne donne **que l'accès en lecture**

### Éditeur vs Lecteur

Le mode **éditeur** est activé par la présence d'une clé maître passée en
paramètre d'URL :

```
https://dude-freemedoc.github.io/conges-rtt/?key=LA_CLE_MAITRE_JSONBIN
```

- **Éditeurs** (avec `?key=...`) : peuvent modifier le planning. La clé est
  conservée dans le `localStorage` du navigateur, pas besoin de la remettre
  ensuite. Statut affiché : `Mode collaboratif actif (ÉDITEUR)`.
- **Lecteurs** (sans clé) : consultation seule. Tous les boutons de
  modification sont désactivés. Statut : `Mode collaboratif actif (LECTEUR)`.

### Synchronisation

- Au chargement : affichage immédiat de la dernière version connue (cache
  `localStorage`), puis rafraîchissement avec les données distantes
- Sync automatique toutes les **10 secondes** + au retour sur l'onglet
- Les modifications d'un éditeur sont visibles par tous les lecteurs sous
  10 secondes

### Repli local

Si JSONbin est indisponible (réseau coupé, quota dépassé…), l'application
bascule automatiquement sur du stockage `localStorage` purement local.

---

## Codes d'absence

| Code | Libellé              | Couleur  |
| ---- | -------------------- | -------- |
| CP   | Congé payé           | Vert     |
| CP   | CP provisoire        | Orange   |
| RTT  | RTT                  | Vert     |
| RTT  | RTT provisoire       | Orange   |
| FR   | Fractionnement       | Bleu     |
| CSS  | Congé sans solde     | Vert     |
| FAM  | Évènement familial   | Violet   |
| PM   | Paternité / Maternité| Turquoise|
| MAL  | Maladie              | Rouge    |
| DIV  | Divers               | Gris     |
| RC   | Récupération         | Vert     |

Les variantes demi-journée et provisoire sont gérées en interne mais ne
s'affichent pas comme codes distincts :

- **Demi-journée** : la cellule est visuellement divisée en deux, le code
  apparaît en haut (matinée) ou en bas (après-midi).
- **Provisoire** : le code reste « CP » ou « RTT », seule la couleur orange
  (au lieu du vert) signale le caractère provisoire.

## Notes

- Les CP / RTT provisoires sont décomptés comme des CP / RTT classiques dans
  les totaux ; la couleur orange permet uniquement de les distinguer
  visuellement.
- Les soldes initiaux sont uniquement modifiables depuis le récap annuel.
- L'export CSV reprend l'état du mois actuellement sélectionné.
