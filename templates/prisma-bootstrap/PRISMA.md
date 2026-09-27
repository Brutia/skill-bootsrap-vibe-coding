# Prisma 7 — configuration `next-app`

Ce guide décrit la stack base de données par défaut : **Prisma ORM 7** + PostgreSQL.

L’agent **ne lance pas** ces commandes à votre place.

> Ce fichier est copié dans `next-app/` après l’init shadcn (voir [INIT.md](../next-app/INIT.md)). Les commandes ci-dessous s’exécutent depuis `next-app/`.

## Prérequis

- Node.js **20.19+**
- TypeScript **5.4+**
- Init shadcn terminée dans `next-app/` (voir `INIT.md`)
- Fichiers Prisma copiés depuis `prisma-bootstrap/` (voir `INIT.md` § Après l’init)

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
pnpm add @prisma/client @prisma/adapter-pg dotenv pg
pnpm add -D prisma tsx @types/pg
```

## 2. Fichiers de référence

Les fichiers suivants ont été copiés depuis `prisma-bootstrap/` lors du bootstrap. Vérifier qu’ils sont présents et les adapter si besoin :

| Fichier | Rôle |
|---------|------|
| `prisma.config.ts` | URL de connexion (`env("DATABASE_URL")`), chemin des migrations |
| `prisma/schema.prisma` | Modèles ; **pas de `url`** dans le bloc `datasource` |
| `lib/prisma.ts` | Singleton `PrismaClient` avec adaptateur `@prisma/adapter-pg` |

Si un fichier manque, le recopier depuis `../prisma-bootstrap/` ou lancer :

```bash
pnpm dlx prisma init --output ../app/generated/prisma
```

puis réaligner sur les modèles de `prisma-bootstrap/`.

### Points clés Prisma 7

- `provider = "prisma-client"` (pas `prisma-client-js`)
- `output = "../app/generated/prisma"` dans le generator
- Import : `from "../app/generated/prisma/client"` (pas `@prisma/client`)
- `import "dotenv/config"` en tête de `prisma.config.ts`
- Pas de propriété `engine` dans `prisma.config.ts`

## 3. Scripts `package.json`

Ajouter (ou adapter) dans `package.json` :

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

## 4. Première migration

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
