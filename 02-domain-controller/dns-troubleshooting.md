# DNS Troubleshooting — DC01

## Problem

After promoting DC01 to a Domain Controller, DNS queries initially timed out.

Example:

"```text"
nslookup dc01 192.168.10.10

DNS request timed out.
Server: UnKnown
Address: 192.168.10.10
Investigation

The DNS Server service was verified:

Get-Service DNS

Result:

Status   Name
------   ----
Running  DNS

DNS was confirmed to be listening on port 53:

Get-NetUDPEndpoint -LocalPort 53

The DNS firewall rules were also verified:

DNS (UDP, Incoming)    Enabled    Any    Inbound    Allow
DNS (TCP, Incoming)    Enabled    Any    Inbound    Allow
Root Cause

Active Directory DNS records for DC01 had not been completely registered.

dcdiag /test:dns initially reported:

No host records (A or AAAA) were found for this DC
Record registrations cannot be found for all the network adapters
Resolution

DNS registration was manually initiated:

ipconfig /flushdns
ipconfig /registerdns
Restart-Service Netlogon

After registration, the DC01 A record was available:

DC01 → 192.168.10.10

The LDAP SRV record was also verified:

_ldap._tcp.dc._msdcs.corp.home.arpa

Target:
DC01.corp.home.arpa

Port:
389
Final Verification
dcdiag /test:dns

Result:

DC01 passed test DNS
corp.home.arpa passed test DNS

The Domain Controller advertising test also passed:

dcdiag /test:advertising

Result:

DC01 passed test Advertising
Final Status

DNS and Active Directory Domain Controller functionality are operational.

 DNS Server running
 DNS port 53 listening
 DNS firewall rules verified
 DC01 A record registered
 AD LDAP SRV record registered
 DNS test passed
 Domain Controller advertising test passed
