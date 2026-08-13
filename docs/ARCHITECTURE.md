# LUMIN CAM — System Architecture

## 1. Architecture Goal

LUMIN CAM is structured as a modular desktop CAM application. The architecture separates user interface, geometry, CAM calculation, simulation, collision verification, project storage, and post processing.

```text
                LUMIN CAM DESKTOP
                        │
              ┌─────────┴─────────┐
              │                   │
           Frontend             Backend
        React/TypeScript       Native/Services
              │                   │
              └─────────┬─────────┘
                        │
        ┌───────────────┼────────────────┐
        │               │                │
     Geometry        CAM Engine       Project Core
        │               │                │
     CAD data        Toolpaths        .lcam data
        │               │                │
        └───────────────┼────────────────┘
                        │
             ┌──────────┴──────────┐
             │                     │
         Simulation            Collision
             │                     │
             └──────────┬──────────┘
                        │
                  Post Processor
                        │
                     G-code
```

## 2. Module Boundaries

### Frontend
Responsible for:
- UI layout
- User interaction
- 3D viewport
- Operation forms
- Toolpath display
- Simulation controls
- Notifications and validation messages

### Application Core
Responsible for:
- Project lifecycle
- Command/state management
- Undo/redo architecture
- Unit conversion
- Feature orchestration

### Geometry Engine
Responsible for:
- Import
- Geometry representation
- Bounding boxes
- Selection
- Surface/face metadata
- Model validation

### CAM Engine
Responsible for:
- Toolpath strategies
- Cutting parameters
- Linking
- Toolpath validation
- Operation dependencies

### Simulation Engine
Responsible for:
- Stock model
- Material removal
- Tool movement
- Simulation state
- Cycle-time calculation

### Collision Engine
Responsible for:
- Tool collision
- Holder collision
- Rapid collision
- Clearance checks

### Post Processor
Responsible for:
- Controller-specific code generation
- Program formatting
- Machine-specific commands
- G-code validation rules

### Persistence
Responsible for:
- `.lcam` project format
- Preferences
- Tool library data
- Machine library data

## 3. Recommended Technology Direction

- Desktop shell: Tauri
- Frontend: React + TypeScript
- 3D graphics: Three.js
- Geometry/CAD: Open CASCADE Technology
- CAM algorithms: OpenCAMLib plus LUMIN-specific algorithms
- Native computation: C++
- Utility tooling/testing: Python

These are architecture candidates rather than irreversible commitments. Each dependency must pass licensing, integration, performance, and maintenance review before production adoption.

## 4. Core Design Rule

The frontend must never directly own complex machining mathematics. UI actions should call typed application services, which invoke geometry/CAM/simulation modules.

```text
UI Event
  ↓
Application Command
  ↓
CAM Service
  ↓
Geometry/CAM Kernel
  ↓
Validated Toolpath
  ↓
Simulation / Collision
  ↓
Post Processor
```

## 5. Error Handling

Errors should be classified as:

- Import error
- Geometry validation error
- Parameter validation error
- Toolpath generation error
- Simulation error
- Collision warning/error
- Post processor error
- Project persistence error

Users should receive actionable messages rather than raw stack traces.

## 6. Testing Strategy

Each module must have unit tests. Cross-module workflows must have integration tests. Critical toolpaths and post processors must have deterministic regression fixtures.

No generated G-code should be considered production-safe solely because an automated test passed.
