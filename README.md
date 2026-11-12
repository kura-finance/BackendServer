# Kura Backend

> **Archived — no longer hosted or maintained.**
> The Kura clients ([mobile-app](https://github.com/Kura-Finance/mobile-app), [WebClients](https://github.com/Kura-Finance/WebClients)) now run on-chain by default and do not need this backend. The hosted service has been shut down. This repository stays read-only for anyone who wants to self-host the optional backend features. Issues and pull requests are not accepted, and dependencies are no longer updated — review before you deploy.

Open-source backend API for **Kura** — a multi-asset finance application.

## Self-hosting after the archive

The backend never held wallet keys or signed transactions; the wallet works without it. What it adds is the off-chain feature set: TrackFi (Plaid, exchanges, DeBank), the Bridge.xyz fiat ramp, Dinari stocks, Stripe billing, referrals, the card waitlist and in-app account deletion.

- **Point the clients at your deployment.** Mobile: `EXPO_PUBLIC_API_BASE_URL`. Web: `NEXT_PUBLIC_BACKEND_URL`. Both are empty by default; setting them turns the backend features back on in the client.
- **Enable only the domains you have keys for** in [src/config/features.ts](src/config/features.ts).
- **Tiers depend on Stripe.** Web API access (`webTierGate`) and Treasuries (`requirePaidTier`) need tier Pro or Ultimate, and only the Stripe domain sets `User.tier`. Without Stripe every user stays Basic: web clients get 403 on most routes and Treasuries are unavailable on every client. Set tiers yourself or relax those two middlewares.
- **Use your own Privy app.** `PRIVY_APP_ID` / `PRIVY_APP_SECRET` must match the app the clients log in with.

Stack: Node.js 24, TypeScript, Express 5, Prisma, PostgreSQL. Deploy target: Google Cloud Run + Cloud SQL (optional).

[繁體中文](README.zh-TW.md) · [Contributing](CONTRIBUTING.md)

## License

[MIT](LICENSE) · Copyright © 2024–2026 Rick Weng

Third-party notices: [docs/legal/THIRD_PARTY_NOTICES.md](docs/legal/THIRD_PARTY_NOTICES.md)

## Documentation

Full index: **[docs/README.md](docs/README.md)**

| Doc | Description |
|-----|-------------|
| [docs/PRODUCT.md](docs/PRODUCT.md) | Product overview |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Domains, auth, data flows |
| [docs/API.md](docs/API.md) | HTTP mounts and access rules |
| [docs/ENVIRONMENT.md](docs/ENVIRONMENT.md) | Environment variables |
| [.env.example](.env.example) | Env template |
| [src/config/features.ts](src/config/features.ts) | Domain feature toggles |
| [docs/API_KEYS.md](docs/API_KEYS.md) | Partner keys vs free public endpoints |
| [docs/OPERATIONS.md](docs/OPERATIONS.md) | Local / Docker / Cloud Run |
| [docs/SECURITY.md](docs/SECURITY.md) | Security model |

## Quick start

Prerequisites: Node.js 24+, PostgreSQL.

```bash
cp .env.example .env.development
# Enable only the domains you need in src/config/features.ts
# Fill JWT_SECRET, ENCRYPTION_KEY, DB_*, and partner keys for enabled features

npm ci
npx prisma migrate deploy   # or: npx prisma migrate dev
npm run dev
```

Health: `GET http://localhost:8080/health` · Features: `GET http://localhost:8080/api/features`

## Scripts

| Command | Purpose |
|---------|---------|
| `npm run dev` | Dev server with reload (`ts-node-dev`) |
| `npm run build` | Compile TypeScript to `dist/` |
| `npm start` | Run compiled server |
| `npm run dbml` | Regenerate Prisma client / DBML |
