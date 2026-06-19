## Description

<!-- Décris le changement et sa motivation. -->

## Type de changement

- [ ] Nouveau fichier de catégorie (`references/categories/`)
- [ ] Correction dans SKILL.md ou une catégorie existante
- [ ] Amélioration du script Python ou du template HTML
- [ ] Documentation (CLAUDE.md, README, CHANGELOG)
- [ ] Autre :

## Checklist

- [ ] La description de SKILL.md fait ≤ 130 mots (la CI le vérifie)
- [ ] Si le template HTML a été modifié : aucun CDN ni JS externe ajouté
- [ ] Si `build_report.py` a été modifié : les invariants `sum(breakdown) == tco_primary` et `price + usage == tco_primary` tiennent
- [ ] CLAUDE.md mis à jour si une décision de design a changé
- [ ] CHANGELOG.md mis à jour si pertinent
