# Automated Rocky Linux 8 Installation using Kickstart & PXE
A lab project demonstrating automated Rocky Linux 8 deployment using Kickstart, PXE boot, and network-based installation for training purposes.

## Overview
This repository documents a lab project that demonstrates how to automate the installation of **Rocky Linux 8** using **Kickstart** and **PXE boot**.  
The setup was created as part of a student training activity to help learners understand how enterprise Linux systems are deployed in real-world IT environments.

The environment allows client machines to boot over the network, load a PXE menu, and perform a fully unattended operating system installation using a predefined Kickstart configuration file.

---

## What this project demonstrates
This lab shows how the following technologies work together:

- Kickstart (unattended Linux installation)
- PXE Boot using `pxelinux`
- DHCP server for network boot clients
- TFTP server for boot files
- HTTP server for Kickstart file and installation repository
- Firewall configuration for PXE and installation services

---

## High-level Architecture

1. A Rocky Linux 8 server hosts:
   - DHCP
   - TFTP
   - HTTP (Kickstart file and OS repository)

2. A client machine boots using PXE.
3. The client receives an IP address from the DHCP server.
4. The PXE boot menu is loaded.
5. The installer fetches `ks.cfg` from the web server.
6. Rocky Linux is installed automatically without manual input.

---

## Configuration Files

| File | Purpose |
|------|--------|
| `dhcpd.conf` | Assigns IP addresses and directs PXE clients to the bootloader |
| `pxelinux.cfg/default` | Displays the PXE boot menu and kernel boot options |
| `ks.cfg` | Defines automated installation settings such as partitioning, packages, and users |

These files will be updated as the lab environment is rebuilt with a fresh Rocky Linux 8 setup.

---

## Firewall & Network Requirements

The Kickstart PXE server requires the following services to be allowed:

- DHCP (UDP 67/68)
- TFTP (UDP 69)
- HTTP (TCP 80)

These are necessary for PXE boot, file transfer, and access to the Kickstart and OS repository.

---

## How to use this repository

This repository provides reference configuration files and documentation for building a Kickstart PXE environment.

For full setup instructions, screenshots, and troubleshooting, refer to the PDF files in the `docs` folder.

---

## Learning Outcome

This project demonstrates practical experience with:

- Automated Linux provisioning
- PXE boot infrastructure
- Kickstart configuration
- System deployment in a training lab environment

It reflects how operating systems are deployed and standardized in enterprise IT environments.


