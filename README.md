## LordOfWizard's Docker Compose Self-Hosted Nexus Server

![Nexus Banner](https://placehold.co/1200x400/1e3a8a/white?text=Self-Hosted+Nexus+Server)  
*Your all-in-one self-hosted solution for media, automation, and productivity*

---

### 🚀 Introduction
This comprehensive Docker Compose stack brings together 10 powerful self-hosted services into a unified ecosystem. Designed for personal servers and homelabs, this configuration provides everything from media management and automation to file synchronization and password management - all accessible through a secure reverse proxy.

---

### 📦 Included Services
| Service | Port | Subdomain | Description |
|---------|------|-----------|-------------|
| **qBittorrent** | 6901 | `qbittorrent.lordofwizard.com` | Feature-rich torrent client |
| **Crafty Controller** | 6902 | `crafty.lordofwizard.com` | Minecraft server management |
| **Audiobookshelf** | 6903 | `audiobooks.lordofwizard.com` | Audiobook/podcast server |
| **Syncthing** | 6904 | `syncthing.lordofwizard.com` | Continuous file synchronization |
| **n8n** | 6905 | `n8n.lordofwizard.com` | Workflow automation platform |
| **Radarr** | 6906 | `radarr.lordofwizard.com` | Movie collection manager |
| **Sonarr** | 6907 | `sonarr.lordofwizard.com` | TV series manager |
| **Prowlarr** | 6908 | `prowlarr.lordofwizard.com` | Indexer manager |
| **Whisparr** | 6909 | `whisparr.lordofwizard.com` | Adult content media manager |
| **Vaultwarden** | 6910 | `vault.lordofwizard.com` | Bitwarden-compatible password manager |

---

### ⚙️ Prerequisites
1. **Docker Engine** (v20.10.0+)
2. **Docker Compose** (v2.0.0+)
3. Domain name with DNS access
4. Cloudflare account (for SSL certificates)
5. Basic terminal/Linux knowledge

---

### 🛠️ Setup Guide

#### 1. Clone Repository
```bash
git clone https://github.com/lordofwizard/selfhosted-nexus.git
cd selfhosted-nexus
```

#### 2. Directory Structure Setup
```bash
mkdir -p {qbittorrent,crafty,audiobookshelf,syncthing,n8n,radarr,sonarr,prowlarr,whisparr,bitwarden}/{config,data,downloads}
```

#### 3. SSL Certificate Setup
1. Generate Cloudflare Origin Certificate
2. Place certificates in `/etc/ssl/cloudflare/`:
   - `origin.crt`
   - `origin.key`

#### 4. Environment Configuration
Edit `.env` file:
```env
PUID=1000
PGID=1000
TZ=Asia/Kolkata  # Update to your timezone
DOMAIN=lordofwizard.com
```

#### 5. Start Services
```bash
docker compose up -d
```

#### 6. DNS Configuration
Create DNS records pointing to your server IP:
```
A     @         → your.server.ip
CNAME qbittorrent → lordofwizard.com
CNAME crafty      → lordofwizard.com
... (repeat for all services)
```

---

### 🔒 Reverse Proxy Configuration
The Nginx configuration provides:
- SSL termination with TLS 1.2/1.3
- HTTP/2 support
- Subdomain-based routing
- Optimized streaming support
- Security headers

**Key Features:**
- Cloudflare Origin Certificate integration
- WebSocket support (for n8n)
- Buffer optimization for media streaming
- Health checks for critical services

---

### 🌐 Accessing Services
Access services via: `https://<service-name>.lordofwizard.com`

| Service | Initial Credentials | Post-Setup Steps |
|---------|---------------------|------------------|
| **qBittorrent** | admin/adminadmin | Change default credentials |
| **Crafty** | Setup during first access | Configure Minecraft servers |
| **Audiobookshelf** | Create during setup | Add media libraries |
| **Syncthing** | None | Pair devices via UI |
| **n8n** | None | Create admin user |
| *Arr Suite | None | Configure download clients |

---

### 🔄 Maintenance
**Update Services:**
```bash
docker compose pull
docker compose up -d --force-recreate
```

**Backup Strategy:**
1. Regular backups of all `config` volumes
2. Database dumps for critical services
3. Cloud sync for `/data` directories

**Monitoring:**
```bash
docker compose logs -f --tail=50 [service_name]
```

---

### ⚠️ Troubleshooting
**Common Issues:**
1. **Port Conflicts**: Ensure ports 6881,8080 are free
2. **Permission Errors**: Verify PUID/PGID matches host user
3. **SSL Errors**: Confirm certificate paths are correct
4. **Service Unreachable**: Check firewall settings

**Debugging Steps:**
```bash
docker compose ps
docker inspect [container_id]
docker logs [container_id]
```

---

### 🔧 Customization Options
1. **Port Configuration**: Modify `ports` in docker-compose.yml
2. **Storage Paths**: Update volume mappings
3. **Service Selection**: Comment out unused services
4. **Security**: Add authentication via Nginx

---

### 🌟 Future Integrations
1. **Cloudflare Tunnel**: Secure external access
2. **Watchtower**: Automatic container updates
3. **Portainer**: Web-based Docker management
4. **Prometheus+ Grafana**: Monitoring dashboard

---

### 📜 License
This project is licensed under the **MIT License** - see the [LICENSE.md](LICENSE.md) file for details.

---

### ❤️ Support & Contribution
Found this useful? Consider:
- [Sponsoring LordOfWizard](https://github.com/sponsors/lordofwizard)
- Submitting PRs for improvements
- Reporting issues on GitHub

```bash
  _   _ _______  ___   _ ____  
 | \ | | ____\ \/ / | | / ___| 
 |  \| |  _|  \  /| | | \___ \ 
 | |\  | |___ /  \| |_| |___) |
 |_| \_|_____/_/\_\\___/|____/ 
```

