# WhatsAppBridge Architecture

## Design principles

1. **Local-first:** Process user-selected exports on the Windows PC by default.
2. **Modular monolith:** Keep v0.1 in one Python application with clear module boundaries.
3. **Explicit capability boundaries:** File transfer and archive generation are separate from native WhatsApp import.
4. **Recoverability:** Transfers should support progress, cancellation, retries, and clear reports.
5. **Least privilege:** Access only files and devices the user explicitly authorizes.

## High-level components

```text
┌────────────────────────────────────────────┐
│              PySide6 Desktop UI            │
│ Dashboard · Devices · Import · Preview     │
│ Transfer Wizard · Settings · Logs          │
└──────────────────────┬─────────────────────┘
                       ↓
┌────────────────────────────────────────────┐
│             Application Services           │
│ Orchestration · Validation · Jobs          │
│ Progress · Cancellation · Recovery         │
└────────────┬───────────────┬───────────────┘
             ↓               ↓
┌────────────────────┐  ┌────────────────────┐
│ Android Adapter    │  │ Chat Engine        │
│ Export import      │  │ Parser / Normalizer│
│ Optional ADB       │  │ Attachments / HTML │
└────────────────────┘  └────────────────────┘
             ↓               ↓
┌────────────────────┐  ┌────────────────────┐
│ Transfer Services  │  │ Privacy & Integrity│
│ Plan / Execute     │  │ SHA-256 / Redaction│
│ Verify / Recover   │  │ Validation / Cleanup│
└────────────┬───────┴───────────────┬────────┘
             ↓                       ↓
       ┌──────────────────────────────────┐
       │ Local workspace + SQLite metadata│
       └──────────────────────────────────┘
```

## Proposed stack

| Component | Technology | Purpose |
|---|---|---|
| Runtime | Python 3.12+ | Windows desktop application |
| UI | PySide6 | Native-feeling desktop UI and background workers |
| Metadata | SQLite | Project state, file inventory, transfer status |
| Parsing | Standard library initially | Parse exported chat text |
| Device detection | Optional ADB | Detect Android devices and accessible files |
| Integrity | `hashlib` | SHA-256 checksums |
| Tests | pytest | Unit, integration, and security tests |
| Packaging | PyInstaller | Build a Windows executable |

Pin dependency versions in project configuration and use a virtual environment.

## Suggested package structure

```text
src/whatsappbridge/
├── main.py
├── app.py
├── ui/
│   ├── main_window.py
│   ├── dashboard.py
│   ├── device_page.py
│   ├── import_page.py
│   ├── chat_preview.py
│   ├── transfer_wizard.py
│   └── settings_page.py
├── core/
│   ├── models.py
│   ├── project_service.py
│   ├── job_manager.py
│   └── exceptions.py
├── adapters/
│   ├── android/
│   │   ├── adb.py
│   │   └── export_import.py
│   ├── ios/
│   │   ├── device_detection.py
│   │   └── file_delivery.py
│   └── migration/
│       ├── archive.py
│       └── native_import.py
├── chat/
│   ├── parser.py
│   ├── normalizer.py
│   ├── attachments.py
│   └── html_exporter.py
├── transfer/
│   ├── planner.py
│   ├── executor.py
│   ├── verifier.py
│   └── recovery.py
├── storage/
│   ├── database.py
│   └── workspace.py
└── security/
    ├── validation.py
    ├── redaction.py
    └── cleanup.py
```

## Data flow

1. User selects an exported chat and optional media folder.
2. Validate inputs, file types, sizes, and paths.
3. Parse and normalize message records.
4. Match attachment references when possible.
5. Present a transfer plan and warnings for user review.
6. Copy only selected, supported files through a user-mediated destination workflow.
7. Verify file integrity where the receiving workflow allows it.
8. Generate a report and optional HTML archive.

The core pipeline should work offline after the source files are available.

## Android and iOS constraints

- ADB detection does not grant unrestricted access to WhatsApp's private database on a standard, non-rooted Android device.
- iPhone file delivery must use supported, user-mediated methods and respect iOS permissions.
- Do not assume Windows can directly write into WhatsApp's private iOS app container.
- Native WhatsApp import remains isolated and disabled unless a supported, legitimate mechanism is demonstrated.

## Failure handling

Use typed application errors for invalid exports, inaccessible files, unsupported formats, transfer interruptions, hash mismatches, and permission errors. Never silently mark a failed or partial transfer as successful.
