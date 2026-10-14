![logo](logo.png)

# CERT-SE CTF 2026
- https://www.cert.se/2026/10/antar-du-var-utmaning.html

## Description
As in previous years CERT-SE has put together a CTF during the cyber security month. The challenge is open to anyone with an interest in IT security regardless of the level of expertise.

In the .zip file is a network dump (PCAPNG) that contains a total of six flags. All of these have the format “ctf[string]” or “CTF[STRING]”. Everything needed to find the flags is included in the network dump and can be solved offline. Online references contain no clues; free external tools can be useful.

Send as many flags as you find, together with descriptions of how you solved the tasks, to cert@cert.se with subject “CTF2026”. Flag case does not matter. Only one response per person is accepted. The deadline is 31 October 2026.

**Files:** [CERT-SE_CTF2026.zip](./CERT-SE_CTF2026.zip)

# Challenges

**Flags:** (2/6)

## first_flag

1. Run `strings` on the capture and search for the flag prefix:
   ```sh
   cd Challenges
   strings CERT-SE_CTF_2026.pcapng | rg 'ctf\['
   ```
2. The flag is present in plaintext in the output.

***Flag: `ctf[first_flag]`***


## simple_dns_exfil

1. Inspect DNS traffic in Wireshark. Packet 2380 is an A query from `192.168.122.48` to `192.168.122.1` for `Y3RmW3NpbXBsZV9kbnNfZXhmaWxd.flag.com`. Packet 2381 replies with NXDOMAIN.
2. Base64 decode the encoded part of the domain name, `Y3RmW3NpbXBsZV9kbnNfZXhmaWxd`.

[Wireshark and CyberChef screenshot](./Challenges/simple_dns_exfil/solve.png)

***Flag: `ctf[simple_dns_exfil]`***


## Unknown 03
Challenge not identified yet. Working files: [Challenges/unknown_03/](./Challenges/unknown_03/).

***Flag: `not found`***


## Unknown 04
Challenge not identified yet. Working files: [Challenges/unknown_04/](./Challenges/unknown_04/).

***Flag: `not found`***


## Unknown 05
Challenge not identified yet. Working files: [Challenges/unknown_05/](./Challenges/unknown_05/).

***Flag: `not found`***


## Unknown 06
Challenge not identified yet. Working files: [Challenges/unknown_06/](./Challenges/unknown_06/).

***Flag: `not found`***

