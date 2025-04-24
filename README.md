# 🚀 n8n + Chatwoot + Ngrok (Dockerized Stack)

A lightweight, production-ready Docker stack featuring:
- [n8n](https://n8n.io): Open-source workflow automation
- [Chatwoot](https://www.chatwoot.com): Customer messaging platform
- [ngrok](https://ngrok.com): Public tunneling for easy access

---

## 📁 Folder Structure

```
project-root/
├── docker-compose.yml
├── .env
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
CW_FRONTEND_URL=http://localhost:3000
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

| Service   | Local URL              | Public (Ngrok) |
|-----------|------------------------|----------------|
| n8n       | http://localhost:5678  | check `docker logs ngrok-n8n` |
| Chatwoot  | http://localhost:3000  | check `docker logs ngrok-chatwoot` |

To view public URLs from ngrok:

```bash
docker logs ngrok-n8n
docker logs ngrok-chatwoot
```

---

## 🔐 Security Notes

- n8n uses basic auth (`admin / securepassword` by default)
- Avoid using weak passwords if exposed to the internet
- Optionally, add OAuth or Authelia in front for SSO

---

## 🧩 Future Enhancements

- [ ] Google Login for n8n
- [ ] Webhook integration: Chatwoot → n8n → LLM auto-reply
- [ ] WhatsApp or Email alerts via n8n flows

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