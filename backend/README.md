# SolarOS ERP Backend

This backend is designed around the modules already present in `index.html`.

## Stack

- Node.js + TypeScript
- Express API
- Prisma ORM
- PostgreSQL database
- JWT authentication with MFA-ready login flow
- Zod request validation

## Setup

### No Docker or Podman: SQLite Dev Mode

This is the lightest option for slow internet. It creates a local database file at `backend/prisma/dev.db`.

```powershell
cd backend
Copy-Item .env.sqlite.example .env
npm install
npm run prisma:generate:sqlite
npm run prisma:migrate:sqlite -- --name init
npm run seed
npm run dev
```

Use this mode for development and frontend integration. Use PostgreSQL before production.

### With Podman

On Windows, start your Podman machine first if needed:

```powershell
podman machine start
```

Then run PostgreSQL with compose:

```powershell
cd backend
Copy-Item .env.example .env
podman compose up -d
npm install
npm run prisma:generate
npm run prisma:migrate -- --name init
npm run seed
npm run dev
```

If your Podman install does not include `podman compose`, use a direct container command:

```powershell
podman volume create solaros_erp_pgdata
podman run -d --name solaros-erp-postgres `
  -e POSTGRES_USER=postgres `
  -e POSTGRES_PASSWORD=postgres `
  -e POSTGRES_DB=solaros_erp `
  -p 5432:5432 `
  -v solaros_erp_pgdata:/var/lib/postgresql/data `
  postgres:16-alpine
```

Then continue:

```powershell
npm install
npm run prisma:generate
npm run prisma:migrate -- --name init
npm run seed
npm run dev
```

### With Docker

```bash
cd backend
copy .env.example .env
docker compose up -d
npm install
npm run prisma:generate
npm run prisma:migrate -- --name init
npm run seed
npm run dev
```

The API will run on `http://localhost:4000`.

Seeded login:

```text
Email: admin@solaros.in
Password: password123
MFA code: 123456
```

## Main API Areas

```text
GET    /api/health
POST   /api/auth/login
POST   /api/auth/mfa/verify
GET    /api/auth/me

GET    /api/dashboard/summary
GET    /api/dashboard/control-tower

CRUD   /api/masters/items
CRUD   /api/masters/vendors
CRUD   /api/masters/customers
CRUD   /api/masters/boms

CRUD   /api/procurement/purchase-requisitions
CRUD   /api/procurement/rfqs
CRUD   /api/procurement/purchase-orders
POST   /api/procurement/purchase-orders/:id/approve
CRUD   /api/procurement/goods-receipts
CRUD   /api/procurement/vendor-invoices
CRUD   /api/procurement/ap-payments

CRUD   /api/inventory/stock-batches
CRUD   /api/inventory/movements
GET    /api/inventory/stock-overview

CRUD   /api/quality/iqc
POST   /api/quality/iqc/:id/result
CRUD   /api/quality/ncr

CRUD   /api/production/orders
CRUD   /api/production/work-orders
CRUD   /api/production/oee

CRUD   /api/sales/orders
CRUD   /api/sales/dispatches
CRUD   /api/sales/ar-invoices
CRUD   /api/sales/ar-payments

GET    /api/finance/dashboard
CRUD   /api/finance/accounts
CRUD   /api/finance/journal-entries
POST   /api/finance/journal-entries/:id/post

CRUD   /api/hr/employees
CRUD   /api/hr/attendance
CRUD   /api/hr/leave-requests
CRUD   /api/hr/payroll-runs
CRUD   /api/hr/jobs

GET    /api/reports/:domain
POST   /api/ai/chat
CRUD   /api/settings
```

## Frontend Integration Shape

The current `index.html` is static and uses hardcoded values. Replace the table/KPI literals gradually by calling these APIs after login:

1. Call `POST /api/auth/login`.
2. Call `POST /api/auth/mfa/verify` with code `123456`.
3. Store `accessToken`.
4. Send `Authorization: Bearer <accessToken>` on every `/api/*` call.
5. Start with dashboard, purchase orders, stock overview, IQC, sales orders, and employee directory because those already have clear screens.

## Production Notes

Before production, add HTTPS, real MFA delivery, database backups, row-level authorization, audit review screens, and accounting-period locks.
