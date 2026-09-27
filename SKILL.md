---
name: bootstrap-vibe-coding
description: >-
  Bootstrap a Next.js vibe-coding workspace with US stories, next-app (shadcn
  init by the developer), AGENTS.md, and README. Use when starting a new
  vibe-coded Next.js project, bootstrapping skill-bootstrap layout, or the user
  asks to initialize US/, next-app/, or project agent rules.
---

# Bootstrap vibe-coding (Next.js)

Scaffold a project root for vibe-coding: user stories, Next app placeholder, agent rules, and developer README. **Do not** run commands that modify the environment (install, copy, git, docker, shadcn init) — the developer runs those.

## Preconditions

- Target directory is the workspace root (or a path the user gives). If unclear, ask once where to scaffold.
- Read [templates/](templates/) only when generating files; customize from user answers below.

## Phase 1 — Collect inputs (mandatory)

Use **AskQuestion** when available; otherwise ask in chat. Do not scaffold until these are answered.

### 1.1 Project context

Ask the user to describe:

- Product goal and main users
- MVP scope (what is in / out for the first iteration)
- Technical constraints they already know (auth, DB, hosting, design system)
- Any existing docs or URLs to respect

Store this summary in `README.md` (section **Contexte**) and reference it in `AGENTS.md` (section **Contexte du projet**).

### 1.2 Agent rules — keep / remove / add

Present the **default rules** below as a checklist. For each block, ask: **garder**, **enlever**, or **modifier**. Then ask: **autres règles à ajouter ?** (free text).

Default rules (verbatim baseline for `AGENTS.md`):

#### MCP et documentation

- Utiliser le serveur MCP **user-context7** pour vérifier les versions de bibliothèques et les APIs avant d’implémenter du code dépendant d’un framework.
- Utiliser le serveur MCP **user-shadcn** pour concevoir ou étendre les composants UI selon les conventions shadcn.

#### Qualité du code

Toute modification doit laisser le projet dans un état valide : **`pnpm build`** et **`pnpm lint`** (depuis `next-app/`) doivent réussir.

Après des changements de code, lancer ces deux commandes pour vérifier qu’aucune régression n’a été introduite. Corriger les erreurs avant de considérer la tâche terminée.

#### Exécution des commandes

Ne pas exécuter les commandes qui **modifient** l’environnement ou le dépôt, notamment :

- installation ou mise à jour de dépendances (`pnpm install`, `pnpm add`, `npm install`, etc.)
- copie ou génération de fichiers (`cp`, `shadcn init`, migrations Prisma, etc.)
- opérations Docker, Git ou système qui altèrent l’état (`docker compose up`, `git commit`, etc.)

Les commandes de **vérification en lecture seule** sont autorisées (ex. `pnpm build`, `pnpm lint`, `pnpm tsc --noEmit`).

Laisser au développeur les opérations qui modifient l’environnement (install, migrations, déploiement, etc.).

#### Base de données (Prisma 7)

- Utiliser **Prisma ORM 7** par défaut (Node.js 20.19+, TypeScript 5.4+).
- Ne pas créer de migrations Prisma soi-même (pas de nouveau dossier sous `next-app/prisma/migrations/`, pas de `migration.sql` écrit à la main).
- Après modification de `schema.prisma`, laisser le développeur lancer `pnpm db:migrate` depuis `next-app/` pour que Prisma génère les migrations.
- Ne pas mettre `url` dans le bloc `datasource` de `schema.prisma` — l’URL est dans `prisma.config.ts`.
- Utiliser `provider = "prisma-client"` (pas `prisma-client-js`) et l’import depuis `app/generated/prisma/client` (pas `@prisma/client`).
- Instancier `PrismaClient` avec l’adaptateur `@prisma/adapter-pg` (voir [templates/prisma-bootstrap/lib/prisma.ts](templates/prisma-bootstrap/lib/prisma.ts)).

#### Next.js — proxy (pas middleware)

- Ne pas créer de fichier `middleware.ts` — la convention est dépréciée depuis Next.js 16.
- Utiliser `proxy.ts` à la racine de `next-app/` (ou dans `src/` si le projet utilise `src/`), avec une fonction exportée nommée `proxy` (pas `middleware`).
- Pour next-auth v4 : `export { default as proxy } from "next-auth/middleware"` dans `proxy.ts`.
- Référence : [Migration middleware → proxy](https://nextjs.org/docs/messages/middleware-to-proxy).

#### Authentification

- Rester sur **next-auth v4** uniquement (pas de migration vers la v5).

Adapt paths if the user’s layout differs; default app root is **`next-app/`**.

### 1.3 README extras

Ask:

- **Base de données** : oui / non. If yes (default stack): **Prisma 7** + PostgreSQL via **docker-compose** à la racine du dépôt.
- If yes, confirm or customize defaults: Postgres version (`16`), user (`app`), password (`app_dev`), database name (`app`), port (`5432`).
- **Autres services** (Redis, S3, etc.) for docker/README sections.

## Phase 2 — Create directory layout

At the target root, create:

```
./
├── US/
├── next-app/
├── AGENTS.md
└── README.md
```

If database is enabled (§1.3), also create at the **repository root**:

```
./
├── docker-compose.yml      # PostgreSQL de test (depuis templates/docker-compose.yml)
├── .env.example            # Variables Docker (POSTGRES_*)
└── prisma-bootstrap/       # Fichiers Prisma de référence (hors next-app, avant init shadcn)
    ├── PRISMA.md
    ├── prisma.config.ts
    ├── prisma/schema.prisma
    ├── lib/prisma.ts
    └── .env.example
```

Replace `{{POSTGRES_VERSION}}`, `{{POSTGRES_USER}}`, `{{POSTGRES_PASSWORD}}`, `{{POSTGRES_DB}}` in those files with values from §1.3 (defaults: `16`, `app`, `app_dev`, `app`).

### `US/` — user stories

For each story the user wants at bootstrap (or one example `US-001` if they prefer to fill later):

```
US/
├── README.md
└── US-XXX-<slug>/
    ├── fonctionnel.md
    ├── technique.md
    └── visuel.md
```

- Copy structure and headings from [templates/US/](templates/US/).
- **fonctionnel** : persona, besoin, critères d’acceptation, hors scope
- **technique** : API, modèle de données, contraintes, dépendances, tâches agent vs développeur
- **visuel** : maquettes, états UI, tokens/composants shadcn, responsive/a11y

If the user provides story titles only, create empty files with section headings from templates.

### `next-app/` — Next + shadcn (developer-initiated)

**Do not** run `pnpm dlx shadcn@latest init ...`.

1. Create `next-app/` with **only** [templates/next-app/INIT.md](templates/next-app/INIT.md) — no `package.json`, no `prisma/`, no `lib/` before shadcn init.
2. If database is enabled (§1.3), copy the reference files from [templates/prisma-bootstrap/](templates/prisma-bootstrap/) into `prisma-bootstrap/` at the repository root (not into `next-app/`). The developer copies them into `next-app/` **after** shadcn init (see INIT.md).
3. Tell the user to run from the **repository root**:

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
```

At the project name prompt, answer **`.`** (current directory). If `INIT.md` blocks an empty-directory check, follow the workaround in INIT.md (temporarily move INIT.md, init, restore).

4. After init, if database is enabled, the developer copies Prisma files from `prisma-bootstrap/` into `next-app/` and follows `PRISMA.md` (see INIT.md § Après l’init).
5. After they confirm init is done, they can open `next-app/` as the main coding root; agent work on app code stays under `next-app/` unless the user says otherwise.

### `AGENTS.md`

Generate from [templates/AGENTS.md](templates/AGENTS.md):

- Include only rule blocks the user kept (plus any they added).
- Inject **Contexte du projet** from §1.1.
- Keep tone imperative and concise; French is fine if the user works in French.

### `README.md`

Generate from [templates/README.md](templates/README.md):

- **Contexte** from §1.1
- **Structure du dépôt** (US, next-app, AGENTS.md)
- **Prérequis** (Node 20.19+, pnpm, Docker if DB)
- **Démarrage** : shadcn init (developer, `next-app/` quasi vide), puis copie `prisma-bootstrap/` → `next-app/` si DB, puis `pnpm install` / `pnpm dev` — list commands as copy-paste blocks **without running them**
- **Base de données** (if §1.3): section Docker Compose (démarrer/arrêter/réinitialiser), copie `.env.example` → `.env`, copie Prisma depuis `prisma-bootstrap/` après init shadcn, `DATABASE_URL` dans `next-app/.env`, Prisma 7 setup (voir `next-app/PRISMA.md` après copie), `pnpm db:migrate` depuis `next-app/`
- **User stories** : how to add `US-XXX-*` folders

## Phase 3 — Handoff checklist

Report to the user:

```
Bootstrap vibe-coding
- [ ] US/ (+ stories créées)
- [ ] next-app/INIT.md — init shadcn à lancer par vous (dossier quasi vide)
- [ ] docker-compose.yml + .env.example — base PostgreSQL de test (si base de données)
- [ ] prisma-bootstrap/ — fichiers Prisma de référence à copier dans next-app/ après init (si base de données)
- [ ] AGENTS.md (règles validées)
- [ ] README.md
```

Remind: agent must not run commands that modify the environment (install, copy, docker, git); run `pnpm build` and `pnpm lint` to verify changes; MCP context7 + shadcn for framework/UI work.

## Anti-patterns

- Running commands that modify the environment (`pnpm install`, `pnpm add`, `git commit`, `docker compose up`, `shadcn init`, etc.)
- Finishing a task without verifying `pnpm build` and `pnpm lint` pass
- Writing Prisma migration SQL or creating `prisma/migrations/*` manually
- Using Prisma < 7 patterns (`prisma-client-js`, `url` in `schema.prisma`, import from `@prisma/client`)
- Creating `middleware.ts` instead of `proxy.ts`
- Upgrading to Auth.js / next-auth v5 without explicit user request
- Skipping the rules questionnaire
- Copying `prisma/`, `lib/`, or other files into `next-app/` before shadcn init (causes `next-app/next-app/` or init failure)

## Templates

| File | Role |
|------|------|
| [templates/AGENTS.md](templates/AGENTS.md) | Default agent rules skeleton |
| [templates/README.md](templates/README.md) | Developer README skeleton |
| [templates/US/README.md](templates/US/README.md) | US folder conventions |
| [templates/US/story/fonctionnel.md](templates/US/story/fonctionnel.md) | Per-story template |
| [templates/US/story/technique.md](templates/US/story/technique.md) | Per-story template |
| [templates/US/story/visuel.md](templates/US/story/visuel.md) | Per-story template |
| [templates/next-app/INIT.md](templates/next-app/INIT.md) | Shadcn init instructions (seul fichier dans `next-app/` au bootstrap) |
| [templates/prisma-bootstrap/PRISMA.md](templates/prisma-bootstrap/PRISMA.md) | Prisma 7 setup (copié dans `next-app/` après init) |
| [templates/prisma-bootstrap/prisma.config.ts](templates/prisma-bootstrap/prisma.config.ts) | Prisma 7 config reference |
| [templates/prisma-bootstrap/prisma/schema.prisma](templates/prisma-bootstrap/prisma/schema.prisma) | Prisma 7 schema reference |
| [templates/prisma-bootstrap/lib/prisma.ts](templates/prisma-bootstrap/lib/prisma.ts) | PrismaClient singleton reference |
| [templates/prisma-bootstrap/.env.example](templates/prisma-bootstrap/.env.example) | `DATABASE_URL` pour l’app (copié dans `next-app/` après init) |
| [templates/docker-compose.yml](templates/docker-compose.yml) | PostgreSQL de test (dev) |
| [templates/.env.example](templates/.env.example) | Variables Docker à la racine |
