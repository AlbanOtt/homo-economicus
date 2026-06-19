---
name: homo-economicus
description: Analyse le coût total de possession d'un achat sur tout son cycle de vie (achat + énergie + consommables + maintenance + décote) et produit un rapport HTML cliquable classant jusqu'à 20 modèles. Utilise ce skill dès que l'utilisateur envisage un achat — comparaison de prix, « vaut le coup », meilleur rapport qualité-prix, TCO, choix optimal, « quel X choisir », « X ou Y », longévité, durée de vie. À déclencher proactivement aussi sur les évocations indirectes : « je dois changer mon lave-linge », « je pense à une voiture d'occasion », « j'hésite entre ces imprimantes ». Couvre électroménager, électronique, véhicules, outils, mobilier, et abonnements à coût récurrent.
---

# Homo Economicus

Aide l'utilisateur à prendre une décision d'achat rationnelle sur le long terme en calculant le coût total de possession (TCO) **dans le contexte de vie réel de l'acheteur**, puis en produisant un rapport HTML clair et partageable.

## Principes fondateurs

1. **Le contexte personnel est la fondation, pas un détail.** Le « meilleur » produit dépend de l'horizon de vie de l'acheteur. Un célibataire de 22 ans qui peut emménager en couple dans 3 ans n'optimise pas comme un retraité, ni comme une famille stable. **Toujours** collecter ce contexte avant la recherche produit.

2. **Ne demander que ce qui change la réponse de ≥ 5 à 10%.** Si une question n'a pas le potentiel de modifier le classement final ou le TCO d'au moins ~5%, ne pas la poser. L'utilisateur n'a pas le temps de répondre à 15 questions.

3. **Être honnête sur les zones d'ombre.** Quand les données de fiabilité ou de durabilité manquent, le dire explicitement dans un callout du rapport et expliquer comment ça affecte la confiance dans la recommandation.

4. **Toujours présenter des scénarios.** Ne pas ancrer sur un unique cas « probable ». Montrer 2-3 trajectoires plausibles dérivées du contexte personnel (ex : « tu gardes 10 ans » vs « tu revends à 4 ans »).

5. **Mode B est le cas le plus fréquent.** L'utilisateur décrit un besoin, le skill cherche les candidats. Mais si une liste de modèles est fournie, l'analyser sans recherche supplémentaire.

---

## Workflow

### Phase 1 — Contexte personnel (la fondation)

**Avant toute recherche produit**, collecter les informations qui définiront les scénarios. Lire `references/personal-context.md` pour la méthodologie de dérivation des scénarios.

Utiliser `ask_user_input_v0` pour poser ces questions en une seule fois (au max 3 questions, options tappables). Adapter les questions au contexte déjà connu — si l'utilisateur a déjà mentionné son âge ou sa situation, ne pas redemander.

Variables typiques à couvrir :
- **Tranche d'âge** : ≤30 / 31–50 / 51–70 / 70+
- **Situation de foyer** : étudiant·e / célibataire / en couple sans enfants / famille avec enfants / retraité·e
- **Stabilité prévisible sur 5–10 ans** : stable / changement probable (déménagement, regroupement, séparation, retraite) / très incertain

Géographie et devise : utiliser le contexte déjà disponible (langue de l'échange, signaux de localisation). Demander confirmation seulement en cas d'ambiguïté ou si la précision matters (ex : tarifs énergie locaux différents de la moyenne nationale).

**À la sortie de la Phase 1**, formuler **2 à 3 scénarios** en interne (et les exposer au rapport final). Exemples :
- Jeune célibataire urbain : « Tu gardes 8 ans » vs « Tu déménages/te mets en couple à 3 ans et revends »
- Famille stable : « Usage intensif 12 ans » (un seul scénario suffit)
- Retraité 75 ans : « Usage 5–8 ans » + valoriser la revente / transmission

### Phase 2 — Catégorie et contexte d'usage

Identifier la catégorie de produit (peut être déjà claire dans la demande initiale). Puis :

1. **Lister les catégories disponibles** : `ls references/categories/` (ou regarder la liste ci-dessous)
2. **Charger UNIQUEMENT le fichier de la bonne catégorie** : par exemple `references/categories/lave-linge.md` ou `references/categories/voiture.md`
3. Si **aucune catégorie ne correspond**, charger `references/categories/_default.md` (méthodologie générique)

Ne JAMAIS charger plusieurs fichiers de catégorie sans raison — c'est de la pollution de contexte. Un seul fichier suffit pour une analyse.

**Catégories disponibles dans la v1 du skill** :
- Mobilité : `voiture.md`, `velo.md` (tous types : électrique, musculaire, VTC, VTT, cargo, pliant), `scooter-moto.md`, `trottinette-electrique.md`
- Gros électroménager : `lave-linge.md`, `seche-linge.md`, `lave-vaisselle.md`, `refrigerateur.md`, `four-cuisson.md`
- Chauffage / énergie : `chaudiere.md`, `pompe-a-chaleur.md`, `chauffe-eau.md`, `climatisation.md`, `panneaux-solaires.md`
- Petit électroménager : `cafetiere.md`, `aspirateur.md`, `robot-cuiseur.md`
- Bureautique : `imprimante.md`
- Électronique : `smartphone.md`, `ordinateur-portable.md`, `tv.md`, `console-jeu.md`, `tablette.md`
- Jardin / bricolage : `tondeuse-jardin.md`, `outils-electroportatifs.md`
- Cas spéciaux : `animal-compagnie.md`, `matelas.md`
- Fallback : `_default.md`

Chaque fichier fournit, dans un format homogène :
- Variables d'usage à fort impact (questions à poser, règle des 5-10%)
- Durée de vie de référence
- Métriques de durabilité applicables
- Consommables et coûts de maintenance typiques
- Décote / revente attendue
- `cost_categories` recommandées pour le graphique de décomposition
- Notes spéciales et pièges typiques

Si une variable n'est pas demandée à l'utilisateur, utiliser la valeur par défaut de la catégorie et **noter l'hypothèse** dans la méthodologie du rapport.

### Phase 3 — Identification des candidats

- **Mode A** (liste fournie) : valider que les modèles existent et sont actuels, normaliser les noms commerciaux.
- **Mode B** (besoin décrit, cas fréquent) : recherche web pour identifier 8 à 20 candidats couvrant l'éventail des prix et marques pertinentes. Inclure au moins une option d'entrée de gamme, milieu, et haut de gamme pour faire ressortir les arbitrages.

Diversifier les sources : ne pas se limiter à un seul comparateur ou marchand.

### Phase 4 — Collecte des données

Pour chaque candidat, rechercher sur le web :
- **Prix d'achat actuel** (citer source + date)
- **Consommation énergie/carburant** (spec constructeur + tests réels si trouvés)
- **Durée de vie attendue** : défaut catégorie modulé par signaux spécifiques au modèle/marque
- **Indicateurs de fiabilité** : voir `references/reliability.md` pour les méthodes par catégorie
- **Coût des consommables** (cartouches, filtres, pneus, etc.)
- **Maintenance estimée** sur l'horizon
- **Courbe de décote** si pertinent (voitures, smartphones, gros électroménager haut de gamme)

Si une donnée importante manque après recherche web raisonnable (3-5 requêtes), demander à l'utilisateur **uniquement** si elle est à fort impact. Sinon, utiliser un défaut catégorie et marquer la zone d'ombre.

**Chaque produit doit avoir** :
- Un `data_confidence` : high / medium / low
- Une liste de `gaps` : zones où la donnée est absente ou peu fiable

### Phase 5 — Calcul du TCO par scénario

Pour chaque produit × chaque scénario :

```
TCO = prix_achat
    + Σ (coût_usage_annuel × facteur_actualisation) sur horizon
    + Σ (maintenance_annuelle × facteur_actualisation) sur horizon
    − valeur_de_revente(fin_horizon)
```

Avec :
- `coût_usage_annuel` = énergie + consommables
- `facteur_actualisation` : 2 % par défaut (opportunité du capital nette d'inflation). Préciser dans la méthodologie. Mettre à 0 si l'utilisateur préfère des chiffres bruts.
- `valeur_de_revente` : 0 sauf si la catégorie ou l'horizon le justifie (voitures, smartphones, scénario « revente à 4 ans »).

### Phase 6 — Classement

Produire **deux classements** affichés en parallèle dans le rapport :

1. **Classement TCO pur** : du moins cher au plus cher en coût total.
2. **Classement score ajusté** : TCO modulé de ±15% maximum selon fiabilité et qualité des avis utilisateurs :
   ```
   score_ajusté = TCO × (1 − 0.075 × signal_fiabilité − 0.075 × signal_avis)
   ```
   où chaque `signal` est dans [-1, +1] (positif = bon).

Limiter chaque classement aux **20 meilleurs**. Si plus de 20 candidats, garder les 20 avec le meilleur TCO comme univers de comparaison.

### Phase 7 — Génération du rapport HTML

Construire le JSON conforme au schéma (ci-dessous), l'écrire dans `data.json`, puis appeler :

```bash
python scripts/build_report.py data.json /mnt/user-data/outputs/rapport.html
```

Le script injecte les données dans `assets/report-template.html`, qui contient déjà :
- Le nuage de points cliquable (prix d'achat × coût d'usage) avec courbes d'iso-TCO
- Le graphique de décomposition par poste (barres empilées triables)
- Le styling complet

Sortie : un fichier HTML autonome (Google Fonts seul, pas de dépendances JS externes — fonctionne dans tous les sandbox).

Présenter le fichier final via `present_files`. Dans la conversation, donner un **résumé conversationnel** de 3-5 lignes : top 1 recommandé, principal arbitrage, principale zone d'ombre.

**Important — décomposition des coûts par poste.** Si `cost_categories` et `cost_breakdown` sont fournis (fortement recommandé), un second graphique apparaît : une barre empilée horizontale par modèle, qui montre **où va l'argent**. C'est souvent l'élément le plus pédagogique du rapport : il révèle des coûts cachés (ex : la lessive sur un lave-linge pèse souvent plus que le prix d'achat ; les consommables d'une cafetière à capsules dominent le TCO à 80 % ; la décote d'une voiture peut dépasser le coût du carburant).

**Contrainte de cohérence stricte** : pour chaque produit, `sum(cost_breakdown.values()) == tco_primary`. Si elles divergent, le rapport montrera des incohérences entre le scatter et le breakdown. Si tu ne fournis pas tous les postes en détail, regroupe sous un poste « Autres coûts d'usage » pour garder la somme exacte.

**Choix des postes (`cost_categories`)** : adapter à la catégorie. Toujours inclure « Prix d'achat » comme premier poste. Pour le reste, prendre les 3-5 postes dominants. Exemples :
- *Lave-linge* : Prix d'achat, Électricité, Eau, Lessive, Maintenance
- *Voiture* : Prix d'achat, Carburant, Assurance, Entretien, Décote (en négatif si revente)
- *Smartphone* : Prix d'achat, Forfait (si comparé), Réparations attendues, Accessoires, Revente (négative)
- *Cafetière capsules* : Prix d'achat, Capsules, Électricité, Détartrant
- *Imprimante* : Prix d'achat, Cartouches, Papier, Électricité

Couleurs : par défaut le script applique une palette éditoriale ; on peut surcharger via `color` par catégorie.

Structure du rapport :

---

## Schéma de données pour le rapport

```json
{
  "title": "string — ex: 'Lave-linge : meilleur TCO pour famille de 4'",
  "summary": "string — 3 phrases : verdict, arbitrage clé, niveau de confiance",
  "recommendation_title": "string — ex: 'Bosch Série 8 WAX32M40FF'",
  "recommendation": "string — 1-2 phrases sur le top choix et pourquoi",
  "personal_context": {
    "age_range": "string",
    "household": "string",
    "stability": "string",
    "country_currency": "FR/EUR"
  },
  "scenarios": [
    {
      "name": "string — ex: 'Garde 10 ans'",
      "horizon_years": 10,
      "description": "string — pourquoi ce scénario"
    }
  ],
  "cost_categories": [
    {"key": "purchase",    "label": "Prix d'achat", "color": "#1A1814"},
    {"key": "electricity", "label": "Électricité",  "color": "#7A2828"},
    {"key": "water",       "label": "Eau",          "color": "#C46F3A"},
    {"key": "detergent",   "label": "Lessive",      "color": "#B57814"},
    {"key": "maintenance", "label": "Maintenance",  "color": "#D4C8A8"}
  ],
  "breakdown_intro": "string — 2-3 phrases d'analyse des grands postes (ex: 'la lessive pèse 30% du TCO sur les modèles d'entrée')",
  "products": [
    {
      "rank_tco": 1,
      "rank_adjusted": 2,
      "brand": "string",
      "model": "string",
      "spec_line": "string — ex: '8 kg · 1400 tr/min · 7,8/10'",
      "initial_price": 1050,
      "usage_cost_lifetime": 1830,
      "tco_primary": 2880,
      "cost_breakdown": {
        "purchase": 1050,
        "electricity": 210,
        "water": 530,
        "detergent": 840,
        "maintenance": 250
      },
      "tco_per_scenario": {"Garde 12 ans": 2880, "Revente 7 ans": 1640},
      "adjusted_score": 2642,
      "reliability_signal": 0.6,
      "review_signal": 0.5,
      "data_confidence": "high",
      "gaps": [],
      "why": "string — 1-2 phrases sur pourquoi ce modèle",
      "sources": ["https://..."]
    }
  ],
  "data_callouts": [
    {"level": "warning", "title": "string (optionnel)", "text": "Pas de données de fiabilité pour le modèle X (sorti il y a 6 mois)."}
  ],
  "methodology": "string markdown — explications, hypothèses, sources, formules",
  "sources": ["https://..."]
}
```

**Règles importantes** :
- `sum(cost_breakdown.values()) == tco_primary` (cohérence visuelle entre scatter et breakdown)
- `initial_price + usage_cost_lifetime == tco_primary` (cohérence interne)
- `cost_categories` et `cost_breakdown` sont optionnels mais fortement recommandés (le graphique de décomposition est l'élément le plus pédagogique du rapport)
- `data_callouts.level` : `info` (vert) | `warning` (ambre) | `alert` (rouge)

---

## Style du rapport

Le rapport est **partageable**, donc didactique sans être condescendant :

- Titres clairs en français
- Phrases courtes
- Une recommandation par section (pas de tableaux d'exception sans verdict)
- Les zones d'ombre dans des **callouts visuels** (encadrés colorés), pas noyées dans le texte
- L'annexe méthodologique est **dépliable** (collapsed par défaut) — accessible mais non envahissante
- Les sources sont listées en fin de rapport avec lien direct

---

## Sortie conversationnelle

Après avoir produit le rapport :
1. Présenter le fichier HTML via `present_files`
2. Donner un résumé oral de 3-5 lignes : top 1, arbitrage principal, zone d'ombre majeure
3. **Ne pas** répéter en prose tout ce qui est dans le rapport. L'utilisateur va l'ouvrir.

---

## Ressources

- `references/personal-context.md` — Comment dériver des scénarios depuis l'âge, le foyer, la stabilité
- `references/categories/` — **Un fichier par catégorie de produit** : questions d'usage, durées de vie, métriques de durabilité, consommables types. Lire uniquement le fichier de la catégorie concernée (ou `_default.md` si non trouvée).
- `references/reliability.md` — Comment estimer la fiabilité par type de produit et gérer les manques
- `assets/report-template.html` — Squelette HTML autonome (SVG vanilla, Google Fonts uniquement)
- `scripts/build_report.py` — Injecte le JSON dans le template
