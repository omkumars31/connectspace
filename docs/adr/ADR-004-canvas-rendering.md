# ADR-004: Use HTML5 Canvas for World Rendering

## Status

Accepted

## Context

The ConnectSpace frontend contains two distinct responsibilities.

Application UI:

- Authentication
- Chat
- Settings
- Sidebar
- Future administrative interfaces

Realtime world rendering:

- Player movement
- Avatar rendering
- Map rendering
- Continuous updates

Three rendering approaches were considered:

- DOM + CSS transforms
- HTML5 Canvas
- PixiJS

The project focuses on realtime multiplayer systems rather than frontend rendering frameworks.

## Decision

The application UI will be built using React.

The shared virtual world will be rendered using the HTML5 Canvas API.

A dedicated renderer will consume the current world state and draw the entire scene each frame.

React will not participate in the per-frame rendering loop.

The rendering architecture will be separated through a Renderer interface so the rendering implementation can be replaced in the future without affecting the Game Engine.

## Consequences

### Benefits

- Rendering becomes a pure function of world state.
- React remains focused on UI rather than animation.
- Reduced per-frame framework overhead.
- Clear separation between simulation and presentation.
- Sufficient performance for the MVP.

### Trade-offs

- Lower-level rendering APIs.
- Manual implementation of rendering utilities.
- Fewer built-in game development features than PixiJS.

These trade-offs are acceptable because the rendering requirements for the MVP remain intentionally simple.

## Future Reconsideration

This decision should be revisited if:

- Large tile maps are introduced.
- Advanced animations become necessary.
- Sprite batching or rendering optimizations become a bottleneck.
- The rendering layer grows significantly in complexity.

Under those conditions, migrating to PixiJS would become a reasonable architectural improvement.