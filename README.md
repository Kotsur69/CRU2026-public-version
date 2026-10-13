# CRU2026 — Contract Registry

A modern rebuild of a legacy internal **contract registry** (CakePHP + MySQL 5.1) as a
Next.js 14 / PostgreSQL application, used in a large company's internal test phase.

> **Public, sanitized copy.** Company names, internal documentation, branding and deployment
> details were removed, and the git history wasn't carried over. UI copy is in Polish.

## What it does
- **Contracts, projects and risk registers** with search, pagination, configurable columns and
  detail views (34-field contract model, annexes, change history)
- **Contractors** directory and dictionaries migrated from the legacy system
- **Read authorization**: who can see which contract, enforced server-side (`lib/authz.ts`,
  `lib/contract-access.ts`)
- **Native login** (NextAuth Credentials + bcrypt), login throttling, password policy,
  route protection via middleware
- **Pluggable file storage** behind a `StorageAdapter` interface: a local adapter for dev, plus a
  read-only **SharePoint adapter (Microsoft Graph, `Sites.Selected`)** for production
- **Legacy migration**: a parser and importer for the old MySQL dump into the new Prisma schema
  (about 20k records), plus a verifier that cross-checks ~39k attachment keys against storage

## Stack
Next.js 14 (App Router) · TypeScript · Prisma · PostgreSQL · NextAuth · Tailwind + shadcn/ui ·
Microsoft Graph

## Run locally
```bash
cd nextjs_space
cp .env.example .env          # set DATABASE_URL, NEXTAUTH_SECRET, SEED_ADMIN_PASSWORD
docker compose up -d          # or point DATABASE_URL at your own Postgres
yarn install
yarn db:deploy                # apply migrations
yarn db:seed                  # dictionaries + admin user
yarn dev                      # http://localhost:3100
```
On Windows, `start.ps1` does all of the above using a portable Postgres.

## Layout
```
nextjs_space/
  app/            routes: login, app shell, registers, api
  components/     layout + per-register UI
  features/       contracts, projects, risk, account
  lib/            auth, authorization, storage adapters, contract domain logic
  prisma/         schema + migrations
  scripts/        seed, test accounts, legacy import/verification
```
