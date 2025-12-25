# media-center 🚀

Welcome to the media-center Docker Compose stack — a collection of services to manage, request, download, and monitor your media libraries (movies, TV, music). This README gives a concise, modern guide to get started, configure the stack, and understand each component.

Contents
--------

- [Quick start ⚡](#quick-start-)
- [Services 🧩](#services-)
- [Configuration 🔧](#configuration-)
- [Security & secrets 🔒](#security--secrets-)
- [Docs & Links 📚](#docs--links-)
- [Troubleshooting 🐞](#troubleshooting-)
- [Contributing & Support 🤝](#contributing--support-)

Quick start ⚡
-------------

1. Copy the example env and edit values:

	```bash
	cp .env.example .env
	# edit .env (do NOT commit this file)
	```

2. Validate compose and pull images:

	```bash
	docker compose config
	docker compose pull
	docker compose up -d
	```

3. Check status:

	```bash
	docker compose ps
	docker compose logs -f <service>
	```

Services 🧩
-----------

Short descriptions (what they do):

- **pia-qbittorrent** 🧭 — qBittorrent with Private Internet Access (PIA) VPN support; used for torrent downloads. Configure PIA creds via `.env` (PIA_USER / PIA_PASSWORD).
- **flaresolverr** 🔓 — Solves Cloudflare/anti-bot challenges for indexers and web fetchers.
- **prowlarr** 🔎 — Indexer aggregator and manager for Radarr/Sonarr/Lidarr.
- **radarr** 🎬 — Movie manager: handles requests, downloads, and file organization.
- **sonarr** 📺 — TV manager: same as Radarr but for TV series.
- **lidarr** 🎵 — Music manager for albums/tracks and imports.
- **soularr** 🎧 — Music request automation that uses Slskd to fetch from Soulseek-style networks.
- **pinchflat** ⬇️ — Downloader service (YouTube/other sources).
- **slskd** 🧩 — Soulseek-compatible daemon used by Soularr.
- **seerr** 📝 — Request manager UI for users to request media (Seerr, successor to Overseerr/Jellyseerr).
- **tautulli** 📊 — Plex monitoring/analytics for Plex servers.

Configuration 🔧
----------------

- Edit `.env` (from `.env.example`) for host paths and user mapping: `CONTAINERS_DIR`, `DOWNLOADS_DIR`, `MEDIA_DIR`, `YOUTUBE_DIR`, `PUID`, `PGID`, `TZ`.
- Use `SEERR_PORT` and `TAUTULLI_PORT` to customize web ports.
- For Seerr DB: set `DB_TYPE=postgres` and provide `DB_HOST`, `DB_USER`, `DB_PASS`, `DB_NAME`.

Security & secrets 🔒
---------------------

- **Do not commit secrets.** Put sensitive values in `.env` (ignored via `.gitignore`) or use Docker secrets.
- If a secret was leaked, rotate it and purge it from Git history (see [git-filter-repo](https://github.com/newren/git-filter-repo) or [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)). Example purge commands are in this README.

Docs & Links 📚
----------------

- Seerr: https://docs.seerr.dev/ (Docker: https://hub.docker.com/r/seerr/seerr)
- Tautulli: https://tautulli.com/ (GitHub: https://github.com/Tautulli/Tautulli)
- PIA qBittorrent: https://github.com/j4ym0/pia-qbittorrent
- Flaresolverr: https://github.com/flaresolverr/flaresolverr
- Prowlarr: https://wiki.servarr.com/prowlarr/
- Radarr: https://radarr.video/
- Sonarr: https://sonarr.tv/
- Lidarr: https://lidarr.audio/
- Soularr: check the container image notes and project repo
- Pinchflat: https://github.com/kieraneglin/pinchflat
- Slskd: https://hub.docker.com/r/slskd/slskd

Troubleshooting 🐞
------------------

- `manifest unknown` on deploy: pin the image to a published tag (e.g., `seerr/seerr:v2.7.3`) or authenticate to GHCR if using GHCR images.
- Use `docker compose logs -f <service>` to view startup errors and common misconfigurations (missing env vars, permissions, etc.).

Contributing & Support 🤝
------------------------

- When contributing, avoid committing local config files or secrets. Open issues/PRs and include logs/config (sanitized) when asking for help.
- If you want help purging secrets from history or hardening the stack, I can prepare scripts and a safe step-by-step plan.

Security note
-------------

I removed `soularr/config.ini` from the repo and the Git history in a previous step due to an exposed API key — please rotate that key if you haven't already. Collaborators should re-clone after a history rewrite:

```bash
rm -rf media-center && git clone https://github.com/jeanrdfs/media-center.git
```

If you want, I can also add short sample config snippets per service or a diagram showing how services connect. Let me know which you prefer.