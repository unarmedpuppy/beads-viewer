# beads-viewer (DEPRECATED)

> **This repository is deprecated.** The beads viewer is now integrated into
> the [homelab-ai dashboard](https://ai-dashboard.server.unarmedpuppy.com/beads).
>
> Access the beads viewer at: https://ai-dashboard.server.unarmedpuppy.com/beads

## Migration

The beads viewer functionality has been moved to the homelab-ai dashboard as part
of the Retro Dashboard project. The new implementation provides:

- Three-column Kanban board: Backlog → In Progress → Done
- Label filtering with sidebar
- Task creation with repo labels
- Real-time updates via polling
- Mobile-responsive design with swipe navigation
- Integrated with Ralph Loop management

See the [homelab-ai dashboard](../homelab-ai/dashboard/) for the new implementation.

## Historical Information

This was the original standalone web interface for the Beads issue tracker (`bd` CLI tool).

- **Former URL**: https://beads.server.unarmedpuppy.com
- **Former Port**: 3015
- **Status**: Deprecated

### Original Features

- Issues view with filtering and search
- Epics view with progress tracking
- Board view with workflow columns
- Keyboard-driven navigation
- Live database synchronization

## References

- [homelab-ai Dashboard](../homelab-ai/dashboard/) - New location
- [Beads CLI Reference](../home-server/agents/reference/beads.md)
