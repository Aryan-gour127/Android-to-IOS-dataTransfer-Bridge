# WhatsAppBridge

**Windows desktop · Python · Android → iPhone · No-reset migration research**

WhatsAppBridge is a privacy-first desktop project for organizing user-exported WhatsApp chats and media, creating readable archives, validating files, and helping users transfer supported content to an already configured iPhone.

> **Important limitation:** WhatsAppBridge v0.1 does not import chat history into the native iPhone WhatsApp app. Copying media or generating an HTML archive is not the same as restoring native WhatsApp conversations. Treat no-reset native import as a research goal until a supported, legitimate mechanism is verified.

## Goals

- Import user-exported WhatsApp text chats and accessible media.
- Parse messages, timestamps, senders, and attachment references when available.
- Organize conversations into a readable local archive.
- Help transfer supported files to an existing iPhone without erasing it.
- Verify files and report failures.
- Keep personal data local by default.

## MVP scope

| Capability | v0.1 |
|---|---|
| Import exported `.txt` chats | Planned |
| Import associated media | Planned |
| Preview and search archive | Planned |
| Generate local HTML archive | Planned |
| Transfer supported files to iPhone | Planned |
| Verify files with SHA-256 | Planned |
| Restore messages inside native WhatsApp | **Not supported** |
| Merge old Android history into existing iPhone chats | **Not supported** |
| Access every private WhatsApp database | **Not supported** |

## Proposed stack

- Python 3.12+
- PySide6 desktop UI
- SQLite for local project metadata
- Python standard library for initial parsing
- `hashlib` for SHA-256 integrity checks
- `pytest` for tests
- PyInstaller for Windows packaging
- Optional Android Debug Bridge (ADB) for device detection and supported accessible files

## Architecture

The application is a modular monolith. The native-import research adapter must remain isolated from the supported archive and file-transfer pipeline.

```text
PySide6 UI
   ↓
Application services
   ├── Android export adapter
   ├── Chat parsing and normalization
   ├── Archive generation
   ├── Transfer planner / executor / verifier
   ├── Privacy and validation
   └── Experimental native-import research adapter
   ↓
Local workspace + SQLite metadata
```

## Development

Create a virtual environment and install the project dependencies once `pyproject.toml` is configured.

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

## Documentation

- [Architecture](docs/architecture.md)
- [Supported features](docs/supported-features.md)
- [Privacy and security](docs/privacy-policy.md)
- [Native import research](docs/native-import-research.md)
- [Development roadmap](docs/roadmap.md)
- [Testing plan](docs/testing.md)
- [Data model](docs/data-model.md)

## Status

This repository contains project planning documentation. Features marked “planned” are not implemented merely because they appear in these documents.
