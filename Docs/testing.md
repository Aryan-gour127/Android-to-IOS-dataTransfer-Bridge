# Testing Plan

## Parser tests

- Different timestamp formats and locale variations.
- Unicode, emoji, right-to-left text, and long messages.
- Multiline messages and system messages.
- Group messages and messages with missing sender labels.
- Unknown message formats and malformed lines.
- Empty files, large files, and invalid encodings.

## Attachment tests

- Present and missing attachments.
- Duplicate filenames.
- Unicode and unusually long filenames.
- Unsupported extensions and mismatched file types.
- Large videos and zero-byte files.
- Source/destination hash matches and mismatches.

## Transfer tests

- Successful transfer.
- User cancellation.
- Network interruption and retry.
- Duplicate transfer and overwrite confirmation.
- Partial transfer and recovery.
- Destination storage or permission failure.
- Session expiry and unauthorized requests.

## Security tests

- Path traversal attempts such as `../../file`.
- Malicious filenames and archive entries.
- Unexpected symbolic links.
- Oversized payloads.
- Unauthorized requests to the local transfer server.
- Expired or invalid session tokens.
- Sensitive data appearing in logs.
- Temporary-file cleanup behavior.

## Device compatibility

Test with real Android and iPhone devices before claiming compatibility. Record device model, OS version, WhatsApp version, export method, file types, and test results. Use synthetic or disposable test data where possible.

## Release checklist

- [ ] Chat parser handles documented input formats.
- [ ] Unsupported records generate warnings.
- [ ] Missing attachments are reported.
- [ ] Hash verification works.
- [ ] Interrupted transfers do not silently appear successful.
- [ ] Unsafe paths are rejected.
- [ ] Transfer sessions expire.
- [ ] Logs redact sensitive data.
- [ ] UI clearly distinguishes file transfer from native WhatsApp import.
- [ ] README accurately describes tested compatibility.
