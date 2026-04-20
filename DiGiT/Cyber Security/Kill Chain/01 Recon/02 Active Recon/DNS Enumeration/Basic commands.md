
DNS Info:
```
A => Address Record (IPv4) AAAA => Address Record (IPv6) CNAME => Canonical Name Record MX => Mail Exchange Record NS => Name Server Record PTR => Pointer Record SOA => Start of Authority record SRV => Service Location record TXT => Text Record Axfr => Zone Transfer. Includes all records about a domain

```
# Host Command

```
host <domainName.com> # Get info of domain
host -t ns <omainName.com> # Find name servers of a domain
host -t cname <domainName.com> # Find CNAME's
host -t txt <domainName.com> # Find text records
host -a <domainName.com> # Query all the records for domain
host -v -t a <domainName.com> # Find the TTL of a domain
host -6 <domainName.com> # Set IPv6 Protocols
host -4 <domainName.com> # Set IPv4 Protocol
```
# nslookup

You can either use -type or -query, does the same thing

Lookup nameserver records:
```
nslookup -type=ns <domainName.com> #Lookup nameserver records
nslookup -type=soa <domainName.com> # Search SOA records
nslookup -type=mx <domainName.com> # Mail Records
nslookup -type=any <domainName.com> # All records
```
# dig command

To get a specific type of record:
```
dig <domainName.com> # Basic info
dig <domainName.com> axfr # Attempt to do a domain transfer
dig axfr <domainName.com> @nameserver
dig <domainName.com> A #get A records
dig <domainName.com> AAA # get AAA records
dig <domainName.com> NS # name Server
dig <domainName.com> SOA # SOA Recrods
dig <domainName.com> any # get all records
dig <domainName.com> any |grep MX # get all records and grep for MX records
dig <domainName.com> +short # Displays limited info
dig -f /root/Desktop/Files/domain.txt # Queries a list of domains at once
```

## Domain transfer

### host command
DNS Recon using Zone Transfer: 
host Command: 
First identify the name server of the target domain, then run: 

```
host -l <targetDomain.com> <nameserverAddress> # DNS transfer
```

### dig Command
```
dig ns <targetdomain.com> 
```

then run: 
```
dig axfr <targetDomain.com> @<targetdomainNameServer>
```

### nslookup Command
```
nslookup 
>>> set type=ns 
>>> <targetDomain.com> 
>>> server <TargetDomainNameServer.com> 
>>> set type=any 
>>> ls -d <targetDomain.com>
```