# Loot

`plaintext/` contains 1,069 text objects exported from `../CERT-SE_CTF_2026.pcapng` with Tshark 4.6.7 (HTTP, FTP data, SMB, IMF and TFTP object exporters). Recovered content is unchanged. HTML is retained as source, not rendered or stripped.

Exporter filenames are preserved, including numeric suffixes for collisions. URL escaping is removed and query strings are dropped (`success.txt%3fipv4` becomes `success.txt`). No context-based filenames were needed.

`manifest.json` records each filename, exporter filename/protocol, media type, size and SHA-256. Classification uses `file --mime-type` and includes `text/*` only. Binary objects are retained separately under `.recovery/`; they have not been opened or decoded. Export logs and HTTP metadata are also there.

Tshark reports an invalid PCAPNG block length after 19,007 packets. These files cover objects recoverable before that error; this is not a guarantee that every file in the supplied capture was recovered. No encrypted traffic, embedded encodings or document archives were decoded, and no recovered content was executed.
