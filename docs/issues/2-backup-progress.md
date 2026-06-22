# Issue #2: Backup

> **Issue**: <https://github.com/torrust/torrust-hash2torrent-demo/issues/2>
>
> **Goal**: Create a manual backup of the production server (Digital Ocean droplet) and store it in the `.backup/` folder of this repo.

## Prerequisites

- [ ] Digital Ocean droplet is turned ON.
- [ ] SSH access to the droplet is working.

## Server Info

- **Hosting**: Digital Ocean
- **Domain**: hash2torrent.com
- **App path**: `/home/torrust/github/torrust/torrust-hash2torrent`
- **Compose project**: `droplet/` directory with `compose.yaml`

## Backup Steps

### 1. Turn On the Droplet

- [x] Power on the droplet via [Digital Ocean Control Panel](https://cloud.digitalocean.com/).
- [x] Wait for the droplet to be fully booted.
- [x] Verify SSH access: `ssh hash2torrent`

### 2. Stop the Application (gracefully)

- [x] `docker compose down` (containers stopped and removed)

### 3. Create Backup Archive

- [x] Archive created: `hash2torrent-backup-20260622_170846.tar.gz` (3.6 GB)
- [x] Includes app source, Docker config, SSL certs, proxy config, and persistent storage

### 4. Download Backup Archive

- [x] Downloaded to `.backup/hash2torrent-backup-20260622_170846.tar.gz`
- [x] Archive verified (valid tar.gz, contents listing confirmed)

### 5. Clean Up Droplet Temp File

- [x] Removed the archive from the droplet: `sudo rm /tmp/hash2torrent-backup-*.tar.gz`

### 6. Turn Off the Droplet

- [x] Powered off the droplet via [Digital Ocean Control Panel](https://cloud.digitalocean.com/).

### 7. Droplet Decommissioned

- [x] All resources removed from Digital Ocean.
- [x] Future deployment (if any) to be reconsidered — possibly to Hetzner or another provider.

## Restore Instructions (if needed)

If you ever need to restore from this backup:

```bash
# On the droplet, after setting up the app directory
cd /home/torrust/github/torrust/torrust-hash2torrent
tar -xzf /path/to/backup/hash2torrent-backup-<timestamp>.tar.gz .
docker compose up --build --detach
```

## Notes

- The `.backup/` folder is gitignored.
- Backup timestamp is included in the filename for traceability.
- Backup includes: app source, Docker config, SSL certs, proxy config, and persistent storage (session/torrent data).

## Post-Backup Analysis

### Torrent Cache: Two Storage Locations Found

After extracting the backup, two torrent storage locations were discovered:

| Location | Torrents | Status |
| --- | --- | --- |
| `droplet/storage/hash2torrent/lib/torrents/` | **107,535** | ✅ Active (Docker Compose volume path) |
| `storage/hash2torrent/lib/torrents/` | **1** | 🗑️ Legacy/abandoned (only the demo torrent) |

The legacy location `storage/hash2torrent/lib/torrents/` is a leftover from a previous standalone (non-Docker) installation that was never cleaned up when the application was migrated to Docker Compose.
