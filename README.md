# homeserver-docker

Docker Compose configuration for my home server, organized into logical service categories.

## Structure

```
├── docker-compose.yml      # Main index file (includes all categories)
├── compose.common.yml      # Shared templates (extends only, not included)
├── compose.core.yml        # Core infrastructure (nginx proxy, ddclient, endlessh)
├── compose.auth.yml        # Authelia authentication stack
├── compose.dashboards.yml  # Homer, Dashdot
├── compose.downloads.yml   # VPN/torrent stack, SABnzbd, Flaresolverr
├── compose.gaming.yml      # Game servers (Minecraft, Hytale, Satisfactory)
├── compose.immich.yml      # Immich photo management stack
├── compose.media.yml       # Media consumption (Plex, Jellyfin, Komga, Suwayomi)
├── compose.searxng.yml     # SearXNG search engine stack
├── compose.starr.yml       # *arr apps + Overseerr + Whisper ASR
├── compose.utilities.yml   # Utilities (Vaultwarden, Syncthing, ntfy)
├── compose.websites.yml    # Web hosting (custom sites)
├── hwaccel.transcoding.yml # Hardware acceleration for transcoding
├── hwaccel.ml.yml          # Hardware acceleration for ML
└── .env                    # Environment variables (not in git)
```

## Usage

```bash
# Start all services
docker compose up -d

# Pull updates and restart
docker compose pull && docker compose up -d

# View merged configuration
docker compose config

# Start specific category only (for testing)
docker compose -f compose.gaming.yml up -d
```

## Service Categories

| Category | Services |
|----------|----------|
| **Core** | nginxproxymanager, ddclient, endlessh |
| **Auth** | authelia, authelia_postgres, authelia_redis |
| **Dashboards** | homer, dashdot |
| **Downloads** | vpn-netns, gluetun, qbittorrent, qbit-port-sync, sabnzbd, flaresolverr |
| **Gaming** | minecraft-server, rlcraft-minecraft-server, hytale-server, satisfactory-server |
| **Immich** | immich-server, immich-machine-learning, immich-redis, immich-postgres |
| **Media** | plex, jellyfin, komga, komf, suwayomi, suwayomi_postgres |
| **SearXNG** | searxng, searxng_redis |
| **Starr** | radarr, sonarr, bazarr, prowlarr, recyclarr, overseerr, whisperasr |
| **Utilities** | vaultwarden, syncthing, ntfy |
| **Websites** | (custom web apps) |

## Shared Templates

`compose.common.yml` provides reusable service templates:
- `gpu-service` - NVIDIA GPU passthrough
- `common-service` - Standard restart policy and logging
- `hotio-service` - For Hotio images (PUID/PGID/TZ)
- `linuxserver-service` - For LinuxServer.io images

Usage in category files:
```yaml
services:
  radarr:
    extends:
      file: compose.common.yml
      service: hotio-service
    image: ghcr.io/hotio/radarr:latest
    # ...
```
