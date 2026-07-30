[![Molecule](https://github.com/iamenr0s/ansible-role-swap/actions/workflows/molecule.yml/badge.svg)](https://github.com/iamenr0s/ansible-role-swap/actions/workflows/molecule.yml) ![Ansible Role](https://img.shields.io/ansible/role/d/iamenr0s/ansible_role_swap) [![CodeFactor](https://www.codefactor.io/repository/github/iamenr0s/ansible-role-swap/badge)](https://www.codefactor.io/repository/github/iamenr0s/ansible-role-swap)

Ansible Role: Swap
===================

Manages the system swap runtime state across Linux hosts. This role toggles swap on or off using `swapon`/`swapoff` and can optionally tune `vm.swappiness`. It is designed for simple runtime control (not for creating swapfiles) and skips swap operations automatically inside containers.

Features
--------
- Enables or disables swap cleanly and idempotently (`swapon -a` / `swapoff -a`).
- Optional tuning of `vm.swappiness` via `ansible.posix.sysctl`.
- Detects container/virtualization environments and skips swap toggling there, since containers typically cannot enable swap.

Requirements
------------
- Ansible 2.9 or higher.
- Collection: `ansible.posix` (for the `sysctl` module).

Supported Platforms
--------------------
- AlmaLinux 8, 9, 10
- Debian 12, 13
- Fedora 42, 43, 44
- Rocky Linux 8, 9, 10
- Ubuntu 22.04, 24.04

Role Variables
---------------
Defined in `defaults/main.yml`:

- `swap_enabled` (bool): Desired runtime state; `true` runs `swapon -a`, `false` runs `swapoff -a` (default: `true`).
- `swap_swappiness` (int|null): Kernel swappiness value (0-100) to set via `vm.swappiness`; set to `null` to skip managing it entirely (default: `60`).

Example Playbook
-----------------
Enable swap and tune swappiness:

```yaml
- hosts: all
  become: true
  roles:
    - role: iamenr0s.ansible_role_swap
      vars:
        swap_enabled: true
        swap_swappiness: 40
```

Disable swap:

```yaml
- hosts: all
  become: true
  roles:
    - role: iamenr0s.ansible_role_swap
      vars:
        swap_enabled: false
```

Contributing & Security
-------------------------
- Contributions are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).
- Report vulnerabilities privately per [SECURITY.md](SECURITY.md); do not open public issues for them.

CI & Release (maintainers)
----------------------------
A single workflow (`.github/workflows/molecule.yml`) runs lint and the full Molecule distro matrix on pushes to `main`, PRs, and `v*` tags. On `v*` tags, a `release` job publishes to Ansible Galaxy after all tests pass.

The Galaxy API key lives in the `galaxy` GitHub environment, which only `v*` tags may target. One-time setup:

```bash
# Galaxy publishing key (environment-scoped, get it from galaxy.ansible.com/ui/token)
gh secret set GALAXY_API_KEY --env galaxy --repo iamenr0s/ansible-role-swap

# Code scanning notifications (Slack webhook URL; for Discord append /slack to the webhook URL)
gh secret set SECURITY_ALERT_WEBHOOK --env galaxy --repo iamenr0s/ansible-role-swap
```

`.github/workflows/code-scanning-notify.yml` polls the code-scanning API every 6 hours and posts new or updated open alerts to that webhook (GitHub Actions cannot trigger on `code_scanning_alert` directly).

To release: tag a commit `vX.Y.Z` and push the tag — CI gates the Galaxy publish.

License
-------
This project is licensed under the [MIT License](LICENSE).

Author Information
--------------------
Author: iamenr0s
Galaxy: `iamenr0s.ansible_role_swap`
