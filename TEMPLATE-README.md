# Vulpes Oculus – Template for your own instance

Technical base adapted from [XxleynaxX/chronikenweb](https://github.com/XxleynaxX/chronikenweb), rebranded as **Vulpes Oculus**.

This template helps you run your **own** Vulpes Oculus site (frontend + proxy + MySQL + Discord).

---

## Quick checklist

- [ ] Template extracted
- [ ] `config.example.json` copied to `config.json` and filled in
- [ ] Own MySQL database created
- [ ] Discord application created (Client ID + Secret)
- [ ] OAuth redirect: `https://YOUR-PROXY/auth/callback`
- [ ] Discord role IDs noted (Admin, Library, Healer)
- [ ] Webhooks created
- [ ] Proxy deployed + environment variables set
- [ ] `PUBLIC_URL` and `ALLOWED_RETURN_ORIGINS` correct
- [ ] Proxy URL set in `vulpesoculus-storage.js`
- [ ] Frontend reachable
- [ ] Discord login tested

---

## What you need

| Part | Examples |
|------|----------|
| MySQL | PlanetScale, Railway, Aiven, own server … |
| Proxy host | Render.com, Railway, VPS |
| Frontend host | GitHub Pages, Netlify, Vercel, own server |
| Discord | Own application + server + roles + webhooks |

Everyone needs their **own** MySQL, proxy, and Discord app.

---

## Setup

### 1. Config
```bash
cp config.example.json config.json
# edit config.json
```

### 2. MySQL
The proxy creates the table on startup (`mysql-schema.sql`).

### 3. Discord application
1. https://discord.com/developers/applications → New Application  
2. OAuth2 redirect: `https://YOUR-PROXY.onrender.com/auth/callback`  
3. Scopes: `identify` + `guilds.members.read`  
4. Put Client ID + Secret into env vars  

### 4. Roles & webhooks
Put role IDs and webhook URLs into config / env vars.

### 5. Deploy proxy (Render example)
- Root directory: `vulpesoculus-discord-proxy`  
- Build: `npm install`  
- Start: `npm start`  
- Set all env vars from `config.json`  
- `PUBLIC_URL` = your proxy URL  

### 6. Point frontend at proxy
In `vulpesoculus-storage.js`:
```js
const DEFAULT_PROXY = "https://your-proxy.onrender.com";
```
Or in the browser:
```js
localStorage.setItem("vulpesoculus_discord_proxy_url", "https://your-proxy.onrender.com");
```

---

## Run locally (terminal)

### Frontend
```bash
python3 -m http.server 8080
# → http://localhost:8080
```

Or:
```bash
npx serve .
php -S localhost:8080
```

### Proxy (backend)
```bash
cd vulpesoculus-discord-proxy
npm install

export MYSQL_URL="mysql://user:pass@host:3306/database"
export DISCORD_CLIENT_ID="your_client_id"
export DISCORD_CLIENT_SECRET="your_secret"
export AUTH_SECRET="long_random_string_min_32_chars"
export PUBLIC_URL="http://localhost:3000"
export ALLOWED_RETURN_ORIGINS="http://localhost:8080,http://127.0.0.1:8080"

npm start
# → http://localhost:3000
```

Point frontend at local proxy (browser F12 → Console):
```js
localStorage.setItem("vulpesoculus_discord_proxy_url", "http://localhost:3000");
```

---

## Custom domain

| Purpose | Example |
|---------|---------|
| Frontend | vulpes-oculus.example.com |
| Proxy | api.vulpes-oculus.example.com |

### Proxy (Render)
1. Settings → Custom Domains → add domain  
2. CNAME at DNS provider: `api` → `xxx.onrender.com`  
3. Update env:
```env
PUBLIC_URL=https://api.vulpes-oculus.example.com
ALLOWED_RETURN_ORIGINS=https://vulpes-oculus.example.com,https://www.vulpes-oculus.example.com
```

### Frontend
GitHub Pages / Netlify / Vercel → add custom domain + DNS.

### Discord OAuth
```
https://api.vulpes-oculus.example.com/auth/callback
```

### Frontend proxy URL
```js
const DEFAULT_PROXY = "https://api.vulpes-oculus.example.com";
```

---

## Required environment variables

- `MYSQL_URL` **or** `MYSQL_HOST` + `MYSQL_USER` + `MYSQL_PASSWORD` + `MYSQL_DATABASE`
- `DISCORD_CLIENT_ID`
- `DISCORD_CLIENT_SECRET`
- `AUTH_SECRET` (min. 32 random characters)
- `PUBLIC_URL`
- `ALLOWED_RETURN_ORIGINS`

**Recommended:** `DISCORD_GUILD_ID`, `ADMIN_ROLE_IDS`, `LIB_ROLE_IDS`, `HEALER_ROLE_IDS`, webhooks

---

## Security

- Never commit `config.json` / `.env` (see `.gitignore`)
- Do not reuse foreign tokens, guild IDs, or webhooks
- Keep `ALLOWED_RETURN_ORIGINS` limited to your real domains

---

## Project name

**Vulpes Oculus**

Stack: HTML / CSS / JS + Node proxy + MySQL + Discord.
