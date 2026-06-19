# Homo Economicus

> Ce skill aide Claude à produire des analyses d'achat optimisées sur tout le cycle de vie d'un produit : prix d'achat + énergie + consommables + maintenance + décote. La sortie est un rapport HTML partageable avec deux graphiques interactifs.

## À quoi ça sert

L'idée est simple : **la plupart des décisions d'achat se font sur le prix d'achat**, alors que c'est souvent une fraction du coût réel. Un imprimante à 60 € peut coûter 900 € de cartouches sur 5 ans. Une voiture diesel à 25 000 € peut décoter de 15 000 € en 5 ans. Un lave-linge premium peut amortir sa prime en évitant un remplacement à 8 ans.

Homo Economicus aide à voir le vrai coût et à comparer les modèles sur la durée. Le skill :
- Pose d'abord les bonnes questions sur **ton contexte personnel** (âge, foyer, stabilité) parce que l'horizon d'amortissement dépend de toi
- Construit **2-3 scénarios** plausibles selon ce contexte
- Cherche les **prix et caractéristiques** des modèles candidats sur le web
- Estime la **fiabilité** à partir de plusieurs sources et marque les zones d'ombre
- Calcule le **TCO** par scénario et produit un **classement double** : TCO pur + score ajusté fiabilité/avis
- Génère un **rapport HTML** avec un nuage de points cliquable, un graphique de décomposition par poste, et des callouts visuels pour les zones d'incertitude

## Configuration recommandée

| Élément | Recommandation |
|---|---|
| **Modèle** | Claude Opus (analyse complexe, raisonnement sur plusieurs sources) |
| **Recherche web** | **Activée** — essentielle pour les prix et caractéristiques actuels |
| **Mode Research** | Recommandé pour les analyses avec ≥ 10 modèles à comparer ; permet une recherche plus exhaustive sur la fiabilité |
| **Code Execution + File Creation** | **Activé** — nécessaire pour générer le rapport HTML |
| **Style de réponse** | Tout style (le skill définit son propre format de sortie) |

### Quand utiliser quel mode

- **Mode chat normal** : recherche d'un produit courant (lave-linge, smartphone) où 3-5 minutes d'analyse suffisent
- **Mode Research** : voiture, panneaux solaires, pompe à chaleur — décisions à plusieurs milliers d'euros qui méritent une analyse plus approfondie (15-30 min de calcul)
- **Sans Mode Research mais avec recherche web** : reste le défaut pour la plupart des cas

## Installation

Le skill est un dossier `homo-economicus/`. Selon la plateforme :

- **Claude.ai (Pro/Team)** : déposer le dossier dans tes Skills, le skill se déclenchera automatiquement quand tu parleras d'un achat
- **API / Claude Code** : monter le dossier dans `/mnt/skills/user/homo-economicus/` ou équivalent
- **Cowork** : déposer dans le dossier des skills personnalisés

## Comment l'invoquer

Le skill se déclenche tout seul quand tu mentionnes un achat. Mais tu peux aussi être explicite. Exemples qui marchent :

- *« Je dois changer mon lave-linge. On est 4 à la maison, deux ados, environ 6-7 lessives par semaine. Aide-moi à choisir. »*
- *« Compare ces 5 modèles d'imprimante laser pour mon usage perso : [liste]. »*
- *« J'ai 74 ans et je veux changer ma voiture pour une occasion. 8000 km/an, surtout en ville. Mon mari m'aide pour l'entretien. »*
- *« Optimise mon prochain achat de vélo électrique pour aller au travail (12 km plat aller-retour). »*
- *« Cafetière à grains ou à capsules ? Je bois 2 cafés/jour. »*

## Catégories supportées (v1)

Le skill couvre **27 catégories** dont les plus fréquentes :

- **Mobilité** : voiture, vélo (tous types : électrique / musculaire / VTC / VTT / cargo / pliant), scooter/moto, trottinette électrique
- **Gros électroménager** : lave-linge, sèche-linge, lave-vaisselle, réfrigérateur, four/plaques
- **Chauffage / énergie** : chaudière, pompe à chaleur, chauffe-eau, climatisation, panneaux solaires
- **Petit électroménager** : cafetière, aspirateur, robot cuiseur
- **Bureautique** : imprimante
- **Électronique** : smartphone, ordinateur portable, TV, console de jeu, tablette
- **Jardin / bricolage** : tondeuse et outils de jardin, outils électroportatifs
- **Cas spéciaux** : animal de compagnie, matelas

Pour une catégorie non listée, le skill utilise une méthodologie générique (`_default.md`) qui produit une analyse correcte mais sans le niveau de détail d'un fichier dédié.

## Limites à connaître

1. **Le skill ne remplace pas un avis vérifié** : il agrège des données publiques et applique des heuristiques. Pour un achat important (voiture, chauffage), croiser avec un avis humain.
2. **Les prix bougent** : un rapport généré aujourd'hui peut être obsolète dans 6 mois. Le skill mentionne les dates de ses sources.
3. **Les données de fiabilité sont souvent pauvres** sur les modèles récents (< 18 mois). Le skill le signale dans des callouts visuels.
4. **L'analyse est calculée par Claude** : risque d'erreur arithmétique ou d'hallucination de spécifications. Sur les chiffres-clés (prix, conso énergie), vérifier les sources citées.
5. **Le rapport est en français par défaut** ; pour une autre langue, le préciser dans la demande.

## Structure du rapport produit

Chaque rapport contient :
1. **Résumé exécutif** (3 phrases, verdict)
2. **Recommandation principale** (le modèle qui sort en tête et pourquoi)
3. **Contexte personnel + scénarios** retenus
4. **Top 3** avec rationale
5. **Nuage de points** : prix d'achat × coût d'usage, avec lignes d'iso-TCO et points cliquables (ouvrent Google sur le modèle)
6. **Décomposition par poste** : barres empilées triables par catégorie de coût
7. **Classements** : par TCO pur ET par score ajusté fiabilité/avis
8. **Sensibilité aux scénarios** (si plusieurs scénarios)
9. **Zones d'ombre & limites** dans des callouts visuels
10. **Méthodologie** (dépliable) : hypothèses, formules, sources
11. **Sources consultées**

Le rapport est un fichier HTML autonome, ouvrable dans n'importe quel navigateur, sans dépendance externe (sauf Google Fonts pour la typographie). Taille typique : 40-80 KB.

## Comment étendre le skill

Pour ajouter une catégorie :
1. Créer `references/categories/<nom-categorie>.md` en suivant le format des fichiers existants
2. Ajouter la catégorie à la liste dans le SKILL.md
3. Tester sur un cas réel

Pour modifier le style du rapport :
- Toucher à `assets/report-template.html` (CSS dans le `<style>`) — *fichier à venir*
- Tester avec un sample data via `python scripts/build_report.py sample.json output.html` — *script à venir*

## Architecture interne (pour curieux)

```
homo-economicus/
├── SKILL.md                          # Orchestrateur du workflow en 7 phases
├── README.md                         # Ce fichier
├── references/
│   ├── personal-context.md           # Comment dériver les scénarios (à venir)
│   ├── reliability.md                # Comment estimer la fiabilité (à venir)
│   └── categories/
│       ├── _default.md               # Méthodologie générique
│       ├── lave-linge.md             # Une fiche par catégorie
│       ├── velo.md
│       ├── voiture.md
│       └── ...                       # (27 catégories au total)
├── assets/
│   └── report-template.html          # Template HTML autonome (SVG vanilla) (à venir)
└── scripts/
    └── build_report.py               # Injecte le JSON dans le template (à venir)
```

**Principe de design** : le SKILL.md reste léger (~250 lignes) car il est chargé à chaque déclenchement. Les fichiers de catégorie sont lus à la demande — seul celui qui correspond au produit demandé est chargé en mémoire. Cette « progressive disclosure » permet d'avoir une expertise très détaillée par catégorie sans saturer le contexte.

## Crédits

Skill conçu par et pour des décisions d'achat rationnelles, en France, autour de 2026.