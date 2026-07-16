# ADR-001: Use Socket.IO as the Realtime Transport Layer

## Status
Accepted

## Context
ConnectSpace is a realtime multiplayer virtual workspace where users move through a shared 2D environment, interact with nearby players, and receive low-latency updates from the server.

One of the earliest architectural decisions was selecting the transport layer for realtime communication between the client and the server.

The primary options considered were:
- Raw WebSockets
- Socket.IO

The project's primary objective is to learn and demonstrate realtime system design concepts such as server-authoritative movement, multiplayer synchronization, spatial partitioning, interest management, and proximity-based communication. The transport layer should support these goals without becoming the focus of the project itself.

## Decision
The project will use Socket.IO as the realtime transport layer.

Socket.IO provides an event-driven API that simplifies bidirectional communication while also offering features that would otherwise need to be implemented manually, including:
- Room-based communication
- Automatic reconnection
- Connection lifecycle events
- Broadcasting helpers
- Automatic serialization of events

These features allow development effort to remain focused on the core engineering challenges of the project rather than rebuilding transport-layer functionality.

The Game Engine will not directly depend on Socket.IO APIs. Instead, Socket.IO will terminate at a dedicated Realtime Gateway responsible for translating network events into domain events consumed by the Game Engine. This keeps the transport layer replaceable in the future.

## Consequences

### Benefits
- Faster development with fewer transport-level concerns.
- Built-in support for rooms, reconnects, and event routing.
- Cleaner event-based programming model.
- Automatic fallback to HTTP long-polling if a WebSocket handshake fails (e.g. restrictive networks or corporate proxies), improving connection reliability without extra work.
- Allows more time to focus on multiplayer architecture and synchronization.
- Easier to explain and maintain within the scope of an MVP.

### Trade-offs
- Slight protocol overhead compared to raw WebSockets.
- Additional abstraction layer.
- Less control over the underlying transport.

These trade-offs are acceptable because the expected scale of the MVP is relatively small, and network transport is unlikely to become the primary performance bottleneck.

## Future Reconsideration
This decision should be revisited if:
- The application targets significantly higher concurrency.
- Transport overhead becomes a measurable bottleneck through profiling.
- Fine-grained control over packet formats or transport behavior becomes necessary.

If those conditions arise, replacing Socket.IO with raw WebSockets should be possible because the Game Engine is intentionally isolated from the transport implementation through the Realtime Gateway.