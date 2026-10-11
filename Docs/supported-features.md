# Supported Features and Capability Boundaries

This document describes the intended scope. It is not proof that planned features are already implemented.

## MVP capabilities

| Feature | Target status | Notes |
|---|---|---|
| Import user-exported chat text | Planned | Export format may vary by OS and app version |
| Import accessible media | Planned | Requires files supplied by the user or exposed by the device |
| Parse messages and timestamps | Planned | Some formats and message types may not be recognized |
| Organize chats and groups | Planned | Display names may be available; internal WhatsApp IDs may not be |
| Local HTML archive | Planned | Readable archive, not a native WhatsApp conversation |
| Local search | Planned | Search within imported archive |
| Transfer supported files to iPhone | Planned | Destination depends on file type and iOS workflow |
| SHA-256 verification | Planned | Verify bytes where source and destination can both be read |
| Progress and recovery report | Planned | Must clearly show partial failures |

## Not supported in v0.1

- Importing complete Android chat history into the native iPhone WhatsApp app.
- Merging Android history into an already existing iPhone WhatsApp history.
- Recreating original WhatsApp message IDs, read states, all group metadata, or every edit/deletion.
- Accessing every private WhatsApp database on a non-rooted Android phone.
- Writing directly into WhatsApp's private iOS app container.
- Guaranteeing that every message, attachment, or special message type can be exported and reconstructed.

## Important distinction

**Media transfer is not native chat migration.** A copied photo or a generated HTML transcript does not appear as an original message in a WhatsApp conversation.

The standard Android-to-iPhone WhatsApp migration workflow uses Apple's Move to iOS during iPhone setup. WhatsAppBridge must not claim to bypass this requirement unless a supported alternative is verified and tested.

## Capability labels for the UI

Use consistent labels:

- **Supported:** Implemented and tested for the described workflow.
- **Limited:** Works only for documented formats or file types.
- **Experimental:** Research-only and disabled by default.
- **Unsupported:** No verified mechanism is available in the current release.

Never label a planned feature “Supported” until implementation and testing are complete.
