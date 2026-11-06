# Vulpes Oculus

[![Join the Vulpes Oculus Discord](https://cdn.prod.website-files.com/6257d5a7f8f5b9f7f8c4c4b/6257d5a7f8f5b9f7f8c4c4b_discord-logo.png)](https://discord.gg/FmsF4rmwqG)

[**Join the Vulpes Oculus Discord Community**](https://discord.gg/FmsF4rmwqG)

**Vulpes Oculus** is a browser-based roleplay platform: character sheets, wiki, calendar, events, races, professions, gods, creatures, rules, map, weather, healer records, and admin tools.

Stack: HTML / CSS / JavaScript frontend, Node.js proxy, MySQL storage, Discord OAuth and webhooks.

---

## Features

### Public site
- Home with overview
- News & Events
- In-game calendar
- Wiki & Library
- Races, Professions, Creatures, Gods
- Rules
- Map
- Character sheet
- Healer records
- Fantasy weather

### Admin
- Dashboard, tasks, projects
- Character management
- Player blacklist
- Activity log
- Role-based access via Discord

### Discord
- Login with Discord
- Webhooks for characters, weather, healer notes, moderation

---

## Quick start

### 1. Config
```bash
cp config.example.json config.json
# Fill in MySQL, Discord, webhooks
```

### 2. Frontend (local)
```bash
python3 -m http.server 8080
# Open http://localhost:8080
```

### 3. Proxy (local)
```bash
cd vulpesoculus-discord-proxy
npm install
export MYSQL_URL="mysql://user:pass@host:3306/vulpes_oculus"
export DISCORD_CLIENT_ID="..."
export DISCORD_CLIENT_SECRET="..."
export AUTH_SECRET="long_random_string_min_32_chars"
export PUBLIC_URL="http://localhost:3000"
export ALLOWED_RETURN_ORIGINS="http://localhost:8080"
npm start
```

Point the frontend at the proxy (browser console):
```js
localStorage.setItem("vulpesoculus_discord_proxy_url", "http://localhost:3000");
```

Or set `DEFAULT_PROXY` in `vulpesoculus-storage.js`.

---

## Deploy

| Part | Suggested host |
|------|----------------|
| Frontend | GitHub Pages, Netlify, Vercel |
| Proxy | Render, Railway, VPS |
| Database | MySQL / MariaDB (any provider) |

**Proxy (Render example)**  
- Root directory: `vulpesoculus-discord-proxy`
- Build: `npm install`
- Start: `npm start`
- Set environment variables from `config.example.json`
- Set `PUBLIC_URL` to the proxy URL

**Discord**  
- Create an application → OAuth2 redirect: `https://YOUR-PROXY/auth/callback`
- Scopes: `identify`, `guilds.members.read`
- Put Client ID, Secret, guild ID, role IDs, and webhooks into env vars

**Custom domain**  
- Frontend: e.g. `vulpes-oculus.example.com`
- Proxy: e.g. `api.vulpes-oculus.example.com`
- Update `PUBLIC_URL`, `ALLOWED_RETURN_ORIGINS`, Discord redirect, and frontend proxy URL

Full checklist: see **SETUP.md** and **TEMPLATE-README.md**.

---

## Project layout

```text
vulpes-oculus/
├── index.html
├── charakterbogen.html          # Character sheet
├── kalender.html, events.html, news.html
├── wiki.html, bibliothek.html
├── voelker.html, berufe.html, kreaturen.html, goetter.html
├── regeln.html, karte.html, heilerakte.html, wetter.html
├── admin/                       # Admin area
├── vulpesoculus-storage.js      # Frontend ↔ proxy
├── vulpesoculus-sync.js
├── vulpesoculus-auth.js
├── vulpesoculus-discord-proxy/  # Node proxy (MySQL + Discord)
├── mysql-schema.sql
├── config.example.json
├── SETUP.md
└── LICENSE
```

---

## Config & secrets

Copy `config.example.json` → `config.json` (never commit secrets).  
Required env vars for the proxy:

- `MYSQL_URL` or `MYSQL_HOST` + `MYSQL_USER` + `MYSQL_PASSWORD` + `MYSQL_DATABASE`
- `DISCORD_CLIENT_ID`, `DISCORD_CLIENT_SECRET`
- `AUTH_SECRET`
- `PUBLIC_URL`, `ALLOWED_RETURN_ORIGINS`

Optional: role IDs, weather/character/healer/mod webhooks.

---

## License

MIT – see [LICENSE](LICENSE).

Upstream structure inspired by [chronikenweb](https://github.com/XxleynaxX/chronikenweb) (Kiyasha). Vulpes Oculus is a separate project name and template.

---

## Description (short)

Vulpes Oculus – A browser-based RP hub: character sheets, wiki, calendar, events, races, gods, creatures, rules, weather, healer records, and admin tools. HTML/JS frontend, Node proxy, MySQL storage, Discord login & webhooks. Run your own instance with config, domain, and full setup guide included.
