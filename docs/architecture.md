# CodeDNA — Windows architecture

CodeDNA is a desktop application. It is not a localhost web application, and it
does not run a server of any kind.

## Runtime shape

```text
CodeDNA.exe
  └── native Windows desktop window
        └── embedded WebView2 (a rendering engine, not a browser)
              └── window.pywebview.api   (in-process JavaScript bridge)
                    └── CodeDNA analysis engine (same process)
```

The interface is a static production build loaded from disk as a `file://`
document. It reaches the analysis engine by calling bridge methods directly.
There is no HTTP layer, no API origin and no socket.

## What this means in practice

| | |
| --- | --- |
| Listening TCP or UDP ports | none |
| localhost / 127.0.0.1 in the release runtime | none |
| External browser windows | none |
| Web framework in the runtime | none |
| Separate Python installation | not required |
| Node.js or npm | not required |
| Network access for a scan | none |

`msedgewebview2.exe` helper processes appear while CodeDNA is running. They
belong to the embedded WebView2 runtime that renders the interface, not to a
separately opened Edge browser. CodeDNA passes switches that disable the
runtime's background networking, component updates, telemetry and sync.

## Why a WebView rather than a native toolkit

The interface is built with web technology and rendered by the platform's own
embedded WebView. This is native desktop packaging with a platform WebView — it
is not WinUI, and it is not a browser application. The distinction that matters
to a user is that the application opens as a normal desktop window with native
dialogs, and nothing on the machine or the network can reach it.

## Process and data model

- A scan reads the selected folder or archive in place. Nothing is uploaded.
- A `.zip` is extracted into the application's own data directory, with the
  traversal and size limits the ingestion layer applies to any archive.
- Analyzed code is parsed and inspected. It is never executed.
- Scan history, findings and settings are stored in a local SQLite database
  under `%LOCALAPPDATA%\CodeDNA\`.
- Exports are written only to a path chosen through the native save dialog.

## Analysis pipeline

```text
selected path
  └── ingestion        file discovery, provenance, archive handling
        └── inventory  language detection, extension and pattern matching
              └── project model
                     frameworks, libraries, entry points, security controls
                     └── analyzers
                            Python AST + taint / data flow
                            lightweight intra-file flow (JS, TS, PHP, HTML)
                            pattern rules (C#, Go, Java, SQL)
                            configuration and value analysis
                            secret detection
                            dependency and manifest analysis
                            └── findings, graph and Security DNA score
```

Each finding records where it came from, the evidence behind it, the data flow
observed, which security controls were absent on that path, and what the engine
could not establish.

## Offline behaviour

Core analysis requires no network connection. The only feature that would use
the network is optional dependency advisory lookup, which is disabled by
default. Operating-system components such as the WebView2 runtime may perform
their own platform-level activity independently of CodeDNA.

## Verification

The following are enforced by automated tests rather than by convention:

- no web-framework import anywhere in the packaged runtime;
- no listening socket during application startup;
- no browser-launch path in the interface;
- the interface URL is one the embedded WebView host will not serve over HTTP;
- the documented capability figures match what the engine reports at runtime.
