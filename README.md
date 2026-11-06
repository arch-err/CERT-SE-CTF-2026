<img src="logo.svg" alt="NCSC / CERT-SE" width="100%">

# CERT-SE CTF 2026
- https://www.cert.se/2026/10/antar-du-var-utmaning.html

## Description
As in previous years CERT-SE has put together a CTF during the cyber security month. The challenge is open to anyone with an interest in IT security regardless of the level of expertise.

In the .zip file is a network dump (PCAPNG) that contains a total of six flags. All of these have the format “ctf[string]” or “CTF[STRING]”. Everything needed to find the flags is included in the network dump and can be solved offline. Online references contain no clues; free external tools can be useful.

Send as many flags as you find, together with descriptions of how you solved the tasks, to cert@cert.se with subject “CTF2026”. Flag case does not matter. Only one response per person is accepted. The deadline is 31 October 2026.

**Files:** [CERT-SE_CTF2026.zip](./CERT-SE_CTF2026.zip)

# Challenges

**Flags:** (6/6)

## string

1. Start with the obvious:
   ```sh
   cd Challenges
   strings CERT-SE_CTF_2026.pcapng | rg 'ctf\['
   ```

Turns out it's literally appended to the end of the pcapng, newline and all. That's also why Wireshark complains that the file is corrupt. Remove those last 16 bytes and the capture works fine.

***Flag: `ctf[first_flag]`***


## dns

1. Look through the DNS traffic in Wireshark. Packet 2380 asks for `Y3RmW3NpbXBsZV9kbnNfZXhmaWxd.flag.com`. That first part looks pretty familiar...
2. Copy it into [CyberChef and decode from Base64](<https://cyberchef.3rr.dev/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=WTNSbVczTnBiWEJzWlY5a2JuTmZaWGhtYVd4ZA>).

[Wireshark + CyberChef](./Challenges/dns/solve.png)

***Flag: `ctf[simple_dns_exfil]`***


## whitespace

1. Export [Om_oss.odt](./Challenges/loot/binary/Om_oss.odt) from the HTTP traffic.
   > Wireshark > File > Export Objects > HTTP
2. Open it. Some text about CERT-SE, followed by a whole bunch of blank pages... `Ctrl+A`, hmm, lots of whitespace here!
3. Peek inside the ODT's `content.xml`. The blank paragraphs aren't all the same: some have tabs, some don't. Save that [whitespace](./Challenges/loot/odt-whitespace/whitespace.txt), keeping tabs and newlines.
4. [CyberChef time](<https://cyberchef.3rr.dev/#recipe=Find_/_Replace(%7B'option':'Regex','string':'%5C%5Ct'%7D,'1',true,false,true,false)Find_/_Replace(%7B'option':'Regex','string':'%5C%5Cn'%7D,'0',true,false,true,false)Find_/_Replace(%7B'option':'Regex','string':'0$'%7D,'',true,false,true,false)From_Binary('Space',8)&input=CgkJCgoKCQkKCQkJCgkKCgoJCQoKCQkKCgkKCQkKCQkKCQkKCQkJCgoJCQoJCQkJCgkJCQoJCgoKCQoJCQkJCQoJCQoKCQoJCgkJCQoJCQoKCQkKCgkKCQoJCQkKCgkKCgkJCQkKCgkKCQoJCQkJCQoJCQkKCQkJCgkJCgkKCgoKCQkKCQoKCQoJCQkKCQoKCgkJCgoJCgkKCQkJCgoJCQoJCQkKCgoKCgkJCgoKCgkKCQkKCgoJCQoJCQoKCQoJCgkKCQkJCQkKCQkKCQoKCQoJCQkKCgkJCgkKCQkJCQkKCQkJCgkKCgoJCQoJCgoKCgkJCgoJCgkKCQoJCQkJCQoJCQkKCgkJCgkJCgoKCgkKCQkKCQkKCQoJCQoKCQoJCgkKCQkJCgkK>): replace tabs with `1`, newlines with `0`, remove the extra final zero, then **From Binary**, 8 bits per byte.

***Flag: `ctf[not_every_whitespace_is_the_same]`***


## png

1. Export [white_wallpaper.png](./Challenges/loot/binary/white_wallpaper.png) from HTTP
2. Throw it into StegSolve. Different planes don't get me anywhere, but random color mappings show dots scattered all over the place. So there is *something* there!
> Pro tip: bring sunglasses next time you ctf at night in a dark room...
3. Binwalk, zsteg, pngcheck and metadata don't turn up anything useful. Guess i gotta check those rando pixels by themselves...
4. There are 208 pixels that aren't quite white. Either red or blue is 254 instead of 255, and each of these pixels has a different alpha value. Conveniently, those alpha values are exactly 1–208.
5. Sort by alpha. Red=254 means `1`, blue=254 means `0`. Feed the [208 bits into CyberChef](<https://cyberchef.3rr.dev/#recipe=From_Binary('Space',8)&input=MDExMDAwMTEwMTExMDEwMDAxMTAwMTEwMDEwMTEwMTEwMTEwMTAwMDAxMTAxMDAxMDExMDAxMDAwMTEwMDEwMDAxMTAwMTAxMDExMDExMTAwMTAxMTExMTAxMTAxMDAxMDExMDExMTAwMTAxMTExMTAxMTEwMDAwMDExMDExMDAwMTEwMDAwMTAxMTAxMDAxMDExMDExMTAwMTAxMTExMTAxMTEwMDExMDExMDEwMDEwMTEwMDExMTAxMTAxMDAwMDExMTAxMDAwMTAxMTEwMQ>) with **From Binary**.

[Bits and notes](./Challenges/png/)

***Flag: `ctf[hidden_in_plain_sight]`***


## icmp

1. Notice there's a shitton of ping traffic. Filter for requests: `icmp.type == 8`.
2. The replies just return the payload back. Nothing interesting there, but the request TTLs keep changing between 64 and 128...
3. Try treating those as binary. Nope, doesn't give anything useful... 64=. 128=_? Yeh, turns out it's **Morse**.
4. Timing separates the letters! Requests are about 0.2 seconds apart within a letter, then there's a roughly 6-second gap before the next one. [Morse groups + frame numbers](./Challenges/icmp/morse-groups.json).
5. Decode the Morse: `INKEMW2UJFGUSTSHL5EVGX2GKVHF2`. That's Base32, so [continue in CyberChef](<https://cyberchef.3rr.dev/#recipe=From_Base32('A-Z2-7%3D',true)&input=SU5LRU1XMlVKRkdVU1RTSEw1RVZHWDJHS1ZIRjI9PT0>).

***Flag: `CTF[TIMING_IS_FUN]`***


## 403

1. Extract a [list of all the HTTP URLs](./Challenges/loot/urls.txt). Notice a bunch of requests to `<ip>/<single-random-letter>/filename`. That's a bit sus, but there's a lot of noise in there too.
2. Spend a while staring at the extracted HTML. Mostly copies of Wikipedia and other public sites, under filenames that have very little to do with their contents. Surely there's something in HTTP we've missed?
3. Look at the status codes instead: `http.response.code == 403`. There are 22 of them, all with those single-char directories. Extract another list, this time just the [URLs that returned 403, in request order](./Challenges/403/urls.txt).
4. Read the directory chars and join them. `%5B` and `%5D` are just `[` and `]`.

Yeah... probably should've noticed that when staring at the URL list earlier.

***Flag: `ctf[inspect_the_codes]`***
