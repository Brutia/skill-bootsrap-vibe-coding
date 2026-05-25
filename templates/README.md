# <!-- Nom du projet -->

<!-- Une ligne : objectif produit -->

## Contexte

<!-- Résumé fourni lors du bootstrap vibe-coding -->

## Structure du dépôt

| Dossier / fichier | Rôle |
|-------------------|------|
| `US/` | User stories (fonctionnel, technique, visuel) |
| `next-app/` | Application Next.js (shadcn, preset b0) |
| `AGENTS.md` | Règles pour les agents IA |

## Prérequis

- Node.js (version LTS recommandée)
- [pnpm](https://pnpm.io/)
<!-- Si base de données -->
- Docker et Docker Compose

## Démarrage

### 1. Initialiser l’application Next (shadcn)

À exécuter **une fois** par le développeur :

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
```

Voir aussi `next-app/INIT.md`.

### 2. Installer les dépendances et lancer le dev server

```bash
cd next-app
pnpm install
pnpm dev
```

L’application est en général disponible sur [http://localhost:3000](http://localhost:3000).

## Base de données

<!-- Supprimer cette section si pas de DB -->

### Démarrer PostgreSQL (Docker)

```bash
<!-- Exemple : adapter au compose du projet -->
docker compose -f docker-compose.yml up -d
```

### Variables d’environnement

Créer `next-app/.env` (ou `.env.local`) :

```env
DATABASE_URL="postgresql://USER:PASSWORD@localhost:5432/DB_NAME"
```

### Migrations Prisma

Après modification de `next-app/prisma/schema.prisma`, **le développeur** exécute :

```bash
cd next-app
pnpm db:migrate
```

Autres scripts utiles (si définis dans `package.json`) :

```bash
pnpm db:studio   # Prisma Studio
pnpm db:push     # push schema sans migration (dev uniquement, si configuré)
```

## User stories

Chaque story vit dans `US/US-XXX-<slug>/` avec :

- `fonctionnel.md` — besoin métier et critères d’acceptation
- `technique.md` — conception et tâches
- `visuel.md` — UI et composants

Voir `US/README.md`.

## Agents IA

Les règles de travail des agents sont dans [AGENTS.md](./AGENTS.md).
