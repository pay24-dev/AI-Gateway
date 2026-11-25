# Local development and testing

This guide records commands verified from the checked-in `makefile`, `docker-compose.dev.yml`, `README.md`, and `web/package.json`. Re-check scripts and toolchain versions at the current commit.

## Toolchain

- Root Go module: see root `go.mod` (inspected baseline: Go 1.26.10).
- `relaykit/`: independent Go module (inspected baseline: Go 1.25.1).
- `tokenkit/`: independent Go module (inspected baseline: Go 1.26).
- Frontend: Bun (inspected `web/package.json`: Bun 1.4.4), React 19, TypeScript, Rsbuild.

## Local development

### Option A — Docker backend + local frontend

From the repository root:

```bash
make dev-api
```

This starts the development backend and its PostgreSQL/Redis dependencies using `docker-compose.dev.yml`.

In another terminal:

```bash
make dev-web
```

The frontend dev server is configured for port 5173 and proxies API requests to the backend on port 3000.

### Option B — local Go backend

Build the embedded frontend first:

```bash
cd web
bun install --frozen-lockfile
bun run build
cd ..
go run .
```

For a separate hot-reload frontend:

```bash
cd web
bun run dev -- --port 5173
```

See the root README and Makefile for the authoritative development instructions.

## Verification commands

### Go modules

From repository root:

```bash
make test
```

The Makefile runs tests for the root module, `relaykit/`, and `tokenkit/`.

For a direct focused test:

```bash
GOWORK=off go test ./path/to/package
```

For an independent module change:

```bash
cd relaykit
GOWORK=off go build ./...
GOWORK=off go test ./...
```

Use the equivalent commands inside `tokenkit/` when it changes. A root build does not prove the independent modules remain buildable.

### Frontend

From `web/`:

```bash
bun install --frozen-lockfile
bun check -b
bun run lint
bun run test
bun run build
```

Run the narrowest relevant test file first, then the required broader checks.

## Database verification

Any change affecting database behavior must follow root `AGENTS.md`: real SQLite, MySQL, and PostgreSQL verification is required. Schema changes also require fresh-database and upgrade-from-latest-release tests, and migrations must be run more than once to verify idempotency.

## Safety notes

- The development Compose file contains local-only example credentials. Never reuse them in production.
- Do not run `make reset-setup` against valuable data: it deletes setup records and administrator users/options from the selected development database.
- Do not run `docker compose down -v` unless deleting local development volumes is intended.
- Do not print `.env`, database DSNs, API keys, or secrets in logs or agent reports.
