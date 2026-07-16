# ADR-002: Use Express.js as the Backend Framework

## Status

Accepted

## Context

The backend for ConnectSpace will expose REST APIs for authentication and persistent data while also hosting the realtime Socket.IO server responsible for multiplayer communication.

The architecture already defines clear boundaries between responsibilities, including:

- Authentication
- Player Service
- Workspace Service
- Socket Gateway
- Game Engine
- Broadcast Engine
- Database Layer

The primary framework options considered were:

- Express.js
- NestJS
- Fastify

The project is intended to demonstrate realtime multiplayer architecture rather than framework-specific concepts. Development time is limited, and the implementation should prioritize engineering depth in networking and distributed systems over learning a new backend framework.

## Decision

The backend will be built using Express.js with TypeScript.

Although Express is intentionally minimal, the project will enforce a modular architecture through folder structure and clear separation of responsibilities rather than relying on framework conventions.

The application will organize business logic into independent services while keeping HTTP controllers and realtime gateways as thin entry points.

## Consequences

### Benefits

- Familiar ecosystem and faster development.
- Greater focus on realtime architecture rather than framework concepts.
- Simple integration with Socket.IO.
- Full control over project structure.
- Lower learning overhead during implementation.

### Trade-offs

- Architectural discipline must be maintained manually.
- No built-in dependency injection.
- No framework-enforced modules or guards.

These trade-offs are acceptable because the project is relatively small and the architecture has already been designed independently of the framework.

## Future Reconsideration

This decision should be revisited if:

- The project grows into many independent modules.
- Multiple backend developers begin contributing.
- Framework-level dependency injection or modularity becomes a productivity advantage.

In that case, migrating to NestJS could provide stronger architectural enforcement without changing the underlying domain model.