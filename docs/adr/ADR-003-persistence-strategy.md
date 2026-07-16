# ADR-003: Use PostgreSQL with an In-Memory Authoritative Game State

## Status

Accepted

## Context

ConnectSpace manages two fundamentally different categories of data.

Persistent data:

- Users
- Credentials
- Profiles
- Last saved player position
- Future workspace metadata

Realtime data:

- Current player position
- Direction
- Online status
- Socket connections
- Spatial grid membership
- Nearby players

Persistent data requires durability and relational consistency.

Realtime data changes continuously and becomes obsolete almost immediately.

Using PostgreSQL as the authoritative source for every movement update would introduce unnecessary disk writes and move the database into the critical path of the realtime simulation.

## Decision

The authoritative game state will exist entirely in server memory.

PostgreSQL will store only durable application data.

Player movement and other realtime state will never be written to PostgreSQL every server tick.

Persistence will use a hybrid strategy:

- Immediate save on graceful disconnect.
- Periodic snapshots of modified players using dirty tracking.
- Batch database writes during snapshot intervals.

## Consequences

### Benefits

- Keeps the realtime simulation independent of database latency.
- Greatly reduces write volume.
- Bounds crash recovery data loss to the snapshot interval.
- Dirty tracking prevents unnecessary database writes.
- Aligns storage technology with its intended purpose.

### Trade-offs

- Additional snapshot mechanism.
- Live state is lost if the server crashes between snapshots.
- More coordination between memory and persistent storage.

These trade-offs are acceptable because the database is used for durability rather than realtime synchronization.

## Future Reconsideration

This decision should be revisited if:

- Multiple backend instances are introduced.
- Shared realtime state becomes necessary.
- Horizontal scaling requires distributed synchronization.

Redis or another distributed in-memory store may then become part of the architecture.