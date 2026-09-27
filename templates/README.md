# <!-- Nom du projet -->

<!-- Une ligne : objectif produit -->

## Contexte

<!-- Résumé fourni lors du bootstrap vibe-coding -->

## Structure du dépôt

| Dossier / fichier | Rôle |
|-------------------|------|
| `US/` | User stories (fonctionnel, technique, visuel) |
| `next-app/` | Application Next.js (shadcn, preset b0) — quasi vide avant init |
| `prisma-bootstrap/` | Fichiers Prisma de référence (copiés dans `next-app/` après init shadcn) |
| `docker-compose.yml` | PostgreSQL de test (dev local) |
| `AGENTS.md` | Règles pour les agents IA |

## Prérequis

- Node.js **20.19+** (requis par Prisma 7)
- TypeScript **5.4+**
- [pnpm](https://pnpm.io/)
<!-- Si base de données -->
- Docker et Docker Compose

## Démarrage

### 1. Initialiser l’application Next (shadcn)

`next-app/` ne contient que `INIT.md` après le bootstrap. À exécuter **une fois** par le développeur :

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
```

Au prompt nom du projet : **`.`** (dossier courant). Voir `next-app/INIT.md` si `INIT.md` bloque l’init.

### 2. Copier les fichiers Prisma (si base de données)

Après l’init shadcn réussie, depuis `next-app/` :

```bash
cp -r ../prisma-bootstrap/prisma .
cp ../prisma-bootstrap/prisma.config.ts .
cp -r ../prisma-bootstrap/lib .
cp ../prisma-bootstrap/.env.example .
mv ../prisma-bootstrap/PRISMA.md .
```

Puis suivre `next-app/PRISMA.md` pour installer les deps Prisma.

### 3. Installer les dépendances et lancer le dev server

```bash
cd next-app
pnpm install
pnpm dev
```

L’application est en général disponible sur [http://localhost:3000](http://localhost:3000).

## Base de données

<!-- Supprimer cette section si pas de DB -->

Stack par défaut : **Prisma ORM 7** + PostgreSQL via Docker Compose. Les fichiers de référence sont dans `prisma-bootstrap/` ; après init shadcn, copier dans `next-app/` et suivre `next-app/PRISMA.md`.

### 1. Démarrer PostgreSQL (Docker)

À la racine du dépôt :

```bash
cp .env.example .env
docker compose up -d
```

Vérifier que le conteneur est prêt :

```bash
docker compose ps
# ou
docker compose logs postgres
```

Le healthcheck attend que PostgreSQL accepte les connexions avant de considérer le service comme démarré.

### 2. Variables d’environnement

Copier les exemples et adapter si besoin :

```bash
cp .env.example .env                    # variables Docker (racine)
cp next-app/.env.example next-app/.env  # DATABASE_URL pour l’app
```

Valeurs par défaut (dev local) :

| Variable | Valeur |
|----------|--------|
| `POSTGRES_USER` | `{{POSTGRES_USER}}` |
| `POSTGRES_PASSWORD` | `{{POSTGRES_PASSWORD}}` |
| `POSTGRES_DB` | `{{POSTGRES_DB}}` |
| `POSTGRES_PORT` | `5432` |

`next-app/.env` :

```env
DATABASE_URL="postgresql://{{POSTGRES_USER}}:{{POSTGRES_PASSWORD}}@localhost:5432/{{POSTGRES_DB}}"
```

### 3. Configurer Prisma 7 (développeur)

Les fichiers ont été copiés depuis `prisma-bootstrap/` (étape Démarrage §2). Installer les deps et adapter si besoin — voir `next-app/PRISMA.md` :

```bash
cd next-app
pnpm add @prisma/client @prisma/adapter-pg dotenv pg
pnpm add -D prisma tsx @types/pg
```

### 4. Migrations Prisma

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

### Commandes Docker utiles

```bash
docker compose up -d      # démarrer PostgreSQL en arrière-plan
docker compose stop       # arrêter sans supprimer les données
docker compose down       # arrêter et supprimer le conteneur
docker compose down -v    # arrêter et supprimer les données (reset complet)
```

## User stories

Chaque story vit dans `US/US-XXX-<slug>/` avec :

- `fonctionnel.md` — besoin métier et critères d’acceptation
- `technique.md` — conception et tâches
- `visuel.md` — UI et composants

Voir `US/README.md`.

## Agents IA

Les règles de travail des agents sont dans [AGENTS.md](./AGENTS.md).
