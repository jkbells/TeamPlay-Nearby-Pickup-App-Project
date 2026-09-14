# TeamPlay-Nearby-Pickup-App-Project
Pickup sports organizer — create games, join with one tap, auto-balance teams by skill. Expo mobile app + Express API + Postgres/Drizzle monorepo. Early-stage scaffold — mobile UI works locally, backend not yet connected.


# PickUp

Organize casual pickup sports games without the group-chat chaos. Create a game, let people join with one tap, and get automatically balanced teams based on self-rated skill.

> **Status: early scaffold.** The mobile app UI is functional but currently stores data locally on-device only — it is not yet connected to the API server or database. See [Current state](#current-state) below before assuming any feature is production-ready.

## Stack

- **Monorepo:** pnpm workspaces, Node.js 24, TypeScript 5.9
- **Mobile app:** Expo + React Native, expo-router
- **API:** Express 5
- **Database:** PostgreSQL + Drizzle ORM
- **Validation / codegen:** Zod, Orval (generates typed API hooks + schemas from an OpenAPI spec)
- **Build:** esbuild

## Project structure

```
artifacts/
  pickup-mobile/       Expo mobile app (feed, create game, profile screens)
  api-server/          Express API (currently: health check route only)
  mockup-sandbox/      Design/prototyping sandbox
  pickup-project-deck/ Project deck / slides
lib/
  db/                  Drizzle schema + Postgres client (schema not yet defined)
  api-spec/            OpenAPI spec (source of truth for the API contract)
  api-zod/             Generated Zod schemas from the spec
  api-client-react/    Generated React hooks for calling the API
```

## Getting started

Requires pnpm and a Postgres database.

```bash
pnpm install

# set the DB connection string
export DATABASE_URL="postgres://..."

# run the API server (port 5000)
pnpm --filter @workspace/api-server run dev

# push the DB schema (once one is defined)
pnpm --filter @workspace/db run push

# regenerate API hooks/schemas after changing the OpenAPI spec
pnpm --filter @workspace/api-spec run codegen
```

To run the mobile app, see `artifacts/pickup-mobile/package.json` for its Expo scripts.

### Useful root scripts

| Command | What it does |
|---|---|
| `pnpm run typecheck` | Typecheck every package in the workspace |
| `pnpm run build` | Typecheck + build all packages |

## Current state

**Working:**
- Mobile app screens: game feed, create-game form, profile
- Local game state (create, join/leave, list) persisted via AsyncStorage on-device
- Basic API server skeleton with a health-check endpoint

**Not yet built:**
- Database schema (tables for users, games, game participants)
- Real API endpoints for creating/joining games
- Connecting the mobile app to the API instead of local storage
- Auth (sign up / log in)
- Skill-based team-balancing logic on the backend
- Deployment

## Planned data model

| Table | Purpose |
|---|---|
| `users` | id, name, email, password hash |
| `games` | id, creator, sport, location, start time, max players, status |
| `game_players` | join table: game, user, skill rating, assigned team |

## License

MIT
