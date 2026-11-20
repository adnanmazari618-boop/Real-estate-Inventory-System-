# Legal cleaned version — Umer Realtors Admin Panel

Date: 2026-11-20
Source: `umer-realtors-admin-panel__6.zip` (OrchardDesk / Umer Realtors)

## What was removed (illegal / against WhatsApp Terms)
- `server/whatsappBaileysService.ts` — deleted. Baileys is an unofficial WhatsApp Web client.
- `server/whatsappGroupExportService.ts` — deleted. Group message collection/export.
- `@whiskeysockets/baileys` — removed from `package.json`.
- Green-API functions in `server/whatsappWebhookService.ts` — deleted. Green-API is another unofficial WhatsApp Web gateway.
- `server.ts` routes for Baileys direct connect, Green-API, WhatsApp group export and the group `.txt` buffer — replaced with 410 Gone stubs that explain to use the official Meta WhatsApp Business API. The server still starts and the frontend still loads; those buttons now show a disabled message instead of connecting.
- Scraped data emptied: `data/whatsapp_group_messages.json` → `[]`, `data/whatsapp_logs.json` → `[]`, `data/whatsapp_buffer.txt` → empty.

## What was kept, unchanged
- Frontend (`src/`, `index.html`) — NOT changed, as requested.
- Inventory, requirements, config, Google Sheets / Drive sync, Gemini AI chat / parse-chat, and the official Meta WhatsApp Business API webhook (`/api/whatsapp/webhook`) — kept.
- `data/config.json`, `data/requirements.json`, `data/inventory.json` — kept as they were.

## What this does NOT fix (still needs attention before public use)
- Firestore rules are still public (`allow read: if true`). Lock them to authenticated admin users and use real Firebase Auth — that needs a frontend change, so it was NOT done here per the no-frontend-change instruction.
- Admin login in the frontend is still a hardcoded local check. Change it and use real Firebase Auth before sharing the link publicly.
- Official WhatsApp Business API costs apply per delivered message (Pakistan: Marketing $0.0473, Utility/Service $0.015, first 1,000 service replies/month free). Group messages are NOT available via the official API — listings from groups will no longer auto-update. Listings will update from direct 1-to-1 messages to the business number, manual entry, pasted chat, and Sheets/Drive sync.
- No LICENSE file was in the original zip, so ownership / reuse rights are still unconfirmed. Only use this if it is Umer's own app.

## Run
```
npm install
npm run build
NODE_ENV=production npm start
```
Set `GEMINI_API_KEY` in `.env` for AI parsing. Without it, AI features will fail but inventory / health check still work.
