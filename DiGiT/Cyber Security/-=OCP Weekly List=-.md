Updated Box List — Modern Replacements
Fair call. Kioptrix 1 specifically has issues because mod_ssl 2.8.4 exploits depend on toolchains that don't exist anymore (gcc versions, OpenSSL headers, etc.). It's a known frustration. Let me give you a revised, modern list that runs cleanly on current Kali.
Revised Weekly Schedule
Week 1: Methodology Calibration
Skip Kioptrix 1. Start with these — all run cleanly on modern Kali, exploits work without compiler gymnastics.

Basic Pentesting 1 (VulnHub, 2017) — clean introduction, multiple paths, no exploit-compile hell
Basic Pentesting 2 (VulnHub, 2018) — slightly harder, great enumeration practice
Mr-Robot 1 (VulnHub, 2016) — WordPress, dictionary attacks, classic web pattern

Walkthroughs:

IppSec on YouTube for Mr-Robot (search "IppSec Mr-Robot")
CryptoCat for Basic Pentesting series
0xdf's blog for write-style reference

Week 2: Linux Privesc Depth
All modern, all relevant.

DC-1 (VulnHub, 2019) — Drupal, classic privesc
DC-2 (VulnHub, 2019) — git, rbash escape, sudo abuse
DC-3 (VulnHub, 2019) — Joomla, kernel exploit
FristiLeaks 1.3 (VulnHub, 2015) — still solid, runs fine

Watch alongside:

Tib3rius — Common Linux Privilege Escalation (YouTube, free version)
John Hammond — Linux Privilege Escalation videos

Week 3: Web Exploitation Patterns

DC-4 (VulnHub, 2019) — brute force, command injection, teehee privesc
DC-5 (VulnHub, 2019) — LFI to RCE, screen exploit
Stapler 1 (VulnHub, 2016) — multiple paths, excellent for trying different approaches
Symfonos 1 (VulnHub, 2019) — SMB, LFI, mail exploitation

Week 4: Mid-Difficulty + Chaining

Symfonos 2, 3 (VulnHub, 2019) — varied techniques
DC-6, DC-7 (VulnHub, 2019) — keep climbing the DC ladder
Sunset: Dawn (VulnHub, 2020) — privesc focused, modern
Raven 1 (VulnHub, 2018) — WordPress, MySQL, classic chain
Raven 2 (VulnHub, 2018) — PHPMailer CVE-2016-10033 (real-world relevant)

Week 5: Harder VulnHub + Discipline

DC-8, DC-9 (VulnHub, 2019) — top of the DC series
Sunset: Midnight, Decoy, Nightfall (VulnHub, 2020) — modern privesc challenges
Tr0ll 1, 2 (VulnHub, 2014–2016) — rabbit holes (exam-relevant skill)
Brainpan 1 (VulnHub, 2013) — still useful conceptually
HackLAB: Vulnix (VulnHub, 2012) — NFS abuse, still works

Week 6: Modern Windows Practice (Free Tier)
VulnHub has limited Windows content, but here's the cost-free way to get Windows practice:

HackTheBox Starting Point (free tier) — guided easy boxes, includes Windows: Tier 1 has Crocodile (Linux), Ignition (Linux), Pennyworth (Linux); Tier 2 has Windows boxes like Funnel, Mongod, and a few others rotate in
TryHackMe free rooms — Blue (EternalBlue), Ice, Steel Mountain, Vulnversity — all free, all Windows-focused for some, all modern
0xdf's blog walkthroughs — read along with retired HTB Windows boxes (Legacy, Blue, Devel, Jerry) even if you can't spin them up

Week 7+: GOAD and Active Directory
Same plan as before — you already have GOAD running.
VulnHub Boxes to Avoid
These have outdated dependency hell similar to Kioptrix 1:

Kioptrix 1 — mod_ssl exploit compilation issues
Kioptrix 2 — partial issues with some exploits
De-ICE series — too old, broken networking
pWnOS 1.0 — outdated, pWnOS 2.0 is fine
Holynix — too old
Anything pre-2013 unless specifically noted above

VulnHub Boxes That Are Worth It (Modern, Reliable)
Master list of boxes that just work on current Kali, organized roughly by difficulty:
Beginner:

Basic Pentesting 1, 2
Mr-Robot
FristiLeaks 1.3
DC-1
Toppo 1
Symfonos 1

Easy-Medium:

DC-2, DC-3, DC-4, DC-5
Raven 1, 2
Stapler 1
Symfonos 2, 3
Sunset: Dawn
Vulnix

Medium:

DC-6, DC-7, DC-8, DC-9
Sunset: Decoy, Midnight, Nightfall
Tr0ll 1, 2
pWnOS 2.0
SkyTower
Lord of the Root
Symfonos 4, 5

Medium-Hard:

Brainpan 1
VulnOS 2 (some exploits dated, but workable)
Node 1

Bonus: TryHackMe Free Path (Worth Knowing)
Since you're not spending money, TryHackMe has surprisingly good free content. These rooms specifically are free and very OSCP-aligned:

Vulnversity — enumeration, privesc basics
Basic Pentesting — methodology
Kenobi — SMB, NFS, privesc
Steel Mountain — Windows AD-adjacent
Blue — EternalBlue (Windows, free)
Ice — Windows, free
Mr Robot CTF — full box
Pickle Rick — quick web/CTF
Brooklyn Nine Nine — quick easy
Game Zone — SQL injection, port forwarding

You can build a solid 2–3 weeks of practice on just free TryHackMe rooms in parallel with VulnHub.
Suggested Restart
Given you've been fighting Kioptrix for hours, I'd recommend:

Tonight: Spin up Basic Pentesting 1 instead. It's modern (Ubuntu 16.04ish), VirtIO works, no RTL8139 voodoo. Should be on the network within minutes of import.
Watch CryptoCat's Basic Pentesting 1 walkthrough on YouTube alongside it
Then: Mr-Robot, then DC-1

By the end of this weekend you should have three boxes rooted and your methodology rhythm starting to form. That's the goal — momentum over perfection.
For each box, the workflow stays the same

Import VMDK/OVA into Proxmox (use E1000 or VirtIO for these newer boxes — they all support it)
Attach to vmbr1002, no VLAN, no firewall flag
Boot, let it grab DHCP from pfSense
From Kali: nmap -sn 10.50.10.0/24 to find it
Then nmap -sC -sV -p- <ip> to enumerate
Try solo for 30–60 min
Walkthrough to unblock or verify after rooting
Write notes in your own words

Want me to walk through the import for a specific one?
Basic Pentesting 1 should be the easiest "restart" box — modern Ubuntu, no driver issues, clean exploits. If you grab the OVA from VulnHub, the import process to Proxmox is a bit different than VMDK (OVA = tar of OVF + VMDKs). Happy to walk you through that if needed.
Or if you want to keep the VulnHub-fighting energy going, DC-1 is the cleanest "first real OSCP-like box" — Drupal exploit via Drupalgeddon2, find-suid privesc, fully reliable on modern Kali. Excellent IppSec-style walkthroughs exist on YouTube.
What's your preference — fresh start with Basic Pentesting 1, or jump to DC-1?