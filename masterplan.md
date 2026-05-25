# NetBird + Keycloak Setup

## Overview

This setup is a hybrid homelab + cloud infrastructure focused on:

- Zero-trust networking
- Private internal service exposure
- Centralized authentication
- Infrastructure automation
- Reproducible deployments
- VLAN-based segmentation

Core stack includes:

- Proxmox
- OPNsense
- NetBird
- Keycloak
- Pi-hole + Unbound
- OCI
- Terraform/OpenTofu
- Ansible
- Traefik

---

# Infrastructure Layout

## On-Prem

Primary production Proxmox host:

- CPU: Ryzen 5 5600X
- RAM: 48 GB DDR4-3200
- GPU: AMD RX 7600

Runs:

- OPNsense VM
- Debian VMs
- Pi-hole LXC
- Internal platform services
- Future PXE infra

---

## Cloud

Oracle Cloud Infrastructure (OCI) is used for:

- Publicly reachable services
- Remote DNS
- Overlay networking support
- Potential relay/control plane components

---

# Proxmox Setup

Proxmox acts as the main virtualization layer.

Current/planned responsibilities:

- VM orchestration
- Network segmentation
- Self-hosted infra services
- Template-based VM provisioning
- Automated infra deployment

Automation goals:

- Terraform/OpenTofu for infra lifecycle
- Ansible for configuration
- Automatic ISO downloads
- VM creation pipelines
- PXE-based unattended provisioning

---

# Networking

## OPNsense

OPNsense runs as a VM inside Proxmox.

Responsibilities:

- VLAN segmentation
- Routing
- Firewalling
- Gateway services
- Internal isolation
- Traffic policies

This is structured more like enterprise infra instead of flat consumer networking.

---

# DNS Stack

## Pi-hole + Unbound

Current DNS architecture:

- Pi-hole handles filtering/ad blocking
- Unbound acts as recursive resolver
- `pihole-updatelists` updates lists weekly

Deployment:

- Existing Pi-hole inside Proxmox LXC
- Planned OCI Pi-hole deployment via Docker
- Nebula-sync for synchronization

OCI Pi-hole is intended to become the primary DNS resolver for Headscale/Tailscale environments.

The DNS stack already handles large query volume (~500k queries).

---

# NetBird Setup

## Purpose

NetBird is used/planned for:

- Secure mesh networking
- Overlay connectivity
- Private service exposure
- Identity-aware access
- Cross-network connectivity

Goal is to avoid exposing infra publicly whenever possible.

---

## NetBird Components

Likely setup includes:

- NetBird management service
- Signal service
- Relay service
- STUN/TURN for NAT traversal
- NetBird agents on infra nodes

This is intended as a self-hosted deployment.

---

## NetBird Usage

NetBird connects:

- Proxmox nodes
- OCI instances
- Personal devices
- Dev machines
- Internal services
- Potential Kubernetes workloads

Benefits:

- Private SSH access
- Internal dashboards over mesh
- No public management ports
- Secure service-to-service communication
- Simplified remote access

---

# Keycloak Setup

## Purpose

Keycloak acts as centralized identity provider (IdP).

Responsibilities:

- Single Sign-On (SSO)
- OIDC authentication
- User management
- Identity federation
- Central auth policies

---

## Keycloak Integration Goals

Keycloak authenticates:

- NetBird users
- Internal dashboards
- Self-hosted applications
- Future Kubernetes ingress auth
- Internal platform tooling

The goal is unified authentication instead of app-local credentials.

---

## Authentication Flow

Typical flow:

1. User accesses service
2. Service redirects to Keycloak
3. User authenticates
4. Keycloak issues OIDC token
5. Service validates token
6. Access granted based on roles/groups

---

# Traefik

Traefik is used/planned for:

- Reverse proxying
- TLS termination
- Internal routing
- Service exposure
- Potential OIDC middleware integration

Potential usage:

- Internal-only apps over NetBird
- Auth-protected dashboards
- Automated routing
- Cloudflare DNS integration

---

# Infrastructure Automation

## Terraform/OpenTofu

Used/planned for:

- OCI provisioning
- DNS automation
- VM provisioning
- Network resources
- Reproducible infra

Cloudflare DNS automation is already part of infra workflows.

---

## Ansible

Used/planned for:

- Server configuration
- Package installation
- Service deployment
- Bootstrap automation
- Consistent provisioning

---

# Security Model

Core design philosophy:

- Zero-trust style networking
- Minimal public exposure
- Identity-based access
- Segmented networks
- Overlay-first connectivity
- Centralized authentication

Instead of:

```text
Internet -> Public Dashboard
```

The model is:

```text
Authenticated User -> NetBird Overlay -> Private Internal Service
```

---

# Additional Components

## Headscale / Tailscale

Tailscale is already configured and operational.

Used for:

- Additional mesh connectivity
- Device access
- DNS integration

OCI Pi-hole is intended to become the sole DNS resolver in Headscale.

---

# Monitoring / Observability

Current ecosystem includes:

- Uptime Kuma public status page
- Prometheus
- Grafana
- Loki
- Tempo
- Dynatrace
- Dash0

Monitoring focuses on:

- Infra uptime
- DNS health
- Service reliability
- Incident visibility

---

# Incident Management

Planned operational workflow:

- Maintain homelab-incidents Git repo
- Add RCA commits after outages/incidents
- Track operational failures historically

Example incidents:

- DNS outages
- Routing failures
- Authentication issues
- Service downtime

---

# Long-Term Direction

The stack is evolving toward:

- Fully reproducible infra
- Identity-aware internal platform
- Automated deployments
- Secure private networking
- Self-hosted developer platform architecture
- Kubernetes-ready infrastructure

