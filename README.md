# 🧙‍♂️ Lordofwizard’s Self-Hosted Server (Docker + Nginx)

This repository contains a fully containerized infrastructure for self-hosting various services using **Docker**, **Docker Compose**, and **Nginx** as a reverse proxy. SSL is handled via **Certbot (Let's Encrypt)**, and containers are auto-updated using **Watchtower**.

---

## 🧱 Services Included

### 📚 Content Management
- **Zola** – Static blog
- **Gitea** – Self-hosted Git
- **Audiobookshelf** – Audiobook server

### 🧠 Utility
- **Crafty** – Minecraft server panel
- **qBittorrent-nox** – Torrent client
- **Portainer** – Docker web UI (optional)
- **Watchtower** – Automatic container updates

### 🎬 Media Automation (`-arr` stack)
- **Radarr** – Movies
- **Sonarr** – TV shows
- **Prowlarr** – Indexer manager
- **Lidarr** – Music
- **Readarr** – eBooks & audiobooks
- **Bazarr** – Subtitles
- **Whisparr** – Adult content (optional)

---

## 🏗️ Directory Structure

```text
├── .env
├── docker-compose.yml
├── nginx/
│   ├── nginx.conf
│   └── conf.d/
│       ├── blog.conf
│       ├── gitea.conf
│       ├── crafty.conf
│       ├── torrent.conf
│       └── media/
│           ├── radarr.conf
│           ├── sonarr.conf
│           ├── ...
├── certbot/
├── blog/
├── gitea/
├── crafty/
├── qbittorrent/
├── audiobook-shelf/
├── portainer/
├── radarr-sonarr-stack/
│   ├── docker-compose.yml
│   └── config/
└── media/
    ├── downloads/
    └── library/
````

---

## 🔧 Prerequisites

* Ubuntu/Debian server with `docker` and `docker-compose` installed
* Domain pointing to your server (e.g. `*.lordofwizard.com`)
* Open ports: **80**, **443**

---

## ⚙️ Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/lordofwizard/selfhosted.git
cd selfhosted
```

### 2. Populate `.env`

```dotenv
DOMAIN=lordofwizard.com
EMAIL=admin@lordofwizard.com
TZ=Asia/Kolkata
```

### 3. Create Docker network

```bash
docker network create web
```

### 4. Launch Nginx + Certbot

```bash
cd nginx
docker compose up -d
```

---

## 🐳 Deploying Services

Each service has its own folder with a `docker-compose.yml`.

```bash
cd blog
docker compose up -d

cd gitea
docker compose up -d

cd radarr-sonarr-stack
docker compose up -d
```

---

## 🔐 SSL Setup

Issue SSL certificates with Certbot (once per domain/subdomain):

```bash
sudo certbot --nginx \
  -d blog.lordofwizard.com \
  -d git.lordofwizard.com \
  -d radarr.lordofwizard.com \
  -d sonarr.lordofwizard.com \
  -d ... (put all urls that you want to be authenticated with certbot)
```

Renewal runs automatically via systemd timers.

---

## 🔁 Auto Updates with Watchtower

Watchtower monitors running containers and updates them automatically when new versions are available.

### Add to `docker-compose.yml`:

```yaml
  watchtower:
    image: containrrr/watchtower
    container_name: watchtower
    restart: unless-stopped
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - TZ=${TZ}
```

That's it! Watchtower will check every 24 hours.

---

## ✅ Access URLs

* [https://blog.lordofwizard.com](https://blog.lordofwizard.com)
* [https://git.lordofwizard.com](https://git.lordofwizard.com)
* [https://torrent.lordofwizard.com](https://torrent.lordofwizard.com)
* [https://radarr.lordofwizard.com](https://radarr.lordofwizard.com)
* [https://sonarr.lordofwizard.com](https://sonarr.lordofwizard.com)
* [https://prowlarr.lordofwizard.com](https://prowlarr.lordofwizard.com)
* [https://lidarr.lordofwizard.com](https://lidarr.lordofwizard.com)
* [https://readarr.lordofwizard.com](https://readarr.lordofwizard.com)
* [https://bazarr.lordofwizard.com](https://bazarr.lordofwizard.com)
* [https://whisparr.lordofwizard.com](https://whisparr.lordofwizard.com) *(optional uk cauzse it's 18+)*

---

## 📦 Backup Strategy

1. Backup all `config/` directories per service
2. Backup Nginx configs and certs (`/etc/nginx/`, `/etc/letsencrypt/`)
3. Backup Docker volumes (or use bind mounts for easier sync)

---

## 🙌 Author

Maintained by **lordofwizard** 🧙‍♂️
Powered by open-source and the self-hosting community.

---


