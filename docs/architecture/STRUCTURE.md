# Repository architecture

This is a modular monorepo. Root holds only workspace-level files.

- `docs/`: specifications, architecture, API and contributor documentation.
- `packages/core/`: framework-independent TypeScript AD/BS conversion and calendar logic.
- `packages/react/`: future reusable React UI components.
- `packages/api-client/`: future typed HTTP client.
- `frontend/`: playground, documentation website and app examples.
- `backend/`: optional API server and its internal code.
- `database/`: optional database schema, migrations and seed tooling.
- `datasets/`: licensed, verified calendar tables and separately sourced event data.
- `sdks/`: future language-specific implementations.
- `tests/`: shared cross-language conformance fixtures and integration tests.
- `.github/`: CI and issue templates.

**Incremental policy:** Create directories only when they contain real source, configuration, tests or documentation. Do not use placeholder files to fabricate implementation progress.

**Initial milestone:** verify source licensing and date range before implementing AD↔BS conversion.
