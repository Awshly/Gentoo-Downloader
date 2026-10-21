# Contributing

- Keep downloads restricted to official Gentoo HTTPS infrastructure by default.
- Never automatically trust a signing key downloaded alongside an ISO.
- Keep resumable partial files on failure.
- Never print a success/verified message until signature and checksum validation both pass.
- Add tests for parsing, bad signatures, wrong hashes, interrupted downloads, and malformed server responses.
- Do not claim support for a media type until the current official directory and verification metadata have been tested.
