# Development Roadmap

A four-week part-time target is a planning estimate for the archive and supported file-transfer MVP. Native import research is open-ended and may conclude that the required workflow is not currently feasible.

## Week 1 — Foundation and parser

- Create the Python package and PySide6 main window.
- Add file/folder selection for exported chats.
- Parse messages, senders, timestamps, multiline content, and attachment references where supported.
- Handle multiple date formats and malformed lines.
- Create synthetic fixtures and parser tests.

**Deliverable:** Imported chat appears in the application with parsing warnings.

## Week 2 — Media and archive

- Match accessible attachments to messages where possible.
- Generate a searchable local HTML archive.
- Show missing and unsupported attachments.
- Add file-size limits and duplicate detection.
- Calculate SHA-256 checksums.

**Deliverable:** Readable conversation archive and media inventory.

## Week 3 — Supported iPhone file delivery

- Build transfer wizard and preflight checks.
- Implement a temporary, authenticated local transfer workflow.
- Allow the user to receive supported files through Safari or another supported iOS workflow.
- Add progress, cancellation, retry, and transfer manifest.
- Verify received files where the destination workflow permits it.

**Deliverable:** Supported files delivered to an existing iPhone without erasing it.

## Week 4 — Hardening and packaging

- Add recovery after interrupted transfers.
- Test Unicode filenames, large media, duplicate files, and malformed inputs.
- Complete privacy and security tests.
- Package a Windows executable with PyInstaller.
- Write setup instructions, feature limitations, and release notes.

**Deliverable:** Distributable MVP with documented capabilities.

## Ongoing — Native import feasibility

- Review official migration documentation and supported integrations.
- Maintain a feasibility matrix.
- Test only on disposable test data and devices.
- Record exact app/OS versions and results.
- Keep experimental work isolated.

**Exit criterion:** A verified, legitimate native-import mechanism is demonstrated—or the project documents that the no-reset native-import requirement is not currently feasible.

## Recommended build order

1. Parser and normalized data model.
2. Attachment matching and inventory.
3. HTML archive and local search.
4. Integrity checks and reports.
5. iPhone file delivery.
6. Recovery and security hardening.
7. Native-import research, in parallel but isolated.

Do not add AI, cloud services, user accounts, or subscriptions to v0.1 unless a concrete requirement justifies them.
