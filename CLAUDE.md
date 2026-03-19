# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Bluebowl is a subscription-based food delivery service (twice-daily fresh meals). The repo is in early scaffolding — directory structures and architecture are defined but most implementation is pending.

## Repository Structure

```
bluebowl/
├── backend/          # Go API service
│   ├── cmd/api/      # Entry points
│   ├── internal/     # Domain packages (subscriptions, users, delivery, routing, orders, kitchen)
│   └── proto/        # Protocol Buffer definitions (gRPC)
├── web/              # Customer-facing Next.js frontend
├── dashboard/        # Admin/ops Next.js dashboard
└── docker-compose.yml
```

## Architecture

- **Backend**: Go service organized by domain. Uses gRPC (proto definitions in `backend/proto/`). Compiled binaries go to `backend/bin/`.
- **Web & Dashboard**: Separate Next.js apps (Turbo monorepo tooling expected based on `.gitignore`).
- **Infrastructure**: Docker Compose for local orchestration (file is currently empty).

## Backend Commands (Go)

Once `go.mod` exists:
```bash
# Run API
go run ./backend/cmd/api/...

# Build
go build -o backend/bin/api ./backend/cmd/api/...

# Test
go test ./backend/...

# Single package test
go test ./backend/internal/subscriptions/...
```

## Frontend Commands (Node/Next.js)

Once `package.json` exists in `web/` or `dashboard/`:
```bash
npm install
npm run dev    # development server
npm run build  # production build
npm run lint
```

## Environment

Copy `.env.example` to `.env` (never commit `.env`). Each component may have its own `.env`.

## Development Notes

- Backend internal packages under `backend/internal/` are not importable by external modules — keep domain logic there.
- Proto changes require regenerating Go stubs before building.
- Vendor directory (`backend/vendor/`) is gitignored; run `go mod vendor` if needed.
