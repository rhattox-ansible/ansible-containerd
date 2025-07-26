# ansible-containerd

An Ansible role to install and configure containerd on Linux systems.

## Features

- Installs containerd from official releases
- Installs runc runtime
- Sets up CNI plugins
- Configures systemd service
- Sets up proper permissions and group access

## Requirements

- Ansible 2.9 or higher
- Linux target system

## Role Variables

```yaml
# Installation paths
temporary_folder: "/tmp/ansible-containerd"
install_path: /usr/local/bin

# ContainerD version and URL
containerd_version: "1.7.28"
containerd_url: "https://github.com/containerd/containerd/releases/download/v{{ containerd_version }}/containerd-{{ containerd_version }}-linux-amd64.tar.gz"
containerd_systemctl_url: "https://raw.githubusercontent.com/containerd/containerd/main/containerd.service"

# RunC version and URL
runc_version: "1.3.0"
runc_url: "https://github.com/opencontainers/runc/releases/download/v{{ runc_version }}/runc.amd64"
runc_path: "/usr/local/sbin/runc"

# CNI Plugin version and URL
cni_plugin_version: "1.7.1"
cni_plugin_url: "https://github.com/containernetworking/plugins/releases/download/v{{ cni_plugin_version }}/cni-plugins-linux-amd64-v{{ cni_plugin_version }}.tgz"
```

## Installation Steps

1. Downloads required components:
   - containerd binaries
   - runc binary
   - CNI plugins
   - systemd service file

2. Extracts and installs binaries:
   - containerd to `/usr/local/bin`
   - runc to `/usr/local/sbin`
   - CNI plugins to proper location

3. Sets up systemd service:
   - Installs service file
   - Enables and starts the service

4. Configures permissions:
   - Creates containerd group
   - Sets proper ownership and permissions
   - Configures socket access

## Usage

Include this role in your playbook:

```yaml
- hosts: servers
  roles:
    - ansible-containerd
```

## Security

- Creates a dedicated containerd group
- Sets proper permissions on binaries and socket
- Configures secure directory permissions
- Manages group access for containerd socket

## License

MIT

## Author

rhattox-ansible

## Execution

ansible-playbook ./main.yaml -K