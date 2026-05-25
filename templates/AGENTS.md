# Agent rules

Règles pour les agents Cursor travaillant sur ce dépôt vibe-coding.

## Contexte du projet

<!-- Rempli par l'agent lors du bootstrap à partir des réponses utilisateur -->

## MCP et documentation

- Utiliser le serveur MCP **user-context7** pour vérifier les versions de bibliothèques et les APIs avant d’implémenter du code dépendant d’un framework.
- Utiliser le serveur MCP **user-shadcn** pour concevoir ou étendre les composants UI selon les conventions shadcn.

## Exécution des commandes

Ne pas exécuter depuis le contexte agent les commandes de gestionnaire de paquets ou système, notamment :

- `pnpm`, `npm`, `yarn`, `tsx`
- `docker`, `git`
- et outils similaires

Laisser ces opérations au développeur ou à l’environnement dédié.

## Base de données (Prisma)

- Ne pas créer de migrations Prisma soi-même (pas de nouveau dossier sous `next-app/prisma/migrations/`, pas de `migration.sql` écrit à la main).
- Après modification de `schema.prisma`, laisser le développeur lancer `pnpm db:migrate` depuis `next-app/` pour que Prisma génère les migrations.

## Authentification

- Rester sur **next-auth v4** uniquement (pas de migration vers la v5).

<!-- Sections optionnelles ajoutées par l'utilisateur lors du bootstrap -->
