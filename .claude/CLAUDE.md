# CLAUDE.md

> Contexte projet pour tout agent Claude (Claude Code, Claude dans l'app, autre) qui intervient dans ce repo. À lire avant de modifier des fichiers. Ce document est tenu à jour au fil du projet — si tu fais évoluer une décision de design, mets ce fichier à jour dans le même commit.

## 1. Le projet en bref

**Homo Economicus** est un Claude skill (format Superpowers, `SKILL.md` dans `.claude/skills/homo-economicus/`) qui aide à analyser le coût total de possession (TCO) d'un achat sur tout son cycle de vie et produit un rapport HTML interactif. Cible : utilisateurs francophones, contexte France (tarifs énergie EDF, indices de réparabilité français, Que Choisir, Argus, etc.).

## 2. État du projet

### Structure
```
homo-economicus/                      # Racine du projet
├── CLAUDE.md                         # Ce fichier
├── README.md                         # Guide utilisateur (modèles, modes, install)
├── LICENSE
├── .gitignore                        # Exclut .claude/skill-creator/
└── .claude/
    └── skills/
        └── homo-economicus/          # Le skill (format Superpowers)
            ├── SKILL.md              # Orchestrateur, 7 phases, ~280 lignes
            ├── references/
            │   ├── categories/       # 1 fichier par catégorie de produit
            │   │   ├── _default.md   # Fallback générique
            │   │   ├── voiture.md
            │   │   ├── velo.md       # (tous types : élec, musculaire, cargo…)
            │   │   └── ...           # (autres catégories à rédiger)
            │   ├── [à venir] personal-context.md  # Scénarios par âge/foyer/stabilité
            │   └── [à venir] reliability.md        # Méthodes d'estimation fiabilité
            ├── [à venir] assets/
            │   └── report-template.html  # Template HTML autonome (SVG vanilla)
            └── [à venir] scripts/
                └── build_report.py       # Injecte JSON → HTML
```

### Ce qui marche
- Génération du rapport HTML depuis JSON via `python scripts/build_report.py data.json report.html`
- Rendu SVG vanilla : scatter chart cliquable + barres empilées de décomposition triables
- Aucune dépendance JS externe (Google Fonts seul, autorisé dans tous les sandbox testés)
- Description SKILL.md à 112 mots (cible skill-creator ~100, marge OK)

### Ce qui n'a PAS été éprouvé
- L'invocation du skill par Claude sur un cas utilisateur réel (le test sur lave-linge utilisait des données fabriquées à la main)
- Les 27 catégories autres que `lave-linge.md` (rédigées sans test bout-en-bout)
- L'installation dans Claude.ai / API / Cowork (chemins décrits dans le README, non vérifiés sur chaque plateforme)

## 3. Décisions de design — à respecter

Ces choix ont été pris délibérément. Ne les remets pas en cause sans validation explicite de l'utilisateur humain.

### 3.1 Esthétique du rapport
- **Direction éditoriale**, pas dashboard générique. Inspirations : The Economist, Reuters analytical pieces.
- **Typographie** : Fraunces (display serif) + IBM Plex Sans (corps) + IBM Plex Mono (chiffres). Pas Inter, pas system fonts.
- **Palette** : warm off-white `#FAF6EE`, ink `#1A1814`, accent `#A8521C` (terre cuite). Pas de gradient purple/blue.
- **Mise en page** : prose readable, callouts visuels colorés, méthodologie repliable.

### 3.2 Architecture progressive disclosure
Le SKILL.md reste **léger** (chargé à chaque déclenchement, coûte du contexte à TOUS les utilisateurs). Les détails métier sont dans `references/categories/<nom>.md` — un seul fichier est lu par analyse, celui de la catégorie demandée. `_default.md` couvre les catégories non listées.

Si tu enrichis une catégorie, c'est dans son fichier dédié, pas dans SKILL.md.

### 3.3 Auto-suffisance HTML
Le rapport HTML doit fonctionner **sans aucune ressource externe** sauf Google Fonts. Pas de Plotly, pas de D3, pas de Tailwind, pas de Chart.js via CDN. C'est délibéré : les sandbox Claude bloquent les scripts JS externes (testé, Plotly a cassé). Tout le rendu est en SVG vanilla via JS inline.

### 3.4 Français-first
Le skill est conçu pour des utilisateurs francophones. Le rapport est en français. Une éventuelle version EN serait une dérivée, pas une refonte.

### 3.5 Règle des 5-10 %
Le skill ne pose que les questions qui peuvent changer le TCO ou le classement final d'au moins 5-10 %. Pas de questionnaire à 15 items.

### 3.6 Contraintes de cohérence des coûts
Pour chaque produit dans le JSON :
- `sum(cost_breakdown.values()) == tco_primary`
- `initial_price + usage_cost_lifetime == tco_primary`

Ces invariants garantissent que les deux graphiques (scatter + breakdown) restent cohérents. Si tu modifies le script ou le template, vérifie que ces contraintes tiennent.

### 3.7 Honnêteté épistémique
Quand les données de fiabilité manquent, le rapport doit le dire **explicitement** dans des callouts visuels et le mentionner dans la méthodologie. Pas de pseudo-confiance, pas de masquage des zones d'ombre.

## 4. Anti-patterns connus

Réflexes typiques de LLM qui constitueraient ici une régression :

- Remplacer le SVG vanilla par Chart.js / Plotly / D3 « pour faire plus moderne » → casse dans le sandbox
- Remplacer Fraunces + IBM Plex par Inter « pour faire plus standard » → choix typo délibéré
- Traduire en anglais sans validation → produit français
- Ajouter `node_modules/`, `package.json`, du build tooling → c'est un skill statique, pas de build step
- Enrichir SKILL.md avec tout le contenu « pour que ce soit plus complet » → casse la progressive disclosure
- Supprimer les callouts d'incertitude « pour que le rapport ait l'air plus pro » → l'honnêteté est une feature
- Convertir le rapport HTML en PDF par défaut → le HTML interactif est le format choisi
- Ajouter des emojis dans le rapport → ton éditorial et sobre
- Complexifier `cost_breakdown` (dict de listes, objets imbriqués) → format actuel `{key: value}` volontairement simple

## 5. Discussion ouverte — ne pas trancher unilatéralement

L'utilisateur travaille sur une **stratégie de validation indépendante** du skill. Claude (cette conversation) avait proposé 7 approches : re-run indépendant, vérification de citations, calibration vs comparateurs établis, audit arithmétique, red-team adversarial, test suite avec ground truth, confiance auto-déclarée. L'utilisateur a sa propre approche en tête mais ne l'a pas encore partagée publiquement dans le projet.

**Ne propose pas de stratégie de validation toi-même** et n'implémente rien dans ce sens sans instruction directe. Si tu vois apparaître `validation/` ou `eval/` dans le repo, c'est que la décision a été prise — sinon, hors-sujet.

## 6. Conventions de travail

- **Commits atomiques**, messages clairs en français. Format conventionnel accepté : `feat:`, `fix:`, `docs:`, etc.
- **Pas de force-push** sur `main`
- **Branche `main` reste publiable** à tout moment ; travail expérimental sur branches `feature/...` ou `chore/...`
- **Avant un changement structurel** (architecture, dépendance, format de schéma), pose une question à l'utilisateur si tu doutes
- **Met à jour ce CLAUDE.md** dans le même commit qu'un changement qui invalide l'une de ses sections

## 7. Pistes d'amélioration possibles

Améliorations cohérentes avec le projet, à proposer mais pas implémenter en silence :

- Tests unitaires pour `build_report.py` (pytest)
- Linter / formatter (ruff, black) avec config
- CI GitHub Actions qui valide les invariants (cf. §3.6) et la longueur de la description SKILL.md
- Mode `--dry-run` dans `build_report.py` qui valide le JSON sans écrire le HTML
- Default `cost_categories` dans le code Python si absentes du JSON
- i18n du template + fichiers de catégorie en anglais comme dérivés (gros chantier, demander avant)

## 8. Travail en cours

Section vivante. À mettre à jour quand le focus du projet bouge. À la date d'écriture de ce fichier :

- **Préparation publication GitHub** : il manque `LICENSE`, `.gitignore`, `CONTRIBUTING.md`, `CHANGELOG.md`, captures d'écran dans `docs/screenshots/`, dossier `examples/` avec le sample lave-linge (HTML + JSON source), README enrichi (badges, screenshots, quick start, exemple)
- **Vérifications avant push** :
  ```bash
  python .claude/skills/homo-economicus/scripts/build_report.py examples/lave-linge-famille-stable.json /tmp/test.html
  grep -c '{{' /tmp/test.html        # doit retourner 0
  grep -c 'cdn\.\|\.js"' .claude/skills/homo-economicus/assets/report-template.html  # doit retourner 0
  python -c "import re; d=re.search(r'description:\s*(.+?)\n---', open('.claude/skills/homo-economicus/SKILL.md').read(), re.DOTALL).group(1); assert len(d.split()) <= 130, f'description {len(d.split())} mots'"
  ```