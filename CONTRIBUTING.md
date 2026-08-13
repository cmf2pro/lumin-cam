# Contributing to LUMIN CAM

Thank you for helping build LUMIN CAM.

## Development Principles

- Keep modules small and focused.
- Prefer readable, testable code over clever code.
- Do not mix UI logic with CAM mathematics.
- Add tests for new geometry, toolpath, simulation, and post-processing behavior.
- Document machining assumptions and units.
- Never assume generated G-code is safe for a real machine without verification.

## Contribution Flow

1. Open or discuss an issue.
2. Define the requirement and acceptance criteria.
3. Implement the smallest complete change.
4. Add or update tests.
5. Update documentation.
6. Open a pull request.

## CAM-Specific Changes

Every new machining strategy should document:
- Supported geometry
- Tool types
- Units
- Cutting assumptions
- Entry/exit behavior
- Linking behavior
- Tolerance rules
- Known limitations
- Regression test cases

## Safety

LUMIN CAM is manufacturing software. Changes that affect toolpaths, simulation, collision detection, or G-code generation require additional review and deterministic test coverage.
