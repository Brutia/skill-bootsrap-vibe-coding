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

1. Copier `.env.example` vers `.env` / `.env.local` si fourni.
2. `pnpm install` puis `pnpm dev` dans `next-app/`.
3. Mettre à jour la section **Base de données** du README racine si vous ajoutez Prisma / Docker.

## Rappel agents

Voir [AGENTS.md](../AGENTS.md) à la racine du dépôt.
