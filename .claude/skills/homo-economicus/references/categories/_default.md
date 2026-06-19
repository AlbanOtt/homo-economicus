# _default — Méthodologie générique

Utiliser ce fichier quand la catégorie demandée n'a pas de fichier dédié dans `references/categories/`. Cette méthodologie générique permet quand même de produire une analyse TCO correcte, à condition d'identifier les bons leviers.

## Étape 1 — Identifier les composantes de coût dominantes

Pour tout produit, le TCO se décompose en :

```
TCO = prix_achat + Σ(coûts d'usage par an × horizon) + maintenance attendue − valeur de revente
```

Les composantes à déterminer pour cette catégorie inconnue :

1. **Qu'est-ce que ce produit consomme en usage ?** Énergie ? Carburant ? Eau ? Consommables propriétaires (capsules, cartouches, filtres, lames…) ? Abonnement obligatoire ?
2. **Quelle est sa fréquence d'usage typique ?** Quotidien ? Hebdo ? Saisonnier ?
3. **A-t-il une maintenance prévisible ?** Pièces d'usure régulières ? Révisions ? Remplacement de composants (batterie, pneus, filtres) ?
4. **Y a-t-il un marché secondaire ?** Si oui, la décote est un poste majeur. Sinon, valeur de revente = 0.

## Étape 2 — Estimer la durée de vie

Sans donnée catégorie spécifique, partir de la **durée de garantie légale étendue** comme borne basse, et chercher sur le web "durée de vie [produit] moyenne" pour affiner. Croiser avec :
- Indice de réparabilité français si applicable (data.gouv.fr)
- Disponibilité des pièces détachées (loi anti-gaspillage : 5-11 ans selon catégorie)
- Retours d'usage longue durée sur forums spécialisés

Si vraiment aucune donnée : prendre 7 ans par défaut et noter cette hypothèse dans la méthodologie du rapport.

## Étape 3 — Choisir les cost_categories pour la décomposition

Adapter à la catégorie. Toujours mettre « Prix d'achat » en premier. Postes typiques par nature de produit :

- **Produit électrique** : Prix d'achat, Électricité, Consommables, Maintenance
- **Produit à carburant** : Prix d'achat, Carburant, Maintenance, Assurance, Décote
- **Produit à consommables propriétaires** : Prix d'achat, Consommables, Énergie, Maintenance — **attention : sur ce type de produit, les consommables dominent souvent à 70-90 % du TCO**
- **Service / abonnement** : Engagement initial, Mensualités × horizon, Frais de sortie

Garder 4-5 postes maximum pour la lisibilité du graphique de décomposition.

## Étape 4 — Trouver les sources de fiabilité

Pour une catégorie inconnue, chercher par ordre de priorité :
1. **Associations de consommateurs** : Que Choisir, Test-Achats, Stiftung Warentest, Consumer Reports
2. **Tests longue durée** : recherche "X années d'utilisation [produit]" sur YouTube et forums
3. **Avis cumulés** : sites marchands avec ≥ 200 avis (filtrer les avis trop récents)
4. **Indice de réparabilité** s'il existe pour la catégorie
5. **Disponibilité des pièces détachées** (signal fort de réparabilité)

Si les données sont vraiment pauvres, `data_confidence = "low"` pour les modèles concernés et ajout d'un callout explicite dans le rapport.

## Étape 5 — Questions à poser à l'utilisateur

Appliquer la règle des 5-10 % : ne poser que les questions qui peuvent changer le classement final. Pour une catégorie inconnue, les questions universelles utiles sont :

- Intensité d'usage (fréquence, durée)
- Contraintes spécifiques (mobilité, espace, partage avec d'autres usages)
- Sensibilité à la marque / écosystème (verrouillage de plateforme)
- Tarifs locaux si l'usage est énergivore

## Important

Quand cette méthodologie générique est utilisée, **toujours signaler dans la méthodologie du rapport** que la catégorie n'avait pas de fiche dédiée. Cela permet à l'utilisateur d'apprécier le niveau de confiance, et incite à enrichir le skill si la catégorie se présente plusieurs fois.
