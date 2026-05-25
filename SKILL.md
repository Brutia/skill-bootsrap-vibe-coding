---
name: bootstrap-vibe-coding
description: >-
  Bootstrap a Next.js vibe-coding workspace with US stories, next-app (shadcn
  init by the developer), AGENTS.md, and README. Use when starting a new
  vibe-coded Next.js project, bootstrapping skill-bootstrap layout, or the user
  asks to initialize US/, next-app/, or project agent rules.
---

# Bootstrap vibe-coding (Next.js)

Scaffold a project root for vibe-coding: user stories, Next app placeholder, agent rules, and developer README. **Do not** run package managers, git, docker, or shadcn init — the developer runs those.

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

#### Exécution des commandes

Ne pas exécuter depuis le contexte agent les commandes de gestionnaire de paquets ou système, notamment :

- `pnpm`, `npm`, `yarn`, `tsx`
- `docker`, `git`
- et outils similaires

Laisser ces opérations au développeur ou à l’environnement dédié.

#### Base de données (Prisma)

- Ne pas créer de migrations Prisma soi-même (pas de nouveau dossier sous `next-app/prisma/migrations/`, pas de `migration.sql` écrit à la main).
- Après modification de `schema.prisma`, laisser le développeur lancer `pnpm db:migrate` depuis `next-app/` pour que Prisma génère les migrations.

#### Authentification

- Rester sur **next-auth v4** uniquement (pas de migration vers la v5).

Adapt paths if the user’s layout differs; default app root is **`next-app/`**.

### 1.3 README extras

Ask:

- **Base de données** : oui / non. If yes: Postgres version, docker compose file name, env var names for `DATABASE_URL`, and scripts (`db:migrate`, `db:studio`, etc.) once known from the user or from a future `package.json`.
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

1. Create `next-app/` with [templates/next-app/INIT.md](templates/next-app/INIT.md) (instructions only).
2. Tell the user to run from the **repository root**:

```bash
cd next-app
pnpm dlx shadcn@latest init --preset b0 --template next
```

3. After they confirm init is done, they can open `next-app/` as the main coding root; agent work on app code stays under `next-app/` unless the user says otherwise.

### `AGENTS.md`

Generate from [templates/AGENTS.md](templates/AGENTS.md):

- Include only rule blocks the user kept (plus any they added).
- Inject **Contexte du projet** from §1.1.
- Keep tone imperative and concise; French is fine if the user works in French.

### `README.md`

Generate from [templates/README.md](templates/README.md):

- **Contexte** from §1.1
- **Structure du dépôt** (US, next-app, AGENTS.md)
- **Prérequis** (Node, pnpm, Docker if DB)
- **Démarrage** : shadcn init command (developer), then `pnpm install` / `pnpm dev` in `next-app/` when applicable — list commands as copy-paste blocks **without running them**
- **Base de données** (if §1.3): docker command to start DB, env example, `pnpm db:migrate` from `next-app/`
- **User stories** : how to add `US-XXX-*` folders

## Phase 3 — Handoff checklist

Report to the user:

```
Bootstrap vibe-coding
- [ ] US/ (+ stories créées)
- [ ] next-app/INIT.md — init shadcn à lancer par vous
- [ ] AGENTS.md (règles validées)
- [ ] README.md
```

Remind: agent must not run pnpm/npm/docker/git; MCP context7 + shadcn for framework/UI work.

## Anti-patterns

- Running `pnpm`, `npm`, `yarn`, `git`, `docker`, or `shadcn init` from the agent
- Writing Prisma migration SQL or creating `prisma/migrations/*` manually
- Upgrading to Auth.js / next-auth v5 without explicit user request
- Skipping the rules questionnaire

## Templates

| File | Role |
|------|------|
| [templates/AGENTS.md](templates/AGENTS.md) | Default agent rules skeleton |
| [templates/README.md](templates/README.md) | Developer README skeleton |
| [templates/US/README.md](templates/US/README.md) | US folder conventions |
| [templates/US/story/fonctionnel.md](templates/US/story/fonctionnel.md) | Per-story template |
| [templates/US/story/technique.md](templates/US/story/technique.md) | Per-story template |
| [templates/US/story/visuel.md](templates/US/story/visuel.md) | Per-story template |
| [templates/next-app/INIT.md](templates/next-app/INIT.md) | Shadcn init instructions |
