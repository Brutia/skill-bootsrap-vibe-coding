# Initialisation `next-app`

Ce dossier accueille l’application Next.js. L’agent **ne lance pas** l’init à votre place.

## Prérequis

`next-app/` doit être **vide** avant l’init shadcn — seul ce fichier (`INIT.md`) est présent après le bootstrap.

> **Ne pas** y placer `prisma/`, `lib/`, `package.json` ou d’autres fichiers avant l’init : le CLI shadcn créerait un sous-dossier `next-app/next-app/` au lieu d’initialiser ce dossier.

## Commande (développeur)

Depuis la racine du dépôt :

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
```

Au prompt **« What is your project named? »** :

1. Répondre **`.`** (dossier courant) pour que l’app vive directement dans `next-app/`.
2. Si le CLI refuse car le dossier n’est pas vide (fichier `INIT.md` présent) :
   - **Option A** — déplacer temporairement ce fichier, init, puis le remettre :
     ```bash
     mv INIT.md ../INIT.md.bak
     pnpm dlx shadcn@latest init --preset b0 --template next
     # au prompt nom du projet : .
     mv ../INIT.md.bak INIT.md
     ```
   - **Option B** — depuis la racine du dépôt, si votre version du CLI le supporte :
     ```bash
     pnpm dlx shadcn@latest init --preset b0 --template next -c next-app -n .
     ```

Répondre aux autres prompts du CLI (TypeScript, etc.) selon votre contexte.

## Piège connu — pourquoi `next-app/` reste vide au bootstrap

Sans `package.json`, `shadcn init` appelle `createProject()` et crée un **nouveau** projet dans `cwd + nom_du_projet`. Le nom par défaut du template Next est `next-app`. Depuis `cd next-app`, cela produit `next-app/next-app/`.

Si le dossier courant contient déjà des fichiers (Prisma, `lib/`, etc.) et que vous entrez `.` comme nom, l’init **échoue** car le dossier n’est pas vide.

**Règle** : bootstrap → init shadcn dans un `next-app/` quasi vide → puis copier les fichiers Prisma depuis `prisma-bootstrap/`.

## Après l’init

1. **Si le projet utilise une base de données** — copier les fichiers de référence Prisma (depuis `next-app/`) :
   ```bash
   cp -r ../prisma-bootstrap/prisma .
   cp ../prisma-bootstrap/prisma.config.ts .
   cp -r ../prisma-bootstrap/lib .
   cp ../prisma-bootstrap/.env.example .
   mv ../prisma-bootstrap/PRISMA.md .
   ```
   Puis suivre [PRISMA.md](./PRISMA.md) pour installer les deps Prisma et lancer `pnpm db:migrate`.

2. Démarrer PostgreSQL avec `docker compose up -d` à la racine du dépôt (voir README).
3. `pnpm install` puis `pnpm dev` dans `next-app/`.
4. Mettre à jour la section **Base de données** du README racine si vous modifiez les identifiants Docker.

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
