# Lave-linge

## Variables d'usage à fort impact
- **Nombre de lavages/semaine** (4-8 selon foyer)
- **Capacité utile** : sous-dimensionner = lavages plus fréquents et mauvaise efficacité
- **Eau dure / douce** : impacte durée de vie (calcaire) et consommation détergent
- **Tarif électricité** (heures pleines/creuses change le classement de 5-10 % sur modèles énergivores)
- **Possibilité de lavage à froid majoritaire** : divise par 2-3 la conso énergie
- **Présence d'un sèche-linge séparé** : si oui, privilégier essorage 1400-1600 tr/min ; sinon 1200 suffit

## Durée de vie de référence
- Entrée de gamme : 7-9 ans
- Milieu de gamme : 10-12 ans
- Haut de gamme Miele/Bosch série 8+/AEG haut : 12-15 ans documentés
- Marques pro (Miele Professional) : 20+ ans

## Métriques de durabilité
- **Indice de réparabilité français** (obligatoire depuis 2021, score /10)
- **Disponibilité pièces détachées** (loi anti-gaspillage : 11 ans après dernière vente)
- **Que Choisir / Stiftung Warentest** tests longue durée
- **Forum Que Choisir, Système-D, Reddit r/BuyItForLife** : retours utilisateurs
- **Taux de retour SAV** chez Darty/Boulanger (parfois mentionnés)

## Consommables et postes récurrents
- Lessive : 60-90 €/an pour 6 lavages/semaine
- Anticalcaire (zone calcaire) : 20-40 €/an
- Maintenance : courroie + pompe selon fiabilité (encode le risque de remplacement sur l'horizon)

## Décote / revente
Faible. Compter 0 sauf modèle Miele récent (< 5 ans) qui peut se revendre 30-40 %.

## cost_categories recommandées
```json
[
  {"key": "purchase",    "label": "Prix d'achat", "color": "#1A1814"},
  {"key": "electricity", "label": "Électricité",  "color": "#7A2828"},
  {"key": "water",       "label": "Eau",          "color": "#C46F3A"},
  {"key": "detergent",   "label": "Lessive",      "color": "#B57814"},
  {"key": "maintenance", "label": "Maintenance",  "color": "#D4C8A8"}
]
```

## Notes spéciales
- **Capacité kg** : sous-dimensionner = +50 % de lavages/an = +TCO. Sur-dimensionner = TCO neutre car un modèle plus grand fait des cycles partiels efficaces.
- **Lavage à froid** : majoritaire = la conso énergie chute de 60-70 %. Demander à l'utilisateur s'il y est prêt.
