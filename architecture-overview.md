# Cisco UCS & EMC VNX Converged Infrastructure (VSPEX Architecture)

![Datacenter Rack Layout](diagrams/datacenter-rack-layout-27u.png)

## Executive Summary
This repository houses the complete **Governance-as-Code & Architecture-as-Code** framework for an Enterprise-grade Converged Infrastructure based on the **EMC VSPEX Option 5 Architecture**.

Designed for high-availability enterprise environments and regional branch operations, this blueprint integrates compute, storage, networking, and virtualization into a standardized, resilient, and fully redundant 10GE iSCSI architecture.

---

## Technical Specifications & Architecture Overview

| Layer | Technology / Hardware Component | Configuration & Role |
| :--- | :--- | :--- |
| **Compute** | Cisco UCS Mini B-Series / C-Series | High-density ESXi Hypervisor Hosts |
| **Fabric Interconnect** | 2x Cisco UCS 6248UP / Fabric Interconnects | Dual-Active HA Clustering, 10GE Unified Fabric |
| **Shared Storage** | EMC VNX 5200 Unified Storage | Dual Storage Processors (SPA/SPB), High-Performance iSCSI SAN |
| **Management Switch** | 2x Cisco Catalyst 2960X Switches | Stacked Out-of-Band (OOB) Management Network |
| **Virtualization Platform** | VMware vSphere ESXi / Microsoft Hyper-V | Enterprise Branch Virtualization Standards |
| **Backup & DR** | Veeam Backup & Replication | Automated Offsite Disaster Recovery Replication |

---

## Architectural Blueprint & Conceptual Diagrams

### 1. High-Availability Hardware Topology Design
The physical topology ensures complete redundancy across compute, fabric, and storage networking. Every component is connected via dual-pathing to eliminate single points of failure (SPOF).

![Option 5 Topology Design](diagrams/vspex-option5-topology-design.png)

### 2. Conceptual Infrastructure & Disaster Recovery
Conceptual overview showing unified management, iSCSI data paths, local backup repositories, and offsite disaster recovery synchronization.

![Option 5 Conceptual Architecture](diagrams/principal-drawing-option5.png)

---

## Repository Structure & Asset Index

```text
cisco-emc-vspex-datacenter-architecture/
├── README.md
├── docs/
│   ├── enterprise-branch-virtualization-standards.md
│   ├── ucs-fabric-interconnect-setup-guide.md
│   └── esxi-sd-card-installation-guide.pdf
├── diagrams/
│   ├── vspex-option5-topology-design.png
│   ├── principal-drawing-option5.png
│   └── datacenter-rack-layout-27u.png
└── templates/
    └── datacenter-cabling-and-power-matrix.xlsx