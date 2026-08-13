# LUMIN CAM — Development Guide

## First Development Milestone

The first implementation target is **Alpha 0.1**: a desktop application that can create a project, import STEP/IGES/STL geometry, display it in a 3D viewport, manage a model tree and save/load a `.lcam` project.

No machine-ready G-code should be generated in Alpha 0.1.

## Suggested Repository Layout

```text
lumin-cam/
├── app/
│   ├── frontend/
│   └── backend/
├── core/
│   ├── geometry/
│   ├── machining/
│   ├── toolpath/
│   ├── simulation/
│   ├── collision/
│   └── post/
├── docs/
├── machines/
├── tools/
├── tests/
└── examples/
```

## Engineering Rule

Do not build the entire CAM engine before the UI exists. Build vertical slices that can be demonstrated and tested.

Recommended sequence:

1. Application shell
2. 3D viewer
3. CAD import
4. Project persistence
5. Stock/WCS
6. Tool library
7. One simple toolpath strategy
8. Simulation
9. Post processor

## Validation

Every stage should have a reproducible example project. CAM regression fixtures should remain in `examples/` and `tests/` so future algorithm changes can be compared against known results.

## Branching

Use feature branches for implementation work. Keep `main` releasable when practical.

Suggested names:

- `feature/cad-import`
- `feature/tool-library`
- `feature/face-toolpath`
- `feature/simulation`
- `fix/post-processor`

## Commit Style

Use clear, imperative commit messages, for example:

- `feat: add STEP import service`
- `feat: add tool library model`
- `test: add contour regression fixture`
- `fix: correct WCS unit conversion`
- `docs: update post processor specification`
