# Skill — bootstrap vibe-coding (Next.js)

Skill Cursor pour initialiser rapidement un dépôt **vibe-coding** autour de Next.js + shadcn (preset b0).

## Installation

Copier le skill dans vos skills personnels :

```bash
mkdir -p ~/.cursor/skills
cp -r .cursor/skills/bootstrap-vibe-coding ~/.cursor/skills/
```

Ou utiliser ce dépôt comme skill de projet : laisser `.cursor/skills/bootstrap-vibe-coding/` à la racine d’un repo dédié aux skills.

## Utilisation

Dans Cursor, invoquer le skill **bootstrap-vibe-coding** (par `@` ou en demandant explicitement de bootstraper un projet vibe-coding Next.js).

L’agent va :

1. Vous demander le **contexte projet** et quelles **règles AGENTS.md** garder / enlever / ajouter
2. Créer `US/`, `next-app/` (avec instructions d’init), `AGENTS.md`, `README.md`
3. **Ne pas** exécuter `pnpm`, `docker`, `git`, ni `shadcn init`

Vous lancez vous-même :

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
```

## Structure produite

```
.
├── US/                 # Stories : fonctionnel / technique / visuel
├── next-app/           # App Next (init shadcn par le dev)
├── AGENTS.md           # Règles agents (personnalisables)
└── README.md           # Commandes et contexte
```

## Contenu du skill

| Chemin | Description |
|--------|-------------|
| `.cursor/skills/bootstrap-vibe-coding/SKILL.md` | Workflow agent |
| `.cursor/skills/bootstrap-vibe-coding/templates/` | Modèles AGENTS, README, US, INIT |

## Licence

Usage interne / personnel selon votre organisation.
