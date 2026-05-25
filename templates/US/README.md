# User stories (US)

Chaque user story est un dossier `US-XXX-<slug>/` avec trois vues :

| Fichier | Contenu |
|---------|---------|
| `fonctionnel.md` | Quoi et pourquoi (métier, acceptance criteria) |
| `technique.md` | Comment (API, données, stack, répartition agent / dev) |
| `visuel.md` | Apparence (maquettes, shadcn, états, a11y) |

## Nommage

- `XXX` : numéro sur 3 chiffres (`001`, `002`, …)
- `<slug>` : kebab-case court (`auth-login`, `dashboard-kpi`)

## Workflow vibe-coding

1. Rédiger ou mettre à jour les trois fichiers avant d’implémenter.
2. Implémenter dans `next-app/` en suivant `technique.md` et `visuel.md`.
3. Cocher les critères d’acceptation dans `fonctionnel.md` au fil de l’eau.

## Modèle

Copier le dossier [story/](story/) vers `US-XXX-<slug>/` et renommer.
