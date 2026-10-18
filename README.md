# New API — Deployment

Deploys [QuantumNous/new-api](https://github.com/QuantumNous/new-api) (LLM gateway & API asset management) via Docker Compose.

Production instance: https://api.tokenhubmax.com

## Stack

- Image: `calciumion/new-api:latest`
- Storage: Docker volume `new-api-data` (SQLite by default; optional external MySQL/PostgreSQL/Redis via `.env`)
- Port: `3000` (container) → `${APP_PORT:-3000}` (host)

## Local usage

```bash
cp .env.example .env
# edit .env as needed
docker compose up -d
# open http://localhost:3000
```

## Deploy

GitHub Actions runs `.github/workflows/deploy.yml` on push to `main` or manual dispatch. Configure repository secrets/variables:

- Secrets: `SSH_HOST_NEWAPI`, `SSH_PRIVATE_KEY_NEWAPI`
- Variables: `SSH_USER_NEWAPI`, `SSH_PORT_NEWAPI`, `DEPLOY_PATH_NEWAPI`, `PRODUCTION_URL_NEWAPI`

## Notes

- `.env` is never committed; only `.env.example` is.
- To upgrade new-API, just re-run the workflow (pulls latest image) or `docker compose pull && docker compose up -d` on the server.
- Data persists in the `new-api-data` volume; do not remove it.
