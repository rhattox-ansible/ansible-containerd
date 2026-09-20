# ansible-containerd

Installs and configures containerd, runc, and CNI plugins on Linux hosts with systemd support.

## Overview

This Ansible role downloads the official containerd, runc, and CNI binaries, installs them to the expected system paths, configures the containerd service, and sets up group permissions for runtime access.

## Features

- Downloads official containerd release artifacts
- Installs the runc runtime binary
- Installs CNI plugin binaries
- Installs the containerd systemd unit file
- Enables and starts the containerd service
- Creates a dedicated containerd group and adjusts permissions
- Cleans up temporary installation files afterward

## Requirements

- Ansible 2.14 or newer
- Linux target system with systemd
- Root or sudo privileges
- Internet access to download release binaries

## Supported Platforms

- Debian: bookworm, bullseye
- Ubuntu: jammy, noble
- EL: 8, 9

## Role Variables

```yaml
# Temporary working directory
temporary_folder: "/tmp/ansible-containerd"

# Installation paths
install_path: "/usr/local/bin"
containerd_systemctl_folder: "/usr/local/lib/systemd/system/"
runc_path: "/usr/local/sbin/runc"
cni_plugin_folder: "/opt/cni/bin"

# containerd
containerd_version: "1.7.28"
containerd_url: "https://github.com/containerd/containerd/releases/download/v{{ containerd_version }}/containerd-{{ containerd_version }}-linux-amd64.tar.gz"
containerd_systemctl_url: "https://raw.githubusercontent.com/containerd/containerd/main/containerd.service"

# runc
runc_version: "1.3.0"
runc_url: "https://github.com/opencontainers/runc/releases/download/v{{ runc_version }}/runc.amd64"

# CNI plugins
cni_plugin_version: "1.7.1"
cni_plugin_url: "https://github.com/containernetworking/plugins/releases/download/v{{ cni_plugin_version }}/cni-plugins-linux-amd64-v{{ cni_plugin_version }}.tgz"
```

## Installation Flow

The role performs the following steps:

1. Creates working directories for temporary files and installation paths.
2. Downloads containerd, runc, CNI, and the containerd service unit.
3. Extracts the archives.
4. Copies binaries to the target locations.
5. Creates the `containerd` system group and configures permissions.
6. Reloads systemd, enables the service, and starts it.
7. Removes temporary files.

## Example Playbook

```yaml
- hosts: servers
  become: true
  roles:
    - role: rhattox-ansible.ansible-containerd
```

If you are using the repo locally, you can also run the included playbook:

```bash
ansible-playbook ./main.yaml -K
```

## Security Notes

- Creates a dedicated `containerd` system group
- Sets restrictive permissions on runtime directories and the containerd socket
- Uses official upstream release artifacts
- Keeps temporary files separate from system-managed directories

## License

MIT

## Author

rhattox-ansible