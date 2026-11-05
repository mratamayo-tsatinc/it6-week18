# SubnetAcad v2

SubnetAcad v2 is a pure front-end, browser-based IPv4 subnetting learning and assessment application. It combines a six-panel subnet visualizer with configurable scored and ungraded learning modules ("Atoms"), deterministic scenario generation, retryable Practice and single-attempt Exam modes, optional timing, configurable browser persistence, CSV export, and QR-based result sharing.

The application is intentionally lightweight: `index.html`, `styles.css`, and `script.js` run directly in the browser without a package manager, compilation step, application server, or database. Its behavior is configuration-driven: users can change the dedicated configuration values near the top of `script.js` without altering the core generation, grading, rendering, navigation, or persistence logic.

## Run locally

Serve the repository over HTTP so the browser can load `students.csv`:

```powershell
python -m http.server 8000
```

Then open `http://localhost:8000`. Opening `index.html` directly with `file://` may prevent the CSV request from working.

## Key files

| File | Purpose |
|---|---|
| `index.html` | Application shell, modal markup, visualizer panels, and Atom view containers |
| `styles.css` | Design tokens, responsive layout, component states, and Atom/visualizer styling |
| `script.js` | Configuration, question generation, grading, persistence, navigation, QR sharing, and subnet calculations |
| `students.csv` | Client-loaded student allowlist used by the login screen |

## Product documentation

- [Documentation index](docs/README.md)
- [Project brief](docs/PROJECT_BRIEF.md)
- [Product requirements document](docs/PRD.md)
- [Implementation plan](docs/IMPLEMENTATION_PLAN.md)
- [UI/UX components and patterns](docs/UI_UX_COMPONENTS.md)

## Current configuration

The checked-in configuration defaults to Practice mode with the timer disabled and local persistence enabled. Practice and Exam maintain separate per-student local sessions containing their seeds, answers, progress, locks, and scores. Legacy conversion phases 1–7 and Atoms 1–5 are disabled. Atom 6 provides ten incrementally arranged, scored VLSM planning scenarios using configurable Class C `/24` parent pools, progressing from 3 LANs + 1 WAN to 5 LANs + 3 WANs. Atom 7 provides one persistent, ungraded Class C `/24` Packet Tracer design brief with 3 routers, 5 department LANs distributed `[2, 1, 2]`, and 2 adjacent serial WANs. Atom 8 reuses that exact seeded requirement for a scored in-app implementation activity with configurable router ports, representative hosts, static routes, automatic validation, and port-status overlays. All topology and seed parameters are configured near the top of `script.js`.

## Configuration model

Curriculum and session parameters are intentionally separated from the core application engine. Users can manually edit the configuration section in `script.js` to control items such as:

- enabled or disabled phases and Atoms;
- question counts and Atom 5 subtype counts;
- decimal ranges, binary widths, and allowed CIDR values;
- allowed IPv4 address classes per activity;
- Atom 6–8 topology, address-pool, LAN-profile, naming, seed, interface, representative-host, and scoring parameters;
- strict binary-answer formatting;
- default Practice/Exam mode, timer duration, practice seeding, and sample-output behavior.

These changes do not require editing the generator, grader, visualizer, or UI implementation.

## Important limitation

The current login and exam controls are client-side learning conveniences, not security boundaries. Student records, question logic, scores, timing, and result-token encryption are accessible to a user who can inspect or modify browser resources. See the PRD and implementation plan for the recommended hardening path.
