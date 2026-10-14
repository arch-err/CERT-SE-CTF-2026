# Loot

`plaintext/` contains 1,069 text objects exported from `../CERT-SE_CTF_2026.pcapng` with Tshark 4.6.7 (HTTP, FTP data, SMB, IMF and TFTP object exporters). Recovered content is unchanged. HTML is retained as source, not rendered or stripped.

Exporter filenames are preserved, including numeric suffixes for collisions. URL escaping is removed and query strings are dropped (`success.txt%3fipv4` becomes `success.txt`). No context-based filenames were needed.

`manifest.json` records each filename, exporter filename/protocol, media type, size and SHA-256. Classification uses `file --mime-type` and includes `text/*` only. Binary objects are retained separately under `.recovery/`; they have not been opened or decoded. Export logs and HTTP metadata are also there.

The original capture has 16 non-PCAPNG bytes appended after its final valid block: `ctf[first_flag]` followed by a newline. All 19,007 packet blocks are valid; the parse error did not hide additional packets. `CERT-SE_CTF_2026.repaired.pcapng` removes only that suffix and parses without errors, including all 198 ICMP packets. The suffix is preserved in `capture-trailing-bytes.txt`, and offsets, block counts and hashes are recorded in `capture-repair.json`. Original capture and ZIP are unchanged.

Object recovery still covers only the supported exporters and plaintext traffic. No encrypted traffic, embedded encodings or document archives were decoded, and no recovered content was executed.

`binary/` contains the non-plaintext objects recovered from the repaired capture, with its own manifest and recovery notes.
