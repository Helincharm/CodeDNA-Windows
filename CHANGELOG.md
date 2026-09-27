# Changelog

All notable changes to CodeDNA are recorded in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and the project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] — 2026-09-27

### Added

- Interactive Code Graph for Python projects, available from the scan results:
  - Project view: packages and modules and the dependencies between them, with
    package drill-down and a neighbourhood view for any module or symbol.
  - Calls view: the call graph between project functions and methods, as an
    overview of the most connected modules and as a view of a single function
    with its callers and callees.
  - Focus Flow, relationship filters, main and all links, Fit, zoom, pan and an
    expanded view.
  - Mutual recursion is drawn as one card per function joined by a two-way
    edge; a function that calls itself is drawn as a loop on its own card.
  - External and unresolved calls can be shown as cards of their own; both are
    hidden by default. Unresolved calls are never attributed to a guessed
    target.
  - Symbol and package inspector with qualified names, definitions, return
    types, data flow, callers, callees and references.
  - Scans recorded by earlier versions are identified as having no symbol
    index; a new scan of the project generates it.
- `THIRD-PARTY-NOTICES.txt` in the distribution, listing the third-party
  components included in the application together with their licenses.

### Changed

- The background animation pauses while the window is minimised or not in
  focus, so an idle CodeDNA window uses almost no CPU.
- Scans run at full speed while the CodeDNA window is in the background.
- Clearer and more consistent interface text, empty states and tooltips, and
  higher contrast for secondary text in both themes.
- Package and executable metadata identify CodeDNA as proprietary software by
  Helinity; the executable's version information reads 1.0.1.

### Fixed

- The interface recovers when the analysis engine becomes ready late at
  startup, so the engine status, scan history and log access load reliably.
- Long names wrap at name separators in the inspector.

## [1.0.0] — 2026-09-16

First public desktop release.

### Platform availability

- **Windows x64** — available, validated as a packaged desktop application.
- **macOS Apple Silicon** — not released. The build pipeline exists but has not
  been built or runtime-validated on Apple Silicon hardware.

### Added

**Security analysis engine**

- 125 security rules across 11 security categories: Input Safety,
  Authentication, Authorization, Secrets, Data Protection, Dependency Health,
  Configuration, Browser Security, Backend Security, Cryptography and
  Infrastructure.
- 13 analyzers covering 19 of the 28 supported languages and file formats.
- 110 recognized extensions and file patterns.
- Python AST analysis with intra-procedural taint and data-flow tracking.
- Lightweight intra-file flow analysis for JavaScript, TypeScript, PHP and HTML.
- Pattern-based static security rules for C#, Go, Java and SQL.
- Configuration and value analysis for Dockerfile, Docker Compose, dotenv, INI,
  JSON, Properties, TOML, XML and YAML.
- Secret detection, including provider-specific credential formats and
  credentials embedded in URLs and connection strings.
- Dependency and manifest analysis for npm and PyPI projects.

**Project intelligence**

- 155 framework profiles spanning Python, JavaScript, TypeScript, Java, C#, Go,
  PHP, Ruby and Rust, describing routes, sinks, security controls and request
  sources.
- Framework and database detection with the evidence behind each match.
- Entry-point discovery, delivered in the JSON export.
- Project relationship graph, delivered in the JSON export. The graph is not
  drawn in the interface in this version.
- Per-scan inventory with analysis coverage per language.
- Provenance separation, so a finding inside a test fixture is not counted
  against product code.

**Findings and reporting**

- Security DNA score out of 100 with a letter grade, per-category breakdown and
  a stated score confidence.
- Findings carry severity, category, rule identifier, CWE reference, file and
  line, a code excerpt, the evidence, the observed data flow, the security
  controls missing on the path, and confidence reasoning.
- Severity and category filtering, and full-text search across findings.
- Scan history with delete.
- JSON export through a native save dialog.

**Desktop application**

- Native Windows desktop window with an embedded WebView2 interface.
- Native folder, file and save dialogs.
- Self-contained runtime: no separate Python, Node.js or npm installation.
- Offline-first. Core analysis performs no network access; optional dependency
  advisory lookup is disabled by default.
- Headless scanning from the command line via `--scan` and `--json`.
- Light and dark interface themes.

### Security and privacy

- No HTTP server, no listening port and no localhost dependency. The interface
  is loaded from disk and communicates with the analysis engine through an
  in-process desktop bridge.
- Analyzed source code is read in place and is never uploaded or executed.
- Regression tests reject any web-framework import, any listening socket during
  startup, and any browser-launch path in the interface.

### Known limitations

- Only Python has a full parser and taint analysis. Other supported languages
  are pattern-based, four of them with lightweight intra-file flow.
- C, C++, Kotlin, Ruby, Rust and Swift are detected and inventoried but are not
  deeply analysed.
- Findings are indicators that require human review. They are not proof of
  exploitability, and a clean scan is not proof that software is secure.

[1.0.1]: https://github.com/Helincharm/CodeDNA-Windows/releases/tag/v1.0.1
[1.0.0]: https://github.com/Helincharm/CodeDNA-Windows/releases/tag/v1.0.0
