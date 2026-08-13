# LUMIN CAM — Product Requirements Document

**Version:** 0.1 Draft  
**Status:** Foundation  
**Product:** LUMIN CAM  
**Category:** 3-axis CAM software for CNC/VMC machining  
**Initial access model:** Free for the first 3 months  

## 1. Product Vision

LUMIN CAM is intended to become an easy-to-use, professional 3-axis CAM platform for CNC/VMC machining. The product will guide a machinist from CAD import through job setup, toolpath creation, simulation, verification, and G-code generation.

The product should prioritize clarity, predictable machining behavior, practical shop-floor workflows, and a beginner-friendly interface without removing professional CAM controls.

## 2. Problem Statement

Many CAM systems are powerful but difficult for new users to learn. Small shops and individual programmers also need practical tooling, machine-aware post processing, simulation, and verification without unnecessary complexity.

LUMIN CAM will address this by providing a structured workflow:

`Import CAD → Define Job → Define Stock → Define WCS → Select Tools → Create Operations → Generate Toolpaths → Simulate → Verify → Post Process`

## 3. Target Users

### Primary
- VMC/CNC operators moving into CAM programming
- CAM programmers working with 3-axis VMC machines
- Small machining workshops
- Manufacturing students and trainees

### Secondary
- Experienced CNC programmers who want a lightweight CAM workflow
- Developers and researchers interested in open CAM technologies

## 4. Product Goals

1. Make common 3-axis CNC programming tasks easier to learn and execute.
2. Provide reliable CAD import and 3D visualization.
3. Provide practical 2.5D and 3-axis toolpath strategies.
4. Provide stock-aware simulation and collision warnings before G-code export.
5. Support configurable machine/controller post processors.
6. Keep the architecture modular so more CAM strategies can be added later.
7. Build toward a professional desktop product with a clear separation between UI, geometry, CAM, simulation, and posting.

## 5. Non-Goals for v0.1–v1.0

LUMIN CAM will not initially attempt to replace a full CAD package or provide 4/5-axis simultaneous machining. Advanced solid modeling, turning, wire EDM, robotics, and full enterprise manufacturing management are outside the initial scope.

## 6. Core Workflow

1. Create a new project.
2. Import a CAD model.
3. Set units.
4. Define machine and work coordinate system.
5. Define stock.
6. Create/select tools.
7. Add machining operations.
8. Generate toolpaths.
9. Inspect toolpath geometry and parameters.
10. Simulate material removal.
11. Detect tool, holder, stock, and model collisions.
12. Review estimated machining time.
13. Select a post processor.
14. Generate G-code.
15. Review generated G-code.
16. Save project and setup documentation.

## 7. Functional Requirements

### 7.1 Project Management
- Create, save, open, and duplicate projects.
- Native project format: `.lcam`.
- Store model references, stock, WCS, tools, operations, machine, post processor, and project metadata.
- Persist units and application preferences.

### 7.2 CAD Import
Initial supported formats:
- STEP
- IGES
- STL

Future candidates:
- OBJ
- DXF
- Parasolid

The geometry subsystem should validate imported models and report import failures without crashing the application.

### 7.3 3D Viewer
Required capabilities:
- Orbit/rotate
- Pan
- Zoom
- Fit model
- Shaded display
- Wireframe display
- Transparency
- Face/edge selection
- Coordinate axes
- WCS visualization
- Stock visualization
- Toolpath visualization

### 7.4 Job Setup
Users must be able to configure:
- Units: metric/inch
- Stock dimensions
- Stock offsets
- Work coordinate system
- Model origin
- Machine selection
- Safe Z / clearance values

### 7.5 Tool Library
Initial tools:
- Flat end mill
- Ball end mill
- Bull nose end mill
- Drill
- Chamfer mill

Tool data should support:
- Tool number
- Diameter
- Corner radius
- Overall length
- Flute length
- Number of flutes
- Tool material
- Holder/shank metadata
- Cutting parameters

### 7.6 Initial Machining Operations
Priority order:
1. Face
2. 2D contour
3. Pocket
4. Drill
5. Chamfer
6. Engrave
7. 3D roughing
8. Waterline
9. Parallel/raster finishing
10. Scallop finishing
11. Pencil finishing

### 7.7 Toolpath Parameters
Operations should expose, where relevant:
- Tool
- Spindle speed
- Feed rate
- Plunge feed
- Stepdown
- Stepover
- Stock to leave
- Tolerance
- Cutting direction
- Entry strategy
- Lead-in/lead-out
- Linking/retract behavior
- Safe heights
- Coolant command

### 7.8 Simulation
The simulation engine shall support:
- Stock removal visualization
- Tool movement
- Rapid movements
- Cutting movements
- Tool and holder display
- Play/pause/stop/reset
- Simulation speed control
- Operation filtering
- Estimated cycle time

### 7.9 Collision Detection
Initial collision classes:
- Tool vs model
- Holder vs model
- Tool vs stock when inappropriate
- Rapid move vs stock/model
- Invalid Z clearance

All warnings must identify the operation and approximate XYZ location.

### 7.10 Post Processing
The system shall provide configurable post processors.

Initial targets:
- Generic 3-axis
- FANUC
- Mitsubishi
- Haas

Post processor configuration should support:
- Units
- Program numbering
- Tool change
- Spindle commands
- Coolant
- Work offset
- Tool length compensation
- Rapid/cutting formatting
- Drilling cycles
- Program start/end
- Optional machine-specific blocks

### 7.11 G-code Viewer
- Syntax-aware text view
- Operation grouping where available
- Line numbering
- Search
- Copy/export
- Link selected G-code lines to simulation/toolpath position when practical

### 7.12 Documentation Output
Generate:
- Setup sheet
- Tool list
- Operation list
- Program metadata
- Estimated cycle time

## 8. User Experience Requirements

### Beginner Mode
Use a guided sequence:

`Model → Stock → WCS → Tool → Operation → Parameters → Generate → Simulate → Export`

### Professional Mode
Expose advanced parameters for experienced programmers.

The UI must prioritize machining intent over technical implementation details. Users should be able to understand what an operation will do before generating it.

## 9. Architecture Requirements

The application should maintain clear module boundaries:

- UI layer
- Application/project layer
- Geometry layer
- CAM/toolpath layer
- Simulation layer
- Collision layer
- Post processor layer
- Persistence layer

Recommended technology direction:
- Desktop shell: Tauri
- Frontend: React + TypeScript
- 3D rendering: Three.js
- CAD kernel/data exchange: Open CASCADE Technology
- CAM algorithms: OpenCAMLib plus LUMIN-specific algorithms where required
- Native computation: C++
- Utility/testing scripts: Python
- Local project metadata: SQLite/structured files as appropriate

Technology decisions must be validated during architecture and proof-of-concept phases.

## 10. Safety and Manufacturing Verification

LUMIN CAM must not claim that generated G-code is automatically safe for every machine. Users must review tool data, work offsets, machine limits, tooling, fixturing, and the generated program before running it on a real machine.

The software should display clear warnings for incomplete or potentially unsafe setups.

## 11. Performance Requirements

The application should:
- Remain interactive during ordinary model viewing.
- Generate common 2.5D operations without blocking the UI.
- Move heavy computations to worker/native processes when practical.
- Support progress reporting and cancellation for long calculations.
- Avoid memory growth during repeated simulation/reset cycles.

Performance benchmarks will be established during Alpha development.

## 12. Licensing and Commercial Model

Initial product access is planned as free for the first 3 months. The final commercial model is not fixed in this PRD and must be documented before public release.

Third-party components must be tracked with their licenses and redistribution requirements. No open-source licensing claim should be made for LUMIN CAM itself until the source-availability and dependency strategy is finalized.

## 13. Version Roadmap

### Alpha 0.1 — Foundation
- Desktop shell
- Project system
- STEP/IGES/STL import
- 3D viewer
- Model tree
- Units
- Save/load `.lcam`

### Alpha 0.2 — Job Setup
- Stock
- WCS
- Machine setup
- Tool library
- Basic setup validation

### Alpha 0.3 — 2.5D CAM
- Face
- Contour
- Pocket
- Drill
- Chamfer
- Toolpath display

### Alpha 0.4 — 3D CAM
- 3D roughing
- Waterline
- Parallel/raster
- Scallop
- Pencil

### Alpha 0.5 — Verification
- Stock simulation
- Tool/holder visualization
- Collision detection
- Cycle-time estimation

### Beta 0.6–0.9 — Production Workflow
- FANUC/Mitsubishi/Haas posts
- G-code viewer
- Setup sheets
- Project validation
- Improved performance
- Automated regression tests

### LUMIN CAM 1.0
- Stable 3-axis workflow
- Production-oriented verification
- Documented post processor system
- Installer/release process
- User documentation

## 14. Future Direction

Potential post-1.0 capabilities:
- Rest machining
- Feature recognition
- Automatic operation suggestions
- Tool recommendation engine
- Templates
- Machine libraries
- Advanced holder collision
- Adaptive clearing
- AI-assisted process planning
- Cloud project synchronization
- Team/project management
- 4/5-axis foundations

AI must remain an advisory layer. Final toolpath generation, verification, and posting should remain deterministic and testable.

## 15. Acceptance Criteria for v1.0

A v1.0 release is considered viable when a trained CNC/CAM user can:

1. Import a supported CAD model.
2. Define stock and WCS.
3. Define or select the required tools.
4. Create common 2.5D and 3-axis operations.
5. Generate visible toolpaths.
6. Simulate material removal.
7. Identify defined collision classes.
8. Generate G-code through a supported post processor.
9. Review the program before export.
10. Save and reopen the complete project without losing setup data.

## 16. Open Questions

- Final pricing after the initial 3-month free period
- Which post processors are included in the first public release
- Whether the core will be fully open source, source-available, or proprietary
- Exact native project file implementation
- Supported Windows/Linux/macOS versions
- Final CAD kernel/CAM library integration strategy

## 17. Success Metrics

Initial project metrics:
- Time from installation to first imported model
- Time from model import to first successful toolpath
- Toolpath generation failure rate
- Simulation stability
- Post processor correctness on validated test cases
- Number of reproducible bugs per release
- User completion rate for the beginner workflow

---

**Document owner:** LUMIN CAM project  
**Next document:** System Architecture  
**Next implementation milestone:** Alpha 0.1 foundation
