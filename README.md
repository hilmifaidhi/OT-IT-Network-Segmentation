# High-Availability Enterprise Network & Industrial OT/IT Security Segmentation

## Project Overview
Proyek ini mengimplementasikan arsitektur jaringan hirarkis skala enterprise dan segmentasi keamanan berbasis **Purdue Model for Control Hierarchy**. Perancangan ini memisahkan zona **Information Technology (IT)** dan **Operational Technology (OT)** untuk mengamankan infrastruktur kritis (PLC/SCADA) dari potensi enterprise cyber threats dan malware.

## Network Topology & Architecture
- **Edge Router (Simulasi MikroTik / Cisco 2911):** Mengelola akses WAN/ISP, NAT Overload, dan Edge Firewall.
- **Core / Distribution Switch (Cisco L3 Switch 3560):** Mengelola Inter-VLAN Routing dan Enforcement Access Control List (ACL).
- **Access Switch (Cisco L2 Switch 2960):** Mengelompokkan port ke masing-masing VLAN.

## Segmentation & IP Addressing Plan (VLSM)
- **VLAN 10 (IT Office):** `192.168.10.0/24` (Gateway: `192.168.10.1`)
- **VLAN 20 (DMZ Server):** `192.168.20.0/28` (Gateway: `192.168.20.1`)
- **VLAN 30 (OT Supervisory / HMI):** `10.10.30.0/24` (Gateway: `10.10.30.1`)
- **VLAN 40 (OT Process Control / PLC):** `10.10.40.0/24` (Gateway: `10.10.40.1`)

## Security Implementation & Enforcement
1. **Extended Access Control List (ACL):**
   - Mengizinkan IT (VLAN 10) mengakses Server DMZ (VLAN 20).
   - Memblokir total akses dari IT (VLAN 10) ke zona steril PLC/Process Control (VLAN 40).
2. **NAT & Edge Firewall:**
   - Mentranslasikan IP VLAN 10 & 20 untuk akses internet.
   - Mengisolasi total VLAN 40 (PLC) agar tidak memiliki akses langsung ke internet.

## Verification & Testing Results
- **IT to DMZ Server (`192.168.20.2`):** `REPLY` (Akses layanan internal berjalan normal).
- **IT to PLC (`10.10.40.10`):** `DESTINATION HOST UNREACHABLE` (ACL berhasil memblokir trafik IT ke PLC).
- **IT to Internet (`8.8.8.8`):** `REPLY` (NAT Overload berhasil).
- **PLC to Internet (`8.8.8.8`):** `DESTINATION HOST UNREACHABLE` (Zona OT berhasil terisolasi total dari internet).
