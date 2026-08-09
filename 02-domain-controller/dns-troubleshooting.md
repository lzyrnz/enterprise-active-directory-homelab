# DNS Troubleshooting — DC01

## Overview

After promoting `DC01` to an Active Directory Domain Controller, DNS queries were initially timing out and Active Directory DNS records were not being properly registered.

This document records the troubleshooting process, root cause, resolution, and final verification.

---

## Environment

| Component | Configuration |
|---|---|
| Server | DC01 |
| Operating System | Windows Server 2022 |
| Active Directory Domain | corp.home.arpa |
| NetBIOS Domain | CORP |
| IPv4 Address | 192.168.10.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | None |
| DNS Server | 192.168.10.10 |
| Network | VirtualBox Internal Network |
| Network Name | AD-LAB |

The lab is intentionally isolated from the Internet at this stage.

---

# 1. Problem

After the Active Directory Domain Services installation and domain controller promotion, DNS queries to DC01 initially failed.

The following command produced DNS timeouts:

powershell
nslookup dc01 192.168.10.10

Initial result:

DNS request timed out.
timeout was 2 seconds.

Server:  UnKnown
Address:  192.168.10.10

Active Directory diagnostics also reported DNS-related failures.

Initial dcdiag /test:dns results included:

No host records (A or AAAA) were found for this DC

Record registrations cannot be found for all the network adapters

The Domain Controller also initially failed the connectivity test.

2. Initial Investigation
2.1 Verify DNS Server Service

The DNS Server service was checked using:

Get-Service DNS

Result:

Status   Name   Display Name
------   ----   -----------
Running  DNS    DNS Server
Result

The DNS Server service was running correctly.

Status: PASS

2.2 Verify DNS Port 53

DNS listeners were checked using:

Get-NetUDPEndpoint -LocalPort 53

and:

Get-NetTCPConnection -LocalPort 53

DNS was found listening on port 53 on the DC01 interfaces, including:

192.168.10.10
127.0.0.1
::1
Result

The DNS Server was actively listening for DNS requests.

Status: PASS

3. Firewall Investigation

The DNS firewall rules were checked.

UDP
Get-NetFirewallRule -DisplayName "DNS (UDP, Incoming)" |
Format-Table DisplayName, Enabled, Profile, Direction, Action

Result:

DisplayName          Enabled  Profile  Direction  Action
-----------          -------  -------  ---------  ------
DNS (UDP, Incoming)  True     Any      Inbound    Allow
TCP
Get-NetFirewallRule -DisplayName "DNS (TCP, Incoming)" |
Format-Table DisplayName, Enabled, Profile, Direction, Action

Result:

DisplayName          Enabled  Profile  Direction  Action
-----------          -------  -------  ---------  ------
DNS (TCP, Incoming)  True     Any      Inbound    Allow
Result

Windows Firewall was not blocking the DNS Server rules.

Status: PASS

4. DNS Zone Investigation

The configured DNS zones were checked using:

Get-DnsServerZone

The following Active Directory-integrated zones were present:

_msdcs.corp.home.arpa
corp.home.arpa

Both zones were configured as:

ZoneType        : Primary
IsDsIntegrated  : True
Result

The required Active Directory DNS zones existed.

Status: PASS

5. Root Cause Identified

The DNS service itself was operational, but DC01's Active Directory DNS records had not initially been registered correctly.

The following command:

Get-DnsServerResourceRecord `
    -ZoneName "corp.home.arpa" `
    -Name "dc01"

initially returned an error indicating that the DC01 record could not be found.

dcdiag /test:dns confirmed the problem:

No host records (A or AAAA) were found for this DC

Record registrations cannot be found for all the network adapters

This prevented Active Directory from properly resolving and locating the domain controller.

6. DNS Registration Recovery

DNS registration was manually triggered.

First, the local DNS cache was cleared:

ipconfig /flushdns

Then DNS registration was initiated:

ipconfig /registerdns

The Netlogon service was restarted:

Restart-Service Netlogon

Netlogon is responsible for registering important Active Directory domain controller locator records in DNS.

7. DNS Record Verification

After registration, the DC01 A record became available.

The following command was used:

Get-DnsServerResourceRecord `
    -ZoneName "corp.home.arpa" `
    -RRType A

The resulting records included:

DC01    A    192.168.10.10

DNS resolution was then tested:

nslookup dc01.corp.home.arpa 192.168.10.10

The final result successfully resolved:

Name:
dc01.corp.home.arpa

Address:
192.168.10.10

Although nslookup displayed intermittent timeout messages during the troubleshooting process, the DNS server ultimately returned the correct A record.

8. DNS Client Configuration

The DNS client configuration was verified:

Get-DnsClientServerAddress -AddressFamily IPv4

Final configuration:

Interface: Ethernet
IPv4 DNS Server: 192.168.10.10

This means DC01 uses itself as its DNS server.

Final configuration:

DC01
 │
 └── DNS → 192.168.10.10
9. Active Directory SRV Record Verification

Active Directory relies on DNS SRV records to locate domain controllers and services.

The LDAP SRV record was tested using:

Resolve-DnsName `
"_ldap._tcp.dc._msdcs.corp.home.arpa" `
-Type SRV `
-Server 192.168.10.10

The result successfully returned:

Target:
DC01.corp.home.arpa

Port:
389

Priority:
0

Weight:
100

This confirmed that Active Directory's LDAP service discovery record was functioning.

10. DNS Diagnostic Verification

The complete DNS diagnostic was run:

dcdiag /test:dns

Final result:

DC01 passed test Connectivity

DC01 passed test DNS

corp.home.arpa passed test DNS

The DNS record registration section also passed:

PASS

The earlier DNS registration failure was therefore resolved.

11. Domain Controller Advertising Verification

The Domain Controller advertising test was performed:

dcdiag /test:advertising

Final result:

DC01 passed test Advertising

This confirms that DC01 is successfully advertising itself as an Active Directory Domain Controller.

12. Final Verification

The following tests were successfully completed:

Test	Result
DNS Server service	PASS
DNS port 53	PASS
DNS firewall rules	PASS
corp.home.arpa zone	PASS
_msdcs.corp.home.arpa zone	PASS
DC01 A record	PASS
LDAP SRV record	PASS
DNS record registration	PASS
dcdiag /test:dns	PASS
dcdiag /test:advertising	PASS
DC01 DNS client configuration	PASS
13. Final DNS Architecture

The completed configuration is:

                    AD-LAB
              192.168.10.0/24
                     │
                     │
              ┌──────▼──────┐
              │    DC01     │
              │             │
              │ 192.168.10.10│
              │             │
              │ AD DS       │
              │ DNS         │
              │ Global Cat. │
              └──────┬──────┘
                     │
          ┌──────────┴──────────┐
          │                     │
          ▼                     ▼
   corp.home.arpa       _msdcs.corp.home.arpa
14. Lessons Learned

This troubleshooting process demonstrated several important Active Directory concepts:

DNS is critical to Active Directory

Active Directory depends heavily on DNS for locating domain controllers and services.

A running DNS service does not guarantee working AD DNS

The DNS Server service was running and listening on port 53, but the domain controller's required DNS records were initially missing.

Netlogon registers important AD records

Restarting Netlogon helped trigger registration of the domain controller locator records.

Domain Controllers should use internal AD DNS

DC01 uses:

192.168.10.10

as its DNS server rather than an external DNS service.

External DNS servers such as public DNS resolvers should not replace the internal Active Directory DNS infrastructure.

15. Final Status

DC01 Active Directory and DNS infrastructure is operational.

 Windows Server 2022 installed
 DC01 hostname configured
 Static IP configured
 Active Directory Domain Services installed
 New forest created
 corp.home.arpa domain created
 DNS Server installed
 Active Directory DNS zones created
 DC01 DNS registration completed
 DC01 A record verified
 LDAP SRV record verified
 DNS diagnostic passed
 Domain Controller advertising passed

Phase 1 — Domain Controller Infrastructure: COMPLETE



