# CodeDNA — Windows

CodeDNA is a desktop-first static analysis and code intelligence tool designed to inspect software projects, identify security issues, visualize project structure, and produce actionable findings without requiring a local web server.

This repository contains the **Windows x64 release** of CodeDNA.

---

## Overview

CodeDNA runs as a standalone Windows desktop application.

The application does **not** rely on:

- a browser tab
- `localhost`
- FastAPI
- Uvicorn
- Node.js at runtime
- a separately installed Python environment

The desktop shell loads the bundled interface locally and communicates directly with the CodeDNA analysis engine through an embedded desktop bridge.

### Runtime architecture

```text
CodeDNA.exe
    ↓
Desktop Window
    ↓
Embedded UI
    ↓
Python Bridge
    ↓
CodeDNA Analysis Engine
```

---

## Features

- Static code analysis
- Security-focused findings
- Rule-based scanning
- Project structure analysis
- Entry point discovery (included in the JSON export)
- Findings filtering
- Finding detail inspection
- Scan history
- Project relationship graph (included in the JSON export)
- JSON export
- Native folder picker
- Native ZIP picker
- Native save dialogs
- Offline-first analysis workflow

---

## Screenshots

| | |
| --- | --- |
| ![Scan overview](screenshots/overview.png) | **Overview** — Security DNA score, severity totals and the findings list. |
| ![Findings](screenshots/findings.png) | **Findings** — filter by severity or category, or search across files and evidence. |
| ![Finding detail](screenshots/finding-detail.png) | **Finding inspector** — rule, CWE, code excerpt, evidence, confidence reasoning, missing controls and the observed data flow. |
| ![Inventory and coverage](screenshots/inventory.png) | **Inventory and coverage** — files analysed, detected frameworks and databases, and analysis depth per language. |

## Analysis Coverage

CodeDNA recognises more file types than it deeply analyses, so coverage is stated per
language rather than as a single number.

| | |
| --- | --- |
| Supported languages and file formats | 28 |
| Of those, with analyzer coverage | 19 |
| Recognized extensions and file patterns | 110 |
| Security rules | 125 |
| Analyzers | 13 |
| Framework profiles | 155 |

### Depth by language

| Coverage level | Languages / formats |
| --- | --- |
| Full semantic analysis — AST and taint/data-flow | Python |
| Static analysis with lightweight intra-file flow | JavaScript, TypeScript, PHP, HTML |
| Static security analysis | C#, Go, Java, SQL |
| Configuration and value analysis | Dockerfile, Docker Compose, dotenv, INI, JSON, Properties, TOML, XML, YAML |
| Generic / resource checks | CSS, Markdown, Shell, Text |
| Detection only | C, C++, Kotlin, Ruby, Rust, Swift |

Detection-only languages are identified and inventoried; they are not deeply analysed.

The interface reports how many languages a given scan encountered. That figure describes
the project being scanned, not the engine.

### What the engine does

- Python AST analysis with intra-procedural taint / data-flow tracking
- Lightweight intra-file flow for JavaScript, TypeScript, PHP and HTML
- Pattern-based static security rules for the remaining supported languages
- Secret detection
- Configuration analysis
- Dependency and manifest analysis
- Framework detection across Python, JavaScript, TypeScript, Java, C#, Go, PHP, Ruby and Rust
- Entry-point discovery
- Project relationship graph (engine output; delivered in the JSON export)
- Security DNA scoring with per-category confidence

Findings are indicators that require human review. They are not proof of exploitability,
and a clean scan is not proof that software is secure.

## Windows Release

### Platform

- Windows 10 / 11
- x64 architecture

### Release artifact

```text
CodeDNA-Windows-x64.zip
```

Inside the package:

```text
CodeDNA/
└── CodeDNA.exe
```

No separate Python, Node.js, npm, Vite, FastAPI, or Uvicorn installation is required.

---

## Installation

1. Download the latest `CodeDNA-Windows-x64.zip` release.
2. Extract the archive.
3. Open the extracted `CodeDNA` folder.
4. Run `CodeDNA.exe`.

Windows may display a security warning for unsigned or newly distributed applications depending on the release configuration.

---

## Usage

### Scan a local project

1. Launch CodeDNA.
2. Select a project folder or ZIP archive.
3. Start the scan.
4. Review findings, severity levels, and project inventory and coverage.
5. Export results when needed.

### Command-line scan

The packaged executable also supports headless scanning:

```text
CodeDNA.exe --scan <path>
```

Optional JSON output:

```text
CodeDNA.exe --scan <path> --json result.json
```

This mode starts the analysis, writes the result, and exits.

It does not start a server or open a browser window.

---

## Desktop Architecture

CodeDNA is intentionally designed as a desktop application rather than a localhost-hosted web application.

The Windows build uses an embedded WebView2 runtime for interface rendering.

`msedgewebview2.exe` helper processes may appear while CodeDNA is running. These processes belong to the embedded WebView2 runtime and do not represent a separately opened Edge browser.

The application itself does not require a listening HTTP port.

---

## Current Windows Validation

The Windows x64 build has been tested as a packaged desktop application.

Validated areas include:

- CodeDNA desktop window startup
- no external browser launch
- local file-based frontend loading
- no localhost dependency
- no application listening port
- project scanning
- findings generation
- project graph generation
- project model generation
- scan history
- folder selection
- ZIP selection
- JSON export
- operation without separately installed Python
- operation without Node.js or npm

The current Windows build also includes regression tests that prevent accidental reintroduction of browser launch logic, localhost-based APIs, or web-framework dependencies into the desktop runtime.

---

## Offline Operation

Core CodeDNA analysis is designed to work without an internet connection.

The analysis engine does not require an external API or local HTTP service for normal project scans.

Some operating-system runtime components may perform their own platform-level background activity independently of the CodeDNA analysis engine.

---

## Security Model

CodeDNA analyzes local source code directly from the selected path.

There is no file upload step in the desktop architecture.

Selected folders and ZIP archives are processed locally by the analysis engine.

---

## Repository Scope

This repository is intended as the Windows distribution and product showcase for CodeDNA.

It may include:

- release artifacts
- screenshots
- changelog
- documentation
- architecture notes

The core source code may remain private.

---

## Release Naming

Recommended release artifact:

```text
CodeDNA-Windows-x64.zip
```

Example release:

```text
v1.0.0
```

---

## Status

**Windows x64:** Available / validated desktop build

---

## License

See the `LICENSE` file included in this repository.
