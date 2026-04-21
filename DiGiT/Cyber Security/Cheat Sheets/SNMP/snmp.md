# SNMP Enumeration

####  **SNMP Enumeration Cheat Sheet**[¶](https://hacknseek.gitlab.io/cheatsheets/snmp/#snmp-enumeration-cheat-sheet "Permanent link")

#####  **Basic SNMP Commands**[¶](https://hacknseek.gitlab.io/cheatsheets/snmp/#basic-snmp-commands "Permanent link")

|**Options**|**Commands**|**Description**|
|---|---|---|
|`-v1`|`snmpwalk -v1 -c public $target`|Perform an SNMP walk using SNMPv1 with the community string "public"|
|`-v2c`|`snmpwalk -v2c -c public $target`|Perform an SNMP walk using SNMPv2c with the community string "public"|
|`-v3`|`snmpwalk -v3 -u user -A password -l authPriv $target`|Perform an SNMP walk using SNMPv3 with authentication and privacy|
|`-c`|`snmpget -v2c -c public $target 1.3.6.1.2.1.1.1.0`|Perform an SNMP get request using SNMPv2c with the community string "public"|
|`-C`|`snmpbulkwalk -v2c -c public $target`|Perform a bulk SNMP walk to retrieve large amounts of data|
||`onesixtyone -c community.txt $target`|Enumerate SNMP community strings using the onesixtyone tool|

#####  **SNMP Discovery and Enumeration with Nmap**[¶](https://hacknseek.gitlab.io/cheatsheets/snmp/#snmp-discovery-and-enumeration-with-nmap "Permanent link")

|**Options**|**Commands**|**Description**|
|---|---|---|
||`nmap -sU -p 161 --script=snmp-brute $target`|Discover SNMP community strings by brute force|
||`nmap -sU -p 161 --script=snmp-info $target`|Gather general information from SNMP|
||`nmap -sU -p 161 --script=snmp-win32-services $target`|Enumerate running services on a Windows system via SNMP|
||`nmap -sU -p 161 --script=snmp-win32-shares $target`|Enumerate shared folders on a Windows system via SNMP|
||`nmap -sU -p 161 --script=snmp-sysdescr $target`|Retrieve the system description via SNMP|
||`nmap -sU -p 161 --script=snmp-netstat $target`|Retrieve network interface details via SNMP|

#####  **SNMP Community String Wordlists**[¶](https://hacknseek.gitlab.io/cheatsheets/snmp/#snmp-community-string-wordlists "Permanent link")

|**Path**|**Wordlist**|**Description**|
|---|---|---|
|`/SecLists/Discovery/SNMP/`|`snmp_default_pass.txt`|Common default SNMP community strings|
|`/SecLists/Discovery/SNMP/`|`snmp_default_community_strings.txt`|A larger list of default community strings|
|`/The-Wordlist-Collection/snmp/`|`common-snmp-community-strings.txt`|A general-purpose list of SNMP community strings|

#####  **Advanced SNMP Enumeration**[¶](https://hacknseek.gitlab.io/cheatsheets/snmp/#advanced-snmp-enumeration "Permanent link")

|**Options**|**Commands**|**Description**|
|---|---|---|
|`-c public`|`snmpwalk -c public -v2c $target 1.3.6.1.2.1.25.1.6.0`|Enumerate system uptime via SNMP|
|`-c public`|`snmpwalk -c public -v2c $target 1.3.6.1.4.1.77.1.2.25`|Enumerate user accounts on Windows systems via SNMP|
|`-c public`|`snmpwalk -c public -v2c $target 1.3.6.1.4.1.77.1.2.3.1.1`|Enumerate running processes via SNMP|
|`-c public`|`snmpwalk -c public -v2c $target 1.3.6.1.2.1.6.13.1.3`|Enumerate TCP connections via SNMP|
|`-c public`|`snmpwalk -c public -v2c $target 1.3.6.1.2.1.4.20.1.1`|Enumerate IP addresses via SNMP|
||`snmp-check $target -c public`|Perform a comprehensive SNMP check using snmp-check|

January 15, 2026 January 15, 2026