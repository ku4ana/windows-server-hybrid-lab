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
- Created server and client certificates for OpenVPN; configured pfBlockerNG to restrict domain access for VPN users.
- Configured NPS policies to allow AD group authentication for VPN (RADIUS client).
- Joined Cloud_Client to Entra ID and enrolled it in Intune; created Configuration Profiles, Compliance Policies, Conditional Access and Update policies.
- Deployed Office 365 and Win32 applications via Intune for testing app deployment workflows.

The purpose of this lab is hands-on preparation for the Windows Server Hybrid Administrator Associate certification and to gain practical infrastructure administration experience.

---

## Architecture Overview


### Server Layout

DC1 — Domain Controller for Test Domain 1; DNS and DHCP for the subnet; provides trust to DC2.

DC2 — Domain Controller for Test Domain 2 in a separate forest; DNS and DHCP for its subnet; provides trust to DC1.

FS1 — File server for Test Domain 1; stores shared data and system state backups for DC1.

WIN11 — Domain‑joined client used for GPO testing and production scenario simulation.

Remote_WIN11 — Non‑domain client using NAT; connects to Test Domain 1 resources via OpenVPN.

Router1 (pfSense) — Gateway with three interfaces (WAN, LAN1, LAN2); provides internet, OpenVPN access and domain filtering via pfBlockerNG.

SYNC1 — Entra Connect server; synchronizes on‑prem users and groups to the Entra tenant; provides PHS and Seamless SSO; hosts NPS (RADIUS) for VPN authentication.

Cloud_Client — Intune‑managed client joined to Entra ID; used to test Configuration Profiles, Compliance, Conditional Access and App Deployment.


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

- Segmented network with WAN, LAN1 and LAN2 interfaces on pfSense to isolate test domains and simulate multi‑subnet production environments.
- Each domain controller serves DNS and DHCP for its subnet; Conditional Forwarders and Reverse Lookup zones configured for cross‑domain name resolution.
- OpenVPN server on pfSense provides secure remote access; client profiles exported and tested on Remote_WIN11.
- NPS (RADIUS) configured to allow AD group authentication for VPN users, enabling centralized access control.
- pfSense enforces routing between subnets and internet access; pfBlockerNG used to restrict domain access for VPN users and simulate policy enforcement.

---

## Troubleshooting Scenarios Resolved

- DHCP authorization issues
- DNS reverse lookup inconsistencies
- GPO wallpaper conflicts (user vs computer + loopback)
- Cross-forest authentication failures
- Entra Connect schema permission errors
- MFA registration and authentication flow issues
- IPv6 DNS interference with Entra synchronization
- Assigning product licenses to users in Entra ID
- Resolving conflicts with Intune compliance and configuration policies
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
- Simulated external remote client access (non-domain joined VM)
- Creating users; Assigning permissions in Active Directory and roles in Entra ID
- Creating and assinging Conditional Access policies in Entra ID
- Creating and assigning compliance, configuration, update policies in Intune MDM

---

## Tools Used

- VMware Workstation Pro
- Windows Server 2025
- Windows 11 Pro
- pfSense (with OpenVPN and pfBlockerNG)
- Entra ID and Entra Connect
- Microsoft Intune MDM
- NPS (RADIUS)
- PowerShell (basic administrative commands)

---

## Project Goal
# Purpose

Provide a compact, reproducible hybrid lab that simulates a small production environment for hands‑on testing of identity, networking and device management scenarios.

# Objectives

- Validate AD and DNS interactions across trusted forests, test Group Policy deployment and client configuration, and demonstrate secure remote access workflows.
- Exercise hybrid identity flows by synchronizing on‑prem users to Entra ID and managing cloud‑joined devices with Intune.
