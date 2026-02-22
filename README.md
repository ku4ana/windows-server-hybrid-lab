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
                         |
                      pfSense
             -------------------------
             |           |           |
            LAN1        LAN2      NAT/WAN
             |           |
       Forest A       Forest B
      (lab.local)   (lab2.local)

### Server Layout
Forest A (lab.local)
- DC1 (Domain Controller, DHCP, DNS)
- FS1 (File Server)
- SYNC01 (Microsoft Entra Connect)
- Windows 11 Client (Domain Joined)

Forest B (lab2.local)
- DC2 (Domain Controller, DHCP, DNS)

Network Routing handled by pfSense VM

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

- Active Directory deployment and management
- Multi-forest trust configuration
- Group Policy design and troubleshooting
- Hybrid identity implementation
- DNS and network troubleshooting
- Role separation and infrastructure design
- Identity architecture understanding

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
