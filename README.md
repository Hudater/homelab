# Homelab

[![Resume](https://img.shields.io/website?label=Resume&color=4051B5&style=for-the-badge&url=https%3A%2F%2Fresume.hudater.dev%2F)](https://resume.hudater.dev/)
[![Portfolio](https://img.shields.io/website?label=portfolio&color=4051B5&style=for-the-badge&url=https%3A%2F%2Fhudater.dev%2F)](https://hudater.dev/)
[![Infrastructure Status](https://img.shields.io/website?label=Infrastructure%20Status&color=4051B5&style=for-the-badge&url=https%3A%2F%2Fstatus.hudater.dev%2F)](https://status.hudater.dev/)
[![Links](https://img.shields.io/website?label=Links&color=4051B5&style=for-the-badge&url=https%3A%2F%2Flinks.hudater.dev%2F)](https://links.hudater.dev/)

> Everything-as-Code private cloud. 2 OCI regions + on-premises Proxmox. Zero-trust access, identity-driven authorization.
>
> k3s, OpenTofu, NetBird, Authentik, Traefik, OCI, Proxmox, Grafana, Loki, Prometheus
[![Blog](https://img.shields.io/badge/Read%20My-Blog-4051B5?style=for-the-badge&color=c63e3e)](https://blog.hudater.dev/categories/homelab/)

![Homelab Architecture](https://res.cloudinary.com/djsasyvfl/image/upload/v1767633860/Hudater_Homelab_v1.0_a2qotp.svg)

---

## Architecture

Three regions connected over a NetBird WireGuard mesh. No self-hosted service is publicly reachable. All access goes through Traefik with Authentik forward authentication.

| Region            | Role                                                |
| ----------------- | --------------------------------------------------- |
| OCI Mumbai        | Primary cloud region                                |
| OCI Zurich        | Secondary cloud region, DR and future CDN candidate |
| On-premises Noida | Primary compute, Proxmox hypervisor and k3s cluster |

The on-premises site runs two environments: **prod** (stable) and **staging** (testing), separated by VLANs. OPNsense handles firewall and inter-VLAN routing.

[Full architecture writeup](https://blog.hudater.dev/categories/homelab/)

---

## Design Principles

- **Everything-as-Code**: Infrastructure, DNS, identity, cluster config, and service definitions are managed via OpenTofu. No manual provisioning.
- **Zero-trust access**: NetBird overlay + Traefik + Authentik Proxy Outpost. Nothing is reachable without passing auth.
- **Identity-first**: Users sign in via Google OAuth through Authentik. A custom enrollment flow assigns group membership at sign-in time. JWT sync propagates groups to all services automatically. Humans and servers are managed as separate groups.
- **Incremental k8s migration**: Core services run on Docker Compose today. Stateless workloads will move to the on-prem k3s cluster under ArgoCD.

---

## Tech Stack

| Layer                      | Tech                                                     |
| -------------------------- | -------------------------------------------------------- |
| Hypervisor                 | Proxmox VE                                               |
| Firewall / Routing         | OPNsense                                                 |
| Cloud                      | Oracle Cloud Infrastructure (Mumbai + Zurich)            |
| Container Orchestration    | k3s, 3 control plane + 3 worker nodes, on-prem           |
| Provisioning               | OpenTofu + Terragrunt, remote state on HCP               |
| Overlay Networking         | NetBird, WireGuard-based mesh                            |
| Reverse Proxy              | Traefik                                                  |
| Identity                   | Authentik, Google OAuth + Proxy Outpost for forward auth |
| DNS                        | Technitium, clustered                                    |
| Monitoring & Observability | Loki, Grafana, Prometheus, Alloy                         |
| Services                   | Docker Compose, k3s migration ongoing                    |
| Domain Management          | Cloudflare, IaC managed                                  |

---

## IaC Coverage

| Component                                      | Tooling                 | State | CI                   |
| ---------------------------------------------- | ----------------------- | ----- | -------------------- |
| OCI Mumbai                                     | OpenTofu + Terragrunt   | HCP   | GitHub Actions       |
| OCI Zurich                                     | OpenTofu + Terragrunt   | HCP   | GitHub Actions       |
| Cloudflare DNS                                 | OpenTofu                | HCP   | GitHub Actions       |
| Resume pipeline                                | Cloudflare Workers + KV | HCP   | GitHub Actions       |
| k3s cluster                                    | OpenTofu + Ansible      | HCP   | None, ArgoCD planned |
| Core services (Authentik, NetBird, Technitium) | OpenTofu                | HCP   | None                 |
| Docker Compose stacks                          | Git-tracked             | —     | None                 |

---

## Services

**Zero-trust stack**: NetBird + Traefik + Authentik. All services sit behind this.

**Self-hosted AI**: Ollama + Open WebUI, running on AMD RX 7600 GPU. Docker MCP Gateway integration planned.

**Utilities**: IT-Tools, Stirling PDF, Portainer & much more

**Monitoring & Observability**: Loki, Grafana, Prometheus, Alloy. Setup in progress.

**WIP**: Forgejo at [git.hudater.dev](https://git.hudater.dev)

---

## Repositories

Follows separation of concerns: `infrastructure-iac` for infrastructure, `infrastructure-services` for services.

| Repository                                                                    | Status       | Responsibility                                                                         |
| ----------------------------------------------------------------------------- | ------------ | -------------------------------------------------------------------------------------- |
| [infrastructure-iac](https://github.com/Hudater/infrastructure-iac)           | Active       | OpenTofu IaC for OCI regions, k3s cluster, and core services                           |
| [infrastructure-services](https://github.com/Hudater/infrastructure-services) | Active       | Docker Compose stacks and service manifests                                            |
| [infrastructure-ansible](https://github.com/Hudater/ansible_lab)              | WIP, private | Configuration management, post-IaC stabilization                                       |
| [resume-pipeline](https://github.com/Hudater/resume-pipeline)                 | Active       | Cloudflare Workers + KV pipeline for resume.hudater.dev                                |
| [archive-docs](https://github.com/Hudater/archive-docs)                       | Archived     | Historical docs, live at [archive-docs.hudater.dev](https://archive-docs.hudater.dev/) |

---

## Contact

[harshit@hudater.dev](mailto:harshit@hudater.dev)
