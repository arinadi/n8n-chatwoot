# 🚀 n8n + Chatwoot + Nginx + Ngrok (Dockerized Stack)

A lightweight, production-ready Docker stack featuring:
- [n8n](https://n8n.io): Open-source workflow automation
- [Chatwoot](https://www.chatwoot.com): Customer messaging platform
- [Nginx](https://nginx.org): Reverse proxy to unify endpoints
- [ngrok](https://ngrok.com): Public tunneling for easy access

---

## 📁 Folder Structure

```
project-root/
├── docker-compose.yml
├── .env
├── nginx/
│   └── nginx.conf
├── n8n/
│   └── data/
├── postgres/
│   └── data/
├── chatwoot/
│   ├── storage/
│   └── postgres/
├── redis/
│   └── data/
```

> All volumes are mounted for persistent data. Your workflows and conversations will remain safe across container restarts.

---

## ⚙️ Environment Variables

Create a `.env` file in the project root:

```env
# === n8n ===
N8N_USER=admin
N8N_PASSWORD=securepassword
N8N_DB_HOST=postgres
N8N_DB_PORT=5432
N8N_DB_NAME=n8n
N8N_DB_USER=n8n
N8N_DB_PASSWORD=n8npass

# === Chatwoot ===
CW_SECRET_KEY_BASE=superlongrandomkey123456
CW_FRONTEND_URL=http://localhost/chatwoot
CW_REDIS_URL=redis://chatwoot-redis:6379
CW_PG_HOST=chatwoot-postgres
CW_PG_USER=postgres
CW_PG_PASSWORD=chatwootpass
CW_PG_DB=chatwoot

# === Ngrok ===
NGROK_AUTHTOKEN=your-ngrok-authtoken
```

---

## ▶️ Running the Stack

1. Create the required folders as shown above.
2. Make sure your `.env` file is configured.
3. Launch the services:

```bash
docker-compose --env-file .env up -d
```

---

## 🌍 Access URLs

| Service   | Local URL (via Nginx)     | Public (Ngrok) |
|-----------|---------------------------|----------------|
| n8n       | http://localhost:8080/n8n/   | check `docker logs ngrok` |
| Chatwoot  | http://localhost:8080/chatwoot/ | check `docker logs ngrok` |

> Public URLs are served under one domain using subpaths: `/n8n/` and `/chatwoot/`

---

## 🔐 Security Notes

- n8n uses basic auth (`admin / securepassword` by default)
- Avoid using weak passwords if exposed to the internet
- Optionally, add OAuth or Authelia in front for SSO
- You can secure Nginx with HTTPS using a reverse proxy service or Let's Encrypt

---

## 🧩 Future Enhancements

- [ ] Google Login for n8n
- [ ] Webhook integration: Chatwoot → n8n → LLM auto-reply
- [ ] WhatsApp or Email alerts via n8n flows
- [ ] Add HTTPS support via Let's Encrypt
- [ ] Optional Auth middleware for Chatwoot

---

## 🧹 Cleanup

To stop and remove containers:

```bash
docker-compose down
docker volume prune
```

---

## 💾 Backup Tips

To safely backup everything:

- `n8n/data/`
- `postgres/data/`
- `chatwoot/postgres/`
- `chatwoot/storage/`
- `redis/data/`

---

## 📃 License

MIT — free to use and extend.