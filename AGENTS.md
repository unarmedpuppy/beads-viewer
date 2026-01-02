# Beads Viewer - Agent Instructions

Web interface for the Beads issue tracker (`bd` CLI tool).

## Overview

Provides a browser-based UI for viewing and navigating Beads issues:
- Issues view with filtering and search
- Epics view with progress tracking
- Board view with workflow columns
- Keyboard-driven navigation
- Live database synchronization

**URL**: https://beads.server.unarmedpuppy.com  
**Port**: 3015 (direct access)

## Tech Stack

| Component | Technology |
|-----------|------------|
| Base Image | Node.js 22 (Debian slim) |
| UI | beads-ui (npm package) |
| CLI | bd Go binary |
| Database | SQLite (from JSONL source) |

## Project Structure

```
beads-viewer/
├── Dockerfile              # Custom build with bd binary + beads-ui
├── docker-compose.yml      # Deployment config
└── README.md               # Documentation
```

## How It Works

1. Container starts with `bd` Go binary installed
2. Startup script rebuilds SQLite DB from `.beads/issues.jsonl`
3. beads-ui Node.js server serves the web interface
4. Mounts home-server's `.beads/` directory for task data

### Key Implementation Details

- Uses `flock` wrapper around `bd` to prevent concurrent migration conflicts
- Rebuilds database fresh on each container start (JSONL is source of truth)
- beads-ui is installed globally via npm

## Quick Commands

```bash
# Local build and run
docker compose up -d --build

# View logs
docker compose logs -f

# Rebuild after beads-ui updates
docker compose build --no-cache && docker compose up -d
```

## Configuration

### Volume Mount (Critical)

The container mounts the home-server's beads directory:
```yaml
volumes:
  - /home/unarmedpuppy/server/.beads:/app/.beads
```

This displays tasks from the home-server project specifically.

### Environment Variables

```bash
PORT=3000           # Web UI port
HOST=0.0.0.0        # Bind address
BEADS_NO_DAEMON=1   # Disable background daemon
```

## Deployment

### Via home-server
```bash
# Deployed at: home-server/apps/beads-viewer/
cd ~/server/apps/beads-viewer
docker compose up -d
```

### Traefik Routing
- Host: beads.server.unarmedpuppy.com
- SSL: Via Traefik with Let's Encrypt

## Boundaries

### Always Do
- Rebuild database on container start (handled automatically)
- Use `--no-cache` when updating beads-ui version
- Check that `.beads/issues.jsonl` exists before starting

### Ask First
- Changing the mounted `.beads` directory
- Upgrading bd or beads-ui versions
- Modifying the startup script

### Never Do
- Edit the SQLite database directly (use JSONL)
- Remove the flock wrapper (causes migration conflicts)
- Mount multiple beads directories

## Troubleshooting

### Container won't start
```bash
# Check if JSONL exists
ls -la /home/unarmedpuppy/server/.beads/issues.jsonl

# Check logs
docker compose logs beads-viewer
```

### Database out of sync
```bash
# Restart to rebuild from JSONL
docker compose restart
```

### bd command hangs
- flock wrapper should prevent this
- Check for zombie bd processes: `ps aux | grep bd`

## See Also

- [Root AGENTS.md](../AGENTS.md) - Cross-project conventions
- [Beads UI GitHub](https://github.com/mantoni/beads-ui)
- [Beads CLI GitHub](https://github.com/steveyegge/beads)
