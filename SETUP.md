# Vulpes Oculus – Setup (short)

Full guide: **TEMPLATE-README.md**

## Minimal steps

1. Copy `config.example.json` → `config.json` and fill it in  
2. Create MySQL database  
3. Discord application + redirect + roles + webhooks  
4. Deploy proxy (`vulpesoculus-discord-proxy`) + env vars  
5. Set proxy URL in `vulpesoculus-storage.js`  
6. Host frontend  
7. Test login  

## Run locally

```bash
# Frontend
python3 -m http.server 8080
# → http://localhost:8080

# Proxy
cd vulpesoculus-discord-proxy
npm install
export MYSQL_URL="mysql://user:pass@host:3306/db"
export DISCORD_CLIENT_ID="..."
export DISCORD_CLIENT_SECRET="..."
export AUTH_SECRET="long_random_min_32_chars"
export PUBLIC_URL="http://localhost:3000"
export ALLOWED_RETURN_ORIGINS="http://localhost:8080"
npm start
```

Browser console (F12):
```js
localStorage.setItem("vulpesoculus_discord_proxy_url", "http://localhost:3000");
```

## Custom domain

| Purpose | Example |
|---------|---------|
| Frontend | vulpes-oculus.example.com |
| Proxy | api.vulpes-oculus.example.com |

1. Add custom domain on Render / Netlify / GitHub Pages  
2. Set DNS (usually CNAME)  
3. Update `PUBLIC_URL` + `ALLOWED_RETURN_ORIGINS`  
4. Update Discord OAuth redirect  
5. Set proxy URL in the frontend  

## Checklist

- [ ] config.json filled in  
- [ ] MySQL reachable  
- [ ] Discord app + redirect  
- [ ] Roles + webhooks  
- [ ] Proxy running  
- [ ] PUBLIC_URL / ALLOWED_RETURN_ORIGINS correct  
- [ ] Frontend → your proxy  
- [ ] Login works  
- [ ] (Optional) Domain configured  

**Project:** Vulpes Oculus
