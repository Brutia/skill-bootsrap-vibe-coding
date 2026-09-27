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
2. Créer `US/`, `next-app/` (uniquement `INIT.md`), `AGENTS.md`, `README.md`
3. Si base de données : `docker-compose.yml`, `.env.example`, `prisma-bootstrap/` (fichiers Prisma de référence — **pas** dans `next-app/` avant init shadcn)
4. **Ne pas** exécuter `pnpm`, `docker`, `git`, ni `shadcn init`

Vous lancez vous-même :

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
# nom du projet : .
```

Puis si base de données, copier `prisma-bootstrap/` → `next-app/` (voir `INIT.md`).

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
├── prisma-bootstrap/   # Fichiers Prisma de référence (si DB, avant init shadcn)
│   ├── PRISMA.md
│   ├── prisma.config.ts
│   ├── prisma/schema.prisma
│   ├── lib/prisma.ts
│   └── .env.example
├── next-app/           # App Next (init shadcn par le dev)
│   └── INIT.md         # Seul fichier avant init shadcn
├── AGENTS.md           # Règles agents (personnalisables)
└── README.md           # Commandes et contexte
```

Après init shadcn + copie Prisma : `next-app/` contient l’app Next + `PRISMA.md`, `prisma/`, `lib/`, etc.

## Contenu du skill

| Chemin | Description |
|--------|-------------|
| `SKILL.md` | Workflow agent |
| `templates/` | Modèles AGENTS, README, US, INIT, prisma-bootstrap, docker-compose |

## Licence

Usage interne / personnel selon votre organisation.
