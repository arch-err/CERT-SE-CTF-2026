# Non-plaintext objects

Exported from the repaired capture with Tshark 4.6.7 using all supported object exporters: HTTP, FTP data, SMB, IMF, TFTP, DICOM and X.509.

- `white_wallpaper.png`: 4096 × 2160 PNG, 42,397 bytes.
- `Om_oss.odt`: OpenDocument Text, 50,010 bytes; retained as the original archive.
- `ocsp-request.der` and `ocsp-response.der`: 83-byte request and 471-byte response; inferred names from HTTP content types.
- `certificates/`: 12 DER certificate objects, including repeated certificates. Exporter filenames retained.

`manifest.json` records filenames, sizes, hashes and HTTP provenance. All bytes are unchanged. No extracted content was executed, and the ODT archive has not been unpacked. These are all non-text objects emitted by the supported exporters; encrypted content and files requiring custom reconstruction are not covered.
