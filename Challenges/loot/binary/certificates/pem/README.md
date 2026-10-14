# PEM certificates

All 12 original DER certificates were converted to PEM. Each conversion was read back with OpenSSL and verified to reproduce the original DER bytes exactly. The originals remain unchanged one directory above.

Each `.pem` has a matching `.txt` with OpenSSL's complete decoded certificate fields. `unique-certificates.pem` contains the five unique certificates, without duplicate copies; this is a collection, not a verified chain. `manifest.json` maps originals to PEM files and records subjects, issuers, dates, serials and SHA-256 fingerprints.

Inspect a certificate with `openssl x509 -in FILE.pem -noout -text`.
