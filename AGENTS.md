# AGENTS.md

## Project Overview

Ansible playbooks for hybrid lab automation — Docker setup, FastAPI deployment, Prometheus + Grafana monitoring, and security hardening across Home Lab and Company Lab environments.

## Tech Stack

- Ansible — configuration management and automation
- Jinja2 — templates for Docker Compose, Nginx, Prometheus configs
- Docker + Docker Compose — application runtime
- Ubuntu — target OS
- GitHub Actions — CI

## Architecture

```
ansible.cfg               → Ansible configuration (inventory path, SSH settings)
inventory/
  hosts.example           → Inventory template (fill your IPs)
playbooks/
  setup-docker.yml        → Install Docker CE + Compose
  deploy-fastapi.yml      → Deploy FastAPI + Nginx via Docker Compose
  setup-monitoring.yml    → Deploy Prometheus + Grafana + Node Exporter
  harden-server.yml       → UFW + fail2ban + SSH hardening
  templates/              → Jinja2 templates
    docker-compose.yml.j2
    nginx.conf.j2
    monitoring-compose.yml.j2
    prometheus.yml.j2
```

## Conventions

- Playbook naming: `verb-noun.yml` (e.g., `setup-docker.yml`, `deploy-fastapi.yml`)
- Templates use `.j2` extension
- Inventory uses INI format with groups `[homelab]` and `[companylab]`
- SSH key-based auth only (no passwords)
- No credentials in code — inventory file is gitignored

## Commands

- Run playbook: `ansible-playbook playbooks/setup-docker.yml`
- Check mode: `ansible-playbook playbooks/setup-docker.yml --check`
- Specific host: `ansible-playbook playbooks/deploy-fastapi.yml -l homelab`

## Recommended Execution Order

1. `harden-server.yml` → Secure the server first
2. `setup-docker.yml` → Install Docker
3. `deploy-fastapi.yml` → Deploy application
4. `setup-monitoring.yml` → Add monitoring

## Security Rules

- No passwords or keys in playbooks
- Use `inventory/hosts` (gitignored) for real IPs and credentials
- Playbooks disable password auth and root SSH login
- Future: Ansible Vault for secrets

## Important Notes

- Target: Ubuntu servers (Home Lab + Company Lab)
- Ansible control node = your local machine
- Templates are parameterized via inventory variables
- Default branch: `main`
