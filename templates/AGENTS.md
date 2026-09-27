# Agent rules

Règles pour les agents Cursor travaillant sur ce dépôt vibe-coding.

## Contexte du projet

<!-- Rempli par l'agent lors du bootstrap à partir des réponses utilisateur -->

## MCP et documentation

- Utiliser le serveur MCP **user-context7** pour vérifier les versions de bibliothèques et les APIs avant d’implémenter du code dépendant d’un framework.
- Utiliser le serveur MCP **user-shadcn** pour concevoir ou étendre les composants UI selon les conventions shadcn.

## Qualité du code

Toute modification doit laisser le projet dans un état valide : **`pnpm build`** et **`pnpm lint`** (depuis `next-app/`) doivent réussir.

Après des changements de code, lancer ces deux commandes pour vérifier qu’aucune régression n’a été introduite. Corriger les erreurs avant de considérer la tâche terminée.

## Exécution des commandes

Ne pas exécuter les commandes qui **modifient** l’environnement ou le dépôt, notamment :

- installation ou mise à jour de dépendances (`pnpm install`, `pnpm add`, `npm install`, etc.)
- copie ou génération de fichiers (`cp`, `shadcn init`, migrations Prisma, etc.)
- opérations Docker, Git ou système qui altèrent l’état (`docker compose up`, `git commit`, etc.)

Les commandes de **vérification en lecture seule** sont autorisées (ex. `pnpm build`, `pnpm lint`, `pnpm tsc --noEmit`).

Laisser au développeur les opérations qui modifient l’environnement (install, migrations, déploiement, etc.).

## Base de données (Prisma 7)

- Utiliser **Prisma ORM 7** (Node.js 20.19+, TypeScript 5.4+).
- Ne pas créer de migrations Prisma soi-même (pas de nouveau dossier sous `next-app/prisma/migrations/`, pas de `migration.sql` écrit à la main).
- Après modification de `schema.prisma`, laisser le développeur lancer `pnpm db:migrate` depuis `next-app/` pour que Prisma génère les migrations.
- Ne pas mettre `url` dans le bloc `datasource` de `schema.prisma` — l’URL est dans `prisma.config.ts`.
- Utiliser `provider = "prisma-client"` (pas `prisma-client-js`) et importer depuis `app/generated/prisma/client` (pas `@prisma/client`).
- Instancier `PrismaClient` avec l’adaptateur `@prisma/adapter-pg` (voir `lib/prisma.ts`).

## Next.js — proxy (pas middleware)

- Ne pas créer de fichier `middleware.ts` — la convention est dépréciée depuis Next.js 16.
- Utiliser `proxy.ts` à la racine de `next-app/` (ou dans `src/` si le projet utilise `src/`), avec une fonction exportée nommée `proxy` (pas `middleware`).
- Pour next-auth v4 : `export { default as proxy } from "next-auth/middleware"` dans `proxy.ts`.
- Référence : [Migration middleware → proxy](https://nextjs.org/docs/messages/middleware-to-proxy).

## Authentification

- Rester sur **next-auth v4** uniquement (pas de migration vers la v5).

<!-- Sections optionnelles ajoutées par l'utilisateur lors du bootstrap -->
