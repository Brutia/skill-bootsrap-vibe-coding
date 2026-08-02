# Initialisation `next-app`

Ce dossier accueille l’application Next.js. L’agent **ne lance pas** l’init à votre place.

## Commande (développeur)

Depuis la racine du dépôt :

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
```

Répondre aux prompts du CLI (nom du projet, TypeScript, etc.) selon votre contexte.

## Après l’init

1. Si le projet utilise une base de données : démarrer PostgreSQL avec `docker compose up -d` à la racine du dépôt (voir README).
2. Copier `.env.example` vers `.env` / `.env.local` si fourni.
3. `pnpm install` puis `pnpm dev` dans `next-app/`.
4. Si le projet utilise une base de données, suivre [PRISMA.md](./PRISMA.md) pour configurer Prisma 7.
5. Mettre à jour la section **Base de données** du README racine si vous modifiez les identifiants Docker.

## Proxy Next.js (pas middleware)

Depuis Next.js 16, la convention `middleware.ts` est dépréciée au profit de `proxy.ts`.

- Créer `proxy.ts` (pas `middleware.ts`) à la racine de `next-app/` ou dans `src/`.
- Exporter une fonction nommée `proxy` (pas `middleware`).
- Pour next-auth v4 :

```typescript
// proxy.ts
export { default as proxy } from "next-auth/middleware";

export const config = {
  matcher: ["/dashboard/:path*"],
};
```

Migration d’un projet existant : `npx @next/codemod@canary middleware-to-proxy .`

Référence : [Migration middleware → proxy](https://nextjs.org/docs/messages/middleware-to-proxy)

## Rappel agents

Voir [AGENTS.md](../AGENTS.md) à la racine du dépôt.
