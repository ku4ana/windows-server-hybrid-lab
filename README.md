# Hybrid Windows Server Infrastructure Lab

## Overview

This project documents a fully virtualized hybrid Windows Server lab environment built using VMware Workstation Pro.  

The lab simulates a small enterprise infrastructure including:

- Multi-forest Active Directory deployment
- Two-way forest trust
- DNS and DHCP configuration
- File server with NTFS and share permissions
- Group Policy implementation (including loopback processing)
- Domain Controller backup and restore
- Microsoft Entra Connect integration
- Password Hash Synchronization (PHS)
- Seamless Single Sign-On (SSO)
- pfSense virtual routing and network segmentation

The purpose of this lab is hands-on preparation for the Windows Server Hybrid Administrator Associate certification and to gain practical infrastructure administration experience.

---

## Architecture Overview

### Logical Design
                              Internet
                                 │
                          [ pfSense Firewall ]
                                 │
            ┌────────────────────┼────────────────────┐
            │                    │                    │
     192.168.100.0/24     192.168.101.0/24       OpenVPN Tunnel
        (Forest 1)            (Forest 2)          10.10.10.0/24
            │                    │                    │
     ┌──────┼────────┐           │                    │
     │      │        │           │                    │
    DC1    FS1     SYNC1        DC2              Remote Win11 VM
 (AD/DNS) (File) (NPS+Entra) (AD/DNS)           (Non-domain client)

### Server Layout
Forest A (lab.local)
- DC1 (Domain Controller, DHCP, DNS)
- FS1 (File Server)
- SYNC01 (Microsoft Entra Connect,NPS)
- Windows 11 Client (Domain Joined)
- Windows 11 Remote Client(Not-Domain joined, VPN connected)

Forest B (lab2.local)
- DC2 (Domain Controller, DHCP, DNS)

Network Routing and DNS filtering handled by pfSense VM

---

## Core Infrastructure Components

### Active Directory
- Deployed two separate forests
- Configured two-way forest trust
- Verified DNS resolution across forests
- Implemented reverse lookup zones

### DNS & DHCP
- Configured forward and reverse lookup zones
- Implemented DHCP scopes
- Configured gateway routing via pfSense
- Resolved cross-forest name resolution issues

### File Services
- Configured shared folders with:
  - NTFS permissions
  - Share permissions
- Implemented domain group-based access control
- Troubleshot authentication and trust-related access issues

### Group Policy
- Created OU-based GPO structure
- Configured:
  - User wallpaper policies
  - Computer policies
  - Loopback processing (Merge mode)
- Analyzed GPO precedence and inheritance
- Used tools:
  - `gpupdate /force`
  - `gpresult /r`

### Backup & Recovery
- Performed Domain Controller system state backup
- Restored DC from backup
- Tested recovery functionality

### Hybrid Identity (Microsoft Entra)
- Deployed Microsoft Entra Connect on dedicated member server
- Configured:
  - Password Hash Synchronization (PHS)
  - Seamless Single Sign-On
- Verified Azure synchronization
- Tested cloud authentication and MFA

---

## Networking

- Deployed pfSense as virtual router
- Configured:
  - Default gateways
  - Firewall rules
- Enabled internet access for isolated lab networks

---

## Troubleshooting Scenarios Resolved

- DHCP authorization issues
- DNS reverse lookup inconsistencies
- GPO wallpaper conflicts (user vs computer + loopback)
- Cross-forest authentication failures
- Entra Connect schema permission errors
- MFA registration and authentication flow issues
- IPv6 DNS interference with Entra synchronization

---

## Skills Demonstrated

- Deployed two-forest AD infrastructure with two-way forest trust
- Configured conditional forwarders for cross-forest name resolution
- Implemented Group Policy inheritance and loopback processing
- Resolved routing instability caused by multiple default gateways
- Deployed OpenVPN with certificate + RADIUS authentication
- Integrated NPS with AD for VPN authorization
- Configured DNSBL filtering for remote VPN clients
- Implemented Microsoft Entra Connect (PHS + Seamless SSO)
- Simulated external remote client access (non-domain joined VM

---

## Tools Used

- VMware Workstation Pro
- Windows Server 2025
- Windows 11
- pfSense
- Microsoft Entra ID
- Microsoft Entra Connect
- PowerShell (basic administrative commands)

---

## Project Goal

To build a realistic hybrid enterprise lab environment to strengthen hands-on skills in:

- Identity management
- Hybrid cloud integration
- Infrastructure design
- Windows Server administration
