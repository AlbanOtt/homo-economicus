# Vélo

Couvre tous les types : musculaire et électrique, urbain (VTC, ville), tout-chemin (gravel), sportif (route, VTT), utilitaire (cargo, longtail, biporteur), pliant. Le skill doit d'abord aider à **identifier le bon type** avant de chercher des modèles.

## Aide au choix du type de vélo

Poser ces questions si l'utilisateur n'a pas tranché. Chaque réponse oriente fortement le type :

- **Usage principal** : domicile-travail, balade, sport, courses/enfants, voyage longue distance ?
- **Distance quotidienne et dénivelé** : < 5 km plat → ville musculaire suffit / 5-15 km plat → VTC musculaire ou électrique léger / > 15 km ou dénivelé important → électrique / > 30 km → électrique avec grosse batterie
- **Type de terrain** : asphalte uniquement → ville/VTC/route / chemins → gravel/VTT / mixte → VTC ou gravel
- **Charge transportée** : 0-5 kg → standard / enfants ou courses régulières → cargo (longtail) ou biporteur / matériel pro → cargo lourd
- **Stockage** : appartement sans local → pliant ou vélo léger / cave/garage → tout est possible
- **Stationnement extérieur sans abri** : composants à pénaliser (chaîne traditionnelle ouverte → préférer courroie ou roue libre intégrée)
- **Sensibilité au vol** : zone urbaine → prévoir gros antivol (50-150 €) et adapter le choix vers du moins identifiable, ou prévoir assurance

Une fois le type identifié, chercher 8-15 modèles sur le segment correspondant.

## Variables d'usage à fort impact

- **Km/an** : pilote l'usure pneus, chaîne, plaquettes
- **Conditions** : pluie + sel hivernal accélère la corrosion (privilégier composants inox, transmission par courroie sur usage utilitaire intensif)
- **Type de transmission** : chaîne dérailleur classique (entretien fréquent, peu cher) vs moyeu intégré Nexus/Alfine (peu d'entretien, plus cher à l'achat) vs courroie carbone (zéro entretien, premium)
- **Pour électrique** : capacité batterie, marque du moteur (Bosch, Shimano, Yamaha, Brose dominent ; gen Bosch Performance Line CX = référence)
- **Charge transportée régulièrement** (pour cargo) : impacte usure freins et transmission

## Durée de vie de référence

- **Cadre acier ou aluminium qualité** : 15-25 ans (le cadre survit toujours, ce sont les composants qui s'usent)
- **Cadre carbone** : 8-15 ans selon usage
- **Moteur électrique milieu/haut de gamme** : 8-15 ans (Bosch garantit officiellement 5 ans mais durabilité documentée jusqu'à 15 ans)
- **Batterie** : 5-10 ans (perte de 20-30 % de capacité à 5 ans en usage modéré). Coût remplacement 400-900 € selon capacité.
- **Composants à changer en cours de vie** : chaîne (3-8 000 km), pneus (2-5 000 km), plaquettes (2-10 000 km), pignons (10-15 000 km), batterie (5-10 ans)

## Métriques de durabilité

- **Marque cadre vs marque composants** : un vélo "Decathlon" avec composants Shimano XT vaut souvent mieux qu'un vélo "marque premium" avec composants Shimano Altus
- **Indice de réparabilité français** : depuis 2023 sur les vélos électriques
- **Forums spécialisés** : VeloElectrique.com, Vélo101, BikeRumor (anglais), Reddit r/ebikes, r/cargobike, r/justridingalong
- **Disponibilité pièces standard** : préférer des composants Shimano/SRAM série courante plutôt que pièces exotiques constructeur
- **Pour électrique** : préférer les marques motorisations établies (Bosch, Shimano EP8, Yamaha PW) — les moteurs "noname" chinois ne sont souvent pas réparables après 3-5 ans

## Consommables et postes récurrents

- **Pneus** : 30-80 €/pneu, remplacement tous les 2-5 000 km
- **Chaîne** : 20-50 €, remplacement tous les 3-8 000 km
- **Plaquettes de frein** (disque) : 15-30 € le jeu, 2-10 000 km selon usage
- **Câbles et gaines** : 30-50 € tous les 3-4 ans
- **Révision annuelle** : 50-120 € (recommandée si l'utilisateur ne fait pas lui-même)
- **Batterie électrique** : remplacement à 5-10 ans, 400-900 €
- **Énergie (électrique)** : très faible, ~5-15 €/an (négligeable)

## Décote / revente

- **Vélo musculaire de marque (Cube, Trek, Specialized, Canyon, Decathlon série Riverside/Rockrider)** : décote modérée, 40-50 % à 5 ans, marché occasion dynamique
- **Vélo électrique premium** : décote 50-60 % à 5 ans
- **Vélo électrique entrée de gamme générique** : décote 70 %+ à 3 ans
- **Cargo** : marché secondaire récent mais en croissance, décote 40-50 % à 5 ans pour Babboe, Riese & Müller, Tern, etc.

## cost_categories recommandées

```json
[
  {"key": "purchase", "label": "Prix d'achat", "color": "#1A1814"},
  {"key": "consumables", "label": "Consommables", "color": "#7A2828"},
  {"key": "maintenance", "label": "Entretien", "color": "#C46F3A"},
  {"key": "battery", "label": "Batterie (si VAE)", "color": "#B57814"},
  {"key": "depreciation", "label": "Décote nette", "color": "#D4C8A8"}
]
```

Pour un vélo musculaire, retirer la ligne « Batterie ».

## Notes spéciales

- **Effet écosystème batterie** : sur un vélo électrique, le choix d'une motorisation Bosch garantit la disponibilité de pièces 10-15 ans ; choisir un moteur "blanc" exotique = risque de fin de vie programmée par indisponibilité de la batterie de remplacement.
- **Coût d'antivol et stationnement** : sur un vélo > 1 500 €, prévoir 80-200 € d'antivol Sold Secure Gold, et potentiellement assurance (60-150 €/an). À intégrer dans `consumables` ou comme catégorie séparée si l'analyse est précise.
- **Forfait mobilités durables** : si l'employeur le propose, peut réduire effectivement le TCO de 200-700 €/an. À demander à l'utilisateur.
- **Cargo vs deuxième voiture** : pour les familles urbaines, comparer le TCO d'un cargo (3-7 k€ + entretien) avec celui d'une deuxième voiture (10-20 k€/an sur 5 ans toutes charges) peut être plus parlant qu'une comparaison entre cargos.
