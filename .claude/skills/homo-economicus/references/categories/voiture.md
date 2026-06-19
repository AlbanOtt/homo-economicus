# Voiture

Couvre voitures particulières neuves et d'occasion : thermique (essence/diesel), hybride (HEV, PHEV), électrique (BEV). Première phase critique : déterminer le type de motorisation pertinent **avant** de chercher des modèles.

## Aide au choix de motorisation

Si l'utilisateur n'a pas tranché entre les motorisations, poser ces questions :

- **Km/an** : < 10 000 → thermique ou hybride léger / 10-20 000 → toutes options viables / > 20 000 → électrique gagne souvent sur TCO
- **Type de trajets** : urbain quasi-exclusif → électrique ou hybride / mix → tout / longue distance fréquente → thermique (diesel sur > 25 000 km/an) ou hybride
- **Possibilité de recharger à domicile** : non → électrique pénalisée
- **Budget d'achat** : électrique souvent + cher à l'achat, mais souvent − cher en TCO sur 10 ans
- **Aversion à l'incertitude** : l'électrique a moins de recul sur la durée de vie batterie au-delà de 10 ans

## Variables d'usage à fort impact (règle des 5-10 %)

- **Km/an** (le levier principal — change tout)
- **Ratio ville / route / autoroute** (change la conso de 30-50 % sur thermique, plus encore sur électrique)
- **Auto-entretien possible ou pas** (vidange, plaquettes : 200-400 €/an d'économies si oui)
- **Stationnement** : extérieur expose à la décote carrosserie ; garage = +1-2 ans de durée de vie
- **Pour électrique** : recharge domicile + tarif local + possibilité heures creuses
- **Profil d'assurance** : très variable, à demander si le but est un classement TCO précis (sinon utiliser des moyennes catégorie)

## Durée de vie de référence

- **Thermique fiable** (Toyota, Mazda, Honda…) : 15-20 ans / 250-350 000 km
- **Thermique moyenne** : 12-15 ans / 200-250 000 km
- **Hybride classique** : 12-15 ans (batterie reste fonctionnelle 10-15 ans, remplacement 1500-3000 € à anticiper après)
- **Électrique** : 10-15 ans (limite batterie). SOH à 80 % à 8-10 ans selon usage. Remplacement batterie 7-15 k€ aujourd'hui mais en baisse.

Adapter à l'horizon personnel : un retraité 75 ans n'a pas besoin de planifier 15 ans, mais doit valoriser la décote/revente.

## Métriques de durabilité (où regarder)

- **ADAC Pannenstatistik** (Allemagne — le plus riche en données long-terme, données /modèle/année)
- **J.D. Power Vehicle Dependability Study** (US — utile sur les marques globales)
- **Consumer Reports Annual Auto Survey** (US, méthodologie sérieuse)
- **Dossier fiabilité Que Choisir et L'Automobile Magazine** (annuels, France)
- **Forums spécifiques modèle** : chercher « [modèle] retour expérience 200000 km »
- **Argus / La Centrale** : taux de décote 1, 3, 5 ans = proxy indirect de fiabilité perçue
- **Pour électrique** : Recurrent Auto, retours batterie (Tesla, Renault Zoé, Hyundai Kona ont beaucoup de data)

## Consommables et postes récurrents

- **Carburant / électricité** : poste 1
- **Assurance** : 400-1500 €/an selon profil — à confirmer avec l'utilisateur si analyse précise
- **Entretien programmé** : 400-800 €/an thermique, 200-500 €/an électrique
- **Pneus** : 400-800 € tous les 3-4 ans (premium 800-1500 €)
- **Contrôle technique** : 80 € tous les 2 ans après 4 ans
- **Crit'Air, péages, parking** : selon usage et localisation

## Décote / revente — **prépondérante**

C'est souvent **le plus gros coût caché** d'une voiture. Toujours l'inclure dans le TCO.

Ordres de grandeur de décote sur 5 ans :
- Marques premium allemandes : 40-50 %
- Toyota / Honda hybride : 30-40 % (excellente tenue)
- Renault / Peugeot / Citroën neuf : 50-60 %
- Tesla / VE premium : 40-55 % (très variable selon modèle et état marché)
- VE entrée de gamme : peut atteindre 60-70 % (effet baisse rapide des prix neufs)

## Cas d'usage ville vs autoroute — note importante

À kilométrage égal, la nature de l'usure diffère. Adapter les estimations :
- **Trajets courts répétés à froid** : usure moteur accélérée ; sur diesel, encrassement FAP ; sur électrique, moins critique mais moins efficient (chauffage habitacle pèse).
- **Autoroute** : consommation supérieure mais usure linéaire et prévisible ; mieux pour les diesels modernes.
- **Ville** : surconsommation thermique (30-50 % de plus que cycle mixte), électrique au top (régen + pas de ralenti).

## cost_categories recommandées

```json
[
  {"key": "purchase", "label": "Prix d'achat", "color": "#1A1814"},
  {"key": "fuel", "label": "Carburant/Élec.", "color": "#7A2828"},
  {"key": "insurance", "label": "Assurance", "color": "#C46F3A"},
  {"key": "maintenance", "label": "Entretien", "color": "#B57814"},
  {"key": "depreciation", "label": "Décote nette", "color": "#D4C8A8"}
]
```

Note : la décote est un coût net (prix d'achat − valeur résiduelle), à inclure comme poste séparé pour qu'elle apparaisse visuellement. Cela évite de la noyer dans `purchase`.

## Notes spéciales

- **Bonus écologique / prime à la conversion** : peut représenter 2-5 k€, à intégrer comme réduction du prix d'achat. Vérifier les conditions d'éligibilité à la date de l'analyse.
- **Voiture d'occasion** : la décote est beaucoup moins forte (déjà absorbée par le 1er propriétaire). Mais la maintenance/fiabilité devient le poste critique.
- **Leasing / LOA / LLD** : transforme le problème en mensualité + frais de sortie. Si l'utilisateur compare achat vs leasing, fournir les deux TCO en parallèle.
