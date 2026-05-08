# Azure Production Network Setup Guide

## Overview

This document explains how to manually create a production-grade Azure networking setup using:

- Resource Group
- Virtual Network (VNET)
- Public Subnet
- Private Subnet
- Network Security Groups (NSG)
- Route Tables
- NAT Gateway
- Public and Private Virtual Machines

---

# Final Architecture

```text
Resource Group
│
└── VNet (10.0.0.0/16)
    │
    ├── Public Subnet  (10.0.1.0/24)
    │     ├── NSG-Public
    │     ├── RouteTable-Public
    │     └── Bastion / Load Balancer / Jump Server
    │
    └── Private Subnet (10.0.2.0/24)
          ├── NSG-Private
          ├── RouteTable-Private
          └── Backend APIs / DB / Redis / AKS
```

---

# STEP 1 — Create Resource Group

## Login

Open Azure Portal:

https://portal.azure.com

---

## Create Resource Group

Search:

```text
Resource Groups
```

Click:

```text
Create
```

Fill:

| Field | Value |
|---|---|
| Subscription | Your Subscription |
| Resource Group | prod-rg |
| Region | Central India |

Click:

```text
Review + Create
```

---

# STEP 2 — Create Virtual Network (VNET)

Search:

```text
Virtual Networks
```

Click:

```text
Create
```

---

## Basics Tab

| Field | Value |
|---|---|
| Resource Group | prod-rg |
| Name | prod-vnet |
| Region | Central India |

---

## IP Addresses Tab

Remove default subnet.

Set Address Space:

```text
10.0.0.0/16
```

---

# STEP 3 — Create Public Subnet

Inside VNET creation:

Click:

```text
+ Add Subnet
```

Fill:

| Field | Value |
|---|---|
| Subnet Name | public-subnet |
| Address Range | 10.0.1.0/24 |

Click:

```text
Add
```

---

# STEP 4 — Create Private Subnet

Again click:

```text
+ Add Subnet
```

Fill:

| Field | Value |
|---|---|
| Subnet Name | private-subnet |
| Address Range | 10.0.2.0/24 |

Click:

```text
Add
```

Then click:

```text
Review + Create
```

---

# STEP 5 — Create Public NSG

Search:

```text
Network Security Groups
```

Click:

```text
Create
```

Fill:

| Field | Value |
|---|---|
| Name | public-nsg |
| Resource Group | prod-rg |
| Region | Central India |

Click:

```text
Review + Create
```

---

# STEP 6 — Configure Public NSG Rules

Open:

```text
public-nsg
→ Inbound security rules
→ Add
```

---

## Allow SSH

| Field | Value |
|---|---|
| Source | Any |
| Source Port Ranges | * |
| Destination | Any |
| Service | SSH |
| Port | 22 |
| Protocol | TCP |
| Action | Allow |
| Priority | 100 |
| Name | allow-ssh |

---

## Allow HTTP

| Field | Value |
|---|---|
| Service | HTTP |
| Port | 80 |
| Name | allow-http |

---

## Allow HTTPS

| Field | Value |
|---|---|
| Service | HTTPS |
| Port | 443 |
| Name | allow-https |

---

# STEP 7 — Create Private NSG

Create another NSG.

| Field | Value |
|---|---|
| Name | private-nsg |
| Resource Group | prod-rg |
| Region | Central India |

---

# STEP 8 — Configure Private NSG Rules

Private subnet should not allow direct internet traffic.

Allow only internal communication.

## Example Rules

| Source | Port | Action |
|---|---|---|
| 10.0.1.0/24 | 22 | Allow |
| 10.0.1.0/24 | 3000 | Allow |
| VirtualNetwork | Any | Allow |

Do NOT allow:

```text
0.0.0.0/0
```

for private servers.

---

# STEP 9 — Associate NSGs to Subnets

Go to:

```text
Virtual Networks
→ prod-vnet
→ Subnets
```

---

## Attach Public NSG

Select:

```text
public-subnet
```

Attach:

```text
public-nsg
```

Save.

---

## Attach Private NSG

Select:

```text
private-subnet
```

Attach:

```text
private-nsg
```

Save.

---

# STEP 10 — Create Public Route Table

Search:

```text
Route Tables
```

Click:

```text
Create
```

Fill:

| Field | Value |
|---|---|
| Name | public-rt |
| Resource Group | prod-rg |
| Region | Central India |

---

# STEP 11 — Configure Public Route

Open:

```text
public-rt
→ Routes
→ Add
```

Add:

| Field | Value |
|---|---|
| Route Name | internet-route |
| Destination Type | IP Addresses |
| Destination CIDR | 0.0.0.0/0 |
| Next Hop Type | Internet |

This means:

```text
All traffic goes to the internet.
```

---

# STEP 12 — Associate Public Route Table

Go to:

```text
public-rt
→ Subnets
→ Associate
```

Select:

```text
public-subnet
```

Save.

---

# STEP 13 — Create Private Route Table

Create another Route Table.

| Field | Value |
|---|---|
| Name | private-rt |
| Resource Group | prod-rg |

---

# STEP 14 — Configure Private Route Table

## Option 1 — No Internet Access

Do not add:

```text
0.0.0.0/0
```

This keeps the subnet fully private.

---

## Option 2 — NAT Gateway (Recommended)

Private servers can access internet outbound only.

Examples:

```bash
apt update
pip install
Docker pull
```

Internet cannot directly access private servers.

---

# STEP 15 — Create NAT Gateway

Search:

```text
NAT Gateway
```

Click:

```text
Create
```

Fill:

| Field | Value |
|---|---|
| Name | prod-nat |
| Resource Group | prod-rg |
| Region | Central India |

---

## Attach Public IP

Create or select:

```text
Public IP Address
```

---

## Associate NAT Gateway to Private Subnet

Go to:

```text
Subnets
→ Associate
```

Select:

```text
private-subnet
```

Save.

---

# STEP 16 — Associate Private Route Table

Attach:

```text
private-rt
```

to:

```text
private-subnet
```

---

# STEP 17 — Create Bastion / Jump VM

Create VM.

## Basics

| Field | Value |
|---|---|
| VM Name | bastion-vm |
| Region | Central India |
| Image | Ubuntu 22.04 |
| Size | Standard_B2s |

---

## Networking

| Field | Value |
|---|---|
| Virtual Network | prod-vnet |
| Subnet | public-subnet |
| Public IP | Enabled |
| NSG | public-nsg |

This VM acts as:

```text
Jump Server / Bastion Host
```

---

# STEP 18 — Create Backend VM

Create another VM.

## Basics

| Field | Value |
|---|---|
| VM Name | backend-vm |
| Image | Ubuntu 22.04 |
| Size | Standard_B2s |

---

## Networking

| Field | Value |
|---|---|
| Virtual Network | prod-vnet |
| Subnet | private-subnet |
| Public IP | Disabled |
| NSG | private-nsg |

Important:

```text
Do NOT expose backend or DB directly to internet.
```

---

# Production Best Practices

## Public Subnet

Use for:

- Load Balancer
- Bastion Host
- Reverse Proxy
- Firewall
- NAT Gateway

---

## Private Subnet

Use for:

- Backend APIs
- Databases
- Redis
- Kubernetes Worker Nodes
- AI/ML Servers

---

# Security Best Practices

## Never Do This

❌ Open all ports:

```text
0.0.0.0/0
```

❌ Put databases in public subnet.

❌ Keep SSH open publicly forever.

❌ Expose backend directly to internet.

---

# Recommended Production Architecture

```text
Internet
   │
Application Gateway / Load Balancer
   │
Public Subnet
   │
Private Backend Subnet
   │
Database Subnet
```

---

# Example Architecture for Backend Deployment

```text
Users
  │
Azure Application Gateway
  │
Public Subnet
  │
Backend API Servers
(Private Subnet)
  │
PostgreSQL / Redis
(Private Subnet)
```

---

# CIDR Reference

| CIDR | Meaning |
|---|---|
| 10.0.0.0/16 | Large VNET |
| 10.0.1.0/24 | Public Subnet |
| 10.0.2.0/24 | Private Subnet |

---

# Azure Components Explanation

| Component | Purpose |
|---|---|
| VNET | Private Network |
| Subnet | Smaller network inside VNET |
| NSG | Firewall |
| Route Table | Traffic routing |
| NAT Gateway | Outbound internet access |
| Public IP | Internet access |
| Bastion | Secure SSH access |

---

# Recommended Next Steps

After networking setup:

1. Configure Azure Load Balancer
2. Configure Azure Application Gateway
3. Setup Azure Bastion
4. Setup CI/CD Pipeline
5. Configure HTTPS SSL
6. Configure Azure Monitor
7. Setup Backup & Disaster Recovery
8. Configure WAF Firewall
9. Setup Auto Scaling
10. Configure Kubernetes / AKS

---

# Useful Azure Documentation

Azure Portal:
https://portal.azure.com

Azure Virtual Network:
https://learn.microsoft.com/en-us/azure/virtual-network/

Azure NSG:
https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview

Azure Route Tables:
https://learn.microsoft.com/en-us/azure/virtual-network/manage-route-table

Azure NAT Gateway:
https://learn.microsoft.com/en-us/azure/nat-gateway/nat-overview

