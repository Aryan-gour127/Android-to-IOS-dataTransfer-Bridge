# Privacy and Security Design

WhatsApp exports can contain private messages, contact information, images, videos, voice notes, documents, and other sensitive data. Privacy must be part of the architecture.

## Data handling principles

- **Local-first:** No cloud upload by default.
- **User consent:** Show which source files and destination files will be processed.
- **Data minimization:** Keep only metadata required for the project unless the user chooses to retain an archive.
- **No credentials:** Never request WhatsApp passwords, one-time verification codes, or Apple Account credentials.
- **Transparent cleanup:** Explain what temporary data is removed and what remains.
- **Safe diagnostics:** Logs should not contain message bodies, OTPs, passwords, or unnecessary personal paths.

## Temporary transfer server

If a local-network transfer server is implemented:

1. Start it only after explicit user action.
2. Bind to the intended local interface rather than all interfaces by default.
3. Require a high-entropy, per-session token.
4. Use an expiring session and stop the server when the transfer ends.
5. Limit allowed file paths, file types, and transfer sizes.
6. Reject path traversal and unexpected symbolic links.
7. Protect the channel appropriately and document the threat model.
8. Never describe plain HTTP on a shared network as end-to-end encrypted.

## File validation

- Treat filenames and archive contents as untrusted input.
- Normalize destination paths and confirm they remain inside the chosen workspace.
- Enforce configurable size limits.
- Handle malformed text and invalid encodings safely.
- Verify file hashes when possible.
- Report mismatches rather than silently accepting files.

## Local storage

Store project metadata, source references, checksums, and transfer state in SQLite. Avoid persisting message bodies in the database unless the user opts into a retained, searchable archive.

## Temporary-file deletion

Offer a cleanup option and document its limitations. Secure deletion cannot be guaranteed on SSDs, copy-on-write filesystems, backups, or systems with snapshots.

## Security testing

Test path traversal, malicious filenames, oversized files, interrupted transfers, unauthorized local requests, expired tokens, log redaction, and accidental overwrite behavior.
