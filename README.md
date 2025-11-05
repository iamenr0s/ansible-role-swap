# Ansible Role: Swap
Manage the system swap runtime state across Linux hosts. This role toggles swap on or off
using the system swapon/swapoff mechanisms and can optionally tune `vm.swappiness`.
It is designed for simple runtime control (not for creating swapfiles) and works across
common Linux distributions.

## Features

- Enable or disable swap cleanly and idempotently (runtime)
- Leverages `swapon -a` and `swapoff -a` based on desired state
- Optional tuning of `vm.swappiness` via `sysctl`
- Safe defaults; suitable for Debian, Ubuntu, RHEL/Alma/Rocky, Fedora

## Requirements

- Ansible 2.9 or higher
- Collections: `ansible.posix`

## Supported Platforms

- Ubuntu 20.04 (focal), 22.04 (jammy), 24.04 (kinetic)
- Debian 11 (bullseye), 12 (bookworm)
- EL/RHEL 8, 9, 10
- Rocky Linux 8, 9, 10
- Fedora 39+

## Role Variables

### Basic Configuration

- `swap_enabled` (bool): Desired runtime state; `true` runs `swapon -a`, `false` runs `swapoff -a`. Default: `true`.
- `swap_swappiness` (int|null): Set `vm.swappiness` when enabled; Default: `60`.

## Dependencies

- `community.general` (for `swapfile`)
- `ansible.posix` (for `sysctl`)

## Example Playbook

### Enable swap (runtime)

```yaml
- hosts: all
  become: true
  roles:
    - role: iamenros.ansible_role_swap
      vars:
        swap_enabled: true
        swap_swappiness: 40
```

### Disable swap (runtime)

```yaml
- hosts: all
  become: true
  roles:
    - role: iamenros.ansible_role_swap
      vars:
        swap_enabled: false
```

## Testing

- Molecule scenario uses a privileged container but keeps `swap_enabled: false` by default because enabling swap is typically not allowed in containers. Adjust for real hosts as needed.

## License

This project is licensed under the [MIT License](LICENSE).

## Author Information

Author: iamenr0s
Galaxy: `iamenr0s.ansible_role_swap`

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Changelog

See `CHANGELOG.md` for version history and release notes.

