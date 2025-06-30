# 🌐 Self-Hosted SearXNG with Cloudflare Tunnel

This project sets up [SearXNG](https://github.com/searxng/searxng) — a powerful, privacy-respecting metasearch engine — using Docker and makes it securely accessible over the internet using a **Cloudflare Tunnel** and your custom domain.

> **Live demo:** [https://search.tyfsadik.org](https://search.tyfsadik.org)

---

## 🚀 Features

- 🔒 Access via HTTPS (Cloudflare secured)
- 🧩 Easy deployment via Docker Compose
- 🌐 Custom domain (e.g., `search.tyfsadik.org`)
- ♻️ Cloudflare Tunnel (no port forwarding or public IP required)
- 🎨 Ready for branding customization

---

## 🛠️ Prerequisites

- A domain name managed by [Cloudflare](https://cloudflare.com)
- Docker + Docker Compose
- `cloudflared` (Cloudflare Tunnel CLI)

---

## 📦 Installation

### 1. Clone this repository

```bash
git clone https://github.com/YOUR_USERNAME/searxng-selfhost.git
cd searxng-selfhost


### 2. Customize your domain

Edit the `Caddyfile`:

```caddy
search.tyfsadik.org {
    reverse_proxy searxng:8080
    header_up Connection "close"
    encode zstd gzip
    tls you@example.com
}
```

Change:

* `search.tyfsadik.org` to your own subdomain
* `you@example.com` to your email for TLS certs

---

## 🐳 Docker Setup

### 3. Start SearXNG and Caddy

```bash
docker compose up -d
```

This runs:

* `searxng`: the search engine backend
* `caddy`: reverse proxy with HTTPS support

---

## ☁️ Cloudflare Tunnel Setup

### 4. Install `cloudflared` (if not yet installed)

```bash
sudo apt install -y curl gnupg lsb-release
curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflared.list
sudo apt update
sudo apt install cloudflared
```

### 5. Authenticate with Cloudflare

```bash
cloudflared login
```

Log in and select your domain.

### 6. Create a tunnel

```bash
cloudflared tunnel create searxng-tunnel
```

### 7. Configure the tunnel

```yaml
# /etc/cloudflared/config.yml
tunnel: searxng-tunnel
credentials-file: /home/YOUR_USERNAME/.cloudflared/TUNNEL_ID.json

ingress:
  - hostname: search.tyfsadik.org
    service: http://localhost:8080
  - service: http_status:404
```

> Be sure to replace `YOUR_USERNAME` and `TUNNEL_ID.json` with your actual path.

### 8. Route your domain

```bash
cloudflared tunnel route dns searxng-tunnel search.tyfsadik.org
```

### 9. Run the tunnel

```bash
cloudflared tunnel run searxng-tunnel
```

---

## 🧪 Verify

Open your browser and go to:

```txt
https://search.tyfsadik.org
```

If everything is working, you should see your SearXNG homepage.

---

## 🎨 Customization

To change the search engine name or theme:

```bash
# Inside your mounted volume
nano searxng/settings.yml
```

Change:

```yaml
branding:
  name: "My Custom Search"
```

---

## 📂 File Structure

```
searxng-selfhost/
├── docker-compose.yml
├── Caddyfile
├── searxng/
│   └── settings.yml
├── .cloudflared/
│   └── <tunnel-credentials>.json
└── README.md
```

---

## 📜 License

This project is open-source, under the [MIT](https://en.wikipedia.org/wiki/MIT_License) license.

---

## 🤝 Credits

* [SearXNG](https://github.com/searxng/searxng)
* [Caddy Server](https://caddyserver.com)
* [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)

---

## 💬 Questions?

Feel free to open an issue or contact [@tyfsadik](https://github.com/tyfsadik).

```

---


```
