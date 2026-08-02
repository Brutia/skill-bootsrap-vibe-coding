# Prisma 7 — configuration `next-app`

Ce guide décrit la stack base de données par défaut : **Prisma ORM 7** + PostgreSQL.

L’agent **ne lance pas** ces commandes à votre place.

## Prérequis

- Node.js **20.19+**
- TypeScript **5.4+**
- PostgreSQL accessible via Docker Compose (voir ci-dessous)

## 0. Démarrer PostgreSQL (Docker)

À la racine du dépôt :

```bash
cp ../.env.example ../.env
docker compose -f ../docker-compose.yml up -d
```

Puis configurer `next-app/.env` :

```bash
cp .env.example .env
```

Le `DATABASE_URL` doit correspondre aux identifiants du `docker-compose.yml` (par défaut : `postgresql://app:app_dev@localhost:5432/app`).

## 1. Installer les dépendances

```bash
cd next-app
pnpm add @prisma/client @prisma/adapter-pg dotenv pg
pnpm add -D prisma tsx @types/pg
```

## 2. Initialiser Prisma

```bash
pnpm dlx prisma init --output ../app/generated/prisma
```

Cela génère :

- `prisma/schema.prisma`
- `prisma.config.ts`
- `.env` (avec un `DATABASE_URL` local)

Si Docker Compose est déjà démarré, remplacer le `DATABASE_URL` généré par celui de `.env.example` (aligné sur le compose).

## 3. Fichiers de référence

Comparer et adapter les fichiers suivants (fournis comme modèles dans ce dépôt) :

| Fichier | Rôle |
|---------|------|
| `prisma.config.ts` | URL de connexion (`env("DATABASE_URL")`), chemin des migrations |
| `prisma/schema.prisma` | Modèles ; **pas de `url`** dans le bloc `datasource` |
| `lib/prisma.ts` | Singleton `PrismaClient` avec adaptateur `@prisma/adapter-pg` |

### Points clés Prisma 7

- `provider = "prisma-client"` (pas `prisma-client-js`)
- `output = "../app/generated/prisma"` dans le generator
- Import : `from "../app/generated/prisma/client"` (pas `@prisma/client`)
- `import "dotenv/config"` en tête de `prisma.config.ts`
- Pas de propriété `engine` dans `prisma.config.ts`

## 4. Scripts `package.json`

Ajouter (ou adapter) dans `next-app/package.json` :

```json
{
  "scripts": {
    "postinstall": "prisma generate",
    "db:migrate": "prisma migrate dev",
    "db:studio": "prisma studio",
    "db:push": "prisma db push"
  }
}
```

## 5. Première migration

```bash
pnpm db:migrate
```

L’agent peut modifier `schema.prisma` mais **ne crée pas** les fichiers sous `prisma/migrations/` — c’est au développeur de lancer `pnpm db:migrate`.

## Anti-patterns (à éviter)

```prisma
// ❌ url dans schema.prisma (Prisma 7)
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

```typescript
// ❌ ancien import
import { PrismaClient } from "@prisma/client";
```

```prisma
// ❌ ancien provider
generator client {
  provider = "prisma-client-js"
}
```

## Documentation

- [Prisma 7 + Next.js](https://www.prisma.io/docs/guides/frameworks/nextjs)
- [Prompt officiel Prisma v7](https://www.prisma.io/docs/ai/prompts/nextjs)
