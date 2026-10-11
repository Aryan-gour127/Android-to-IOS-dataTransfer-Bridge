# Data Model

The model below is a starting point for normalizing exported chats. It is not a complete WhatsApp database schema.

## Python model sketch

```python
from dataclasses import dataclass, field
from datetime import datetime


@dataclass
class Attachment:
    filename: str
    local_path: str | None = None
    media_type: str = "unknown"
    sha256: str | None = None


@dataclass
class Message:
    timestamp: datetime | None
    sender: str | None
    text: str
    attachments: list[Attachment] = field(default_factory=list)
    source_line: int | None = None


@dataclass
class Conversation:
    conversation_id: str
    title: str
    messages: list[Message] = field(default_factory=list)


@dataclass
class TransferReport:
    files_found: int = 0
    files_verified: int = 0
    files_failed: int = 0
    warnings: list[str] = field(default_factory=list)
```

## Model limitations

- Exported text may not include a stable internal conversation ID.
- Sender names may be display names rather than stable identifiers.
- Export formats may differ by locale, OS version, or app version.
- Message IDs, read states, all edit/deletion details, and complete group metadata may be absent.
- Attachment filenames may be duplicated or changed during export.
- Some media may be omitted from a chat export.

## Persistence guidance

Use SQLite for project metadata, file inventory, transfer status, and checksums. Do not store message bodies in the database unless the user chooses to retain a searchable archive.

## Transfer report

A transfer report should record:

- Number of files discovered, selected, copied, verified, skipped, and failed.
- Warnings and unsupported file types.
- Source and destination checksums where available.
- Start/end timestamps and final status.
- No message contents, passwords, OTPs, or unnecessary personal information.
