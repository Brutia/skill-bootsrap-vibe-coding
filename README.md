# Skill — bootstrap vibe-coding (Next.js)

Skill Cursor pour initialiser rapidement un dépôt **vibe-coding** autour de Next.js + shadcn (preset b0), **Prisma 7** et la convention Next.js **proxy**.

## Installation

Copier le skill dans vos skills personnels :

```bash
mkdir -p ~/.cursor/skills
cp -r . ~/.cursor/skills/bootstrap-vibe-coding/
```

Ou utiliser ce dépôt comme skill de projet : laisser le skill à la racine d’un repo dédié aux skills.

## Utilisation

Dans Cursor, invoquer le skill **bootstrap-vibe-coding** (par `@` ou en demandant explicitement de bootstraper un projet vibe-coding Next.js).

L’agent va :

1. Vous demander le **contexte projet** et quelles **règles AGENTS.md** garder / enlever / ajouter
2. Créer `US/`, `next-app/` (avec instructions d’init), `AGENTS.md`, `README.md`
3. Si base de données : `docker-compose.yml`, `.env.example`, modèles Prisma 7 (`prisma.config.ts`, `schema.prisma`, `lib/prisma.ts`, `PRISMA.md`)
4. **Ne pas** exécuter `pnpm`, `docker`, `git`, ni `shadcn init`

Vous lancez vous-même :

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
```

## Stack par défaut

| Composant | Version / convention |
|-----------|----------------------|
| Prisma | **ORM 7** (`prisma-client`, `@prisma/adapter-pg`) |
| Base de données dev | **PostgreSQL** via `docker-compose.yml` |
| Node.js | 20.19+ |
| Next.js routing | `proxy.ts` (pas `middleware.ts`) |
| Auth | next-auth v4 |

## Structure produite

```
.
├── US/                 # Stories : fonctionnel / technique / visuel
├── docker-compose.yml  # PostgreSQL de test (si DB)
├── .env.example        # Variables Docker (si DB)
├── next-app/           # App Next (init shadcn par le dev)
│   ├── INIT.md         # Instructions shadcn + proxy
│   ├── PRISMA.md       # Instructions Prisma 7 (si DB)
│   ├── .env.example    # DATABASE_URL (si DB)
│   ├── prisma.config.ts
│   ├── prisma/schema.prisma
│   └── lib/prisma.ts
├── AGENTS.md           # Règles agents (personnalisables)
└── README.md           # Commandes et contexte
```

## Contenu du skill

| Chemin | Description |
|--------|-------------|
| `SKILL.md` | Workflow agent |
| `templates/` | Modèles AGENTS, README, US, INIT, Prisma 7, docker-compose |

## Licence

Usage interne / personnel selon votre organisation.
