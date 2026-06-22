<div align="center">

<img src="https://img.shields.io/badge/license-Apache%202.0-blue.svg" alt="License" />
<img src="https://img.shields.io/badge/status-early%20development-orange.svg" alt="Status" />
<img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome" />

# Kubix

**Backend observability for developers who don't want to guess.**

Kubix gives you a single dashboard to understand what's happening inside your backend — which APIs depend on what, which queries are slow, and how your schema has changed over time.

[Getting Started](#getting-started) · [Modules](#modules) · [Contributing](#contributing) · [Roadmap](#roadmap)

</div>

---

## The problem

You change an API endpoint. You don't know which services call it.
You deploy a migration. You don't know what it broke.
You have a slow query. You don't know why.

Most teams use 3–4 separate tools to answer these questions — and none of them talk to each other.

## What Kubix does

Kubix connects three pieces of your backend into one view:

| Module | What it shows |
|--------|--------------|
| **API Catalog** | Which service calls which, breaking change risk, dependency graph |
| **DB Performance** | Slow queries, unused indexes, visual EXPLAIN output |
| **Schema & Migration** | ERD, migration history, diff viewer between versions |

The key insight: *"I changed this API → it affects these queries → which requires this migration"* — that chain lives in one place, not three tabs.

## Getting started

**Requirements:** Docker, Docker Compose

```bash
git clone https://github.com/kubixhq/kubix.git
cd kubix
cp .env.example .env
docker compose up
```

Open `http://localhost:3000` — that's it.

### Connect your database

Edit `.env` and add your database connection:

```env
DB_HOST=localhost
DB_PORT=5432
DB_NAME=your_database
DB_USER=your_user
DB_PASSWORD=your_password
```

Restart with `docker compose restart` and Kubix will start collecting metrics.

## Modules

### Module 1 — API Catalog
> *Which service calls which, and what breaks if you change it*

- Auto-discovers APIs from OpenAPI/Swagger specs
- Builds a live dependency graph across services
- Flags breaking change risk before you deploy
- Supports REST (GraphQL coming soon)

### Module 2 — DB Performance
> *Slow queries, dead indexes, visual execution plans*

- Captures slow queries in real time
- Identifies unused and missing indexes
- Renders `EXPLAIN` output as a visual tree
- Supports PostgreSQL (MySQL coming soon)

### Module 3 — Schema & Migration
> *Your database history, made navigable*

- Generates an ERD from your live schema
- Tracks migration history (Flyway, Liquibase, Prisma supported)
- Shows a diff between any two schema versions
- Works without modifying your existing migration setup

## Repositories

Kubix is organized as a multi-repo project under the [`kubixhq`](https://github.com/kubixhq) organization:

| Repository | Description |
|------------|-------------|
| [`kubixhq/kubix`](https://github.com/kubixhq/kubix) | This repo — main entry point, docs, docker-compose |
| [`kubixhq/kubix-api`](https://github.com/kubixhq/kubix-api) | Spring Boot backend — analysis engine & REST API |
| [`kubixhq/kubix-dashboard`](https://github.com/kubixhq/kubix-dashboard) | Next.js frontend — all 3 module UIs |
| [`kubixhq/kubix-connector-postgres`](https://github.com/kubixhq/kubix-connector-postgres) | PostgreSQL connector |
| [`kubixhq/kubix-connector-mysql`](https://github.com/kubixhq/kubix-connector-mysql) | MySQL connector (coming soon) |
| [`kubixhq/kubix-docs`](https://github.com/kubixhq/kubix-docs) | Documentation site |

## Architecture

```
kubixhq/
├── kubix              ← you are here (start here)
├── kubix-api          ← ingestion + analysis engine + REST API
├── kubix-dashboard    ← Next.js, 3 module UIs
└── kubix-connector-* ← one repo per database / migration tool
```

Kubix runs entirely in your infrastructure. No data leaves your environment.

## Connectors

Kubix uses a plugin architecture so you can connect any database or migration tool:

- **Databases:** PostgreSQL ✅ · MySQL 🔜 · MongoDB 🔜
- **Migration tools:** Flyway ✅ · Liquibase ✅ · Prisma ✅ · Raw SQL 🔜
- **API formats:** OpenAPI 3.x ✅ · Swagger 2.x ✅ · GraphQL 🔜

Want a connector that doesn't exist yet? [Open an issue](https://github.com/kubixhq/kubix/issues) or see [Contributing](#contributing).

## Roadmap

- [x] Schema & Migration module (MVP)
- [ ] API Catalog module
- [ ] DB Performance module
- [ ] Cross-module correlation view
- [ ] Slack / PagerDuty alerts
- [ ] GitHub Actions integration

## Contributing

Kubix is in early development and contributions are very welcome.

Each repository has its own setup guide. To get the full stack running locally:

```bash
# Clone the main repo first
git clone https://github.com/kubixhq/kubix.git
cd kubix
docker compose -f docker-compose.dev.yml up
```

This pulls and runs all services together. To work on a specific component, clone that repo separately and follow its own `CONTRIBUTING.md`.

See [CONTRIBUTING.md](./CONTRIBUTING.md) for org-wide guidelines and how to build a connector.

Good first issues are tagged [`good first issue`](https://github.com/kubixhq/kubix/issues?q=is%3Aissue+label%3A%22good+first+issue%22) across all repos.

## License

Apache 2.0 — see [LICENSE](./LICENSE) for details.

---

<div align="center">
Built in public by <a href="https://github.com/kubixhq">kubixhq</a>
</div>
