# Deploy on Railway

1. Railway dashboard > New Project > Deploy from GitHub repo
   > `adnanmazari618-boop/Real-estate-Inventory-System-`
2. Railway will use `railway.json` in this repo:
   - Build: `npm run build`
   - Start: `NODE_ENV=production npm start`
   - Health check: `/health`
3. Set these Variables in Railway (do NOT commit them):
   - `GEMINI_API_KEY` = your Gemini API key (needed for AI parsing)
   - `NODE_ENV` = `production`
   - `PORT` is set automatically by Railway; the server already reads it.
4. Deploy. Railway gives a public URL like
   `https://real-estate-inventory-system-production.up.railway.app`
5. For the official Meta WhatsApp webhook, set the webhook URL to
   `<your Railway URL>/api/whatsapp/webhook`

## Why the first build failed

`npm install` fails on this project without `--legacy-peer-deps`
because of a peer-dependency conflict between Vite 8 / esbuild and
Tailwind. The `.npmrc` in this repo sets `legacy-peer-deps=true`,
so Railway's `npm install` now succeeds. This was tested locally:
`npm install --legacy-peer-deps` then `npm run build` succeeds.

## 2026-11-20 update: Nixpacks build failure

Railway's Nixpacks builder failed at:
`nix-env -if .nixpacks/nixpkgs-*.nix && nix-collect-garbage -d`
exit code 1 — before `npm install` even ran.

Fix: a `Dockerfile` was added at the repo root. Railway always
builds with a Dockerfile when it finds one, so Nixpacks is bypassed
completely. `railway.json` now sets `builder = "DOCKERFILE"`.
