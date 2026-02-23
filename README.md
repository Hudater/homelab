# Overview

This repository contains the architectural documentation, system design decisions, and operational runbooks for my homelab infrastructure.

It will document the architecture, rebuild process, and operational philosophy behind a fully automated, reproducible private infrastructure stack.

> Reproducible, Infrastructure-as-Code driven private cloud built on Proxmox and Oracle Cloud

Provisioning and configuration live in separate repositories.
This repository is the authoritative documentation and system reference.

## Related Repositories

| Repository                                                  | Responsibility                    |
| ----------------------------------------------------------- | --------------------------------- |
| [Infra Lab (OCI WIP)](https://github.com/Hudater/infra_lab) | Infrastructure Provisioning       |
| [Ansible Lab (WIP)](https://github.com/Hudater/ansible_lab) | Configuration Management          |
| [Services](https://github.com/Hudater/services)             | Docker Compose Stacks and Scripts |
| [Archive Docs](https://github.com/Hudater/archive-docs)     | Currently Archived Docs for lab   |

Separation ensures clean lifecycle boundaries between provisioning and configuration.

## System Overview

[![My Homelab](https://res.cloudinary.com/djsasyvfl/image/upload/v1767633860/Hudater_Homelab_v1.0_a2qotp.svg)](https://blog.hudater.dev/posts/homelab/)

The infra is built with strict separation of concerns:

| Layer         | Tech                              |
| ------------- | --------------------------------- |
| Hypervisor    | Proxmox                           |
| Cloud         | Oracle Cloud                      |
| Provisioning  | OpenTofu                          |
| Configuration | Ansible                           |
| Core Services | Firewall, DNS, overlay networking |

Repository Purpose:

- Architecture documentation
- Network topology explanations
- DNS flow documentation
- Disaster recovery procedures
- Design rationale
- Rebuild instructions
- System diagrams
