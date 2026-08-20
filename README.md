# Development Machine Setup

Simple Ansible playbooks to set up development environments on Linux and macOS machines.

## Directory Structure

```
.
├── linux-setup/           # Local Linux machine setup
│   ├── inventory.yml     # Local machine inventory
│   └── playbook.yml      # Setup playbook
├── linux-remote-setup/   # Remote Linux machine setup
│   ├── inventory.yml    # Remote machines inventory
│   └── playbook.yml     # Setup playbook
└── macos-setup/         # Local macOS setup
    ├── inventory.yml    # Local machine inventory
    └── playbook.yml     # Setup playbook
```

## What This Does

Sets up a development environment with common tools and configurations. Choose the setup based on your needs:

### 1. Local Linux Machine Setup (`linux-setup/`)
- For setting up your local Linux development machine
- Supports Ubuntu 22.04, Debian 12, and Fedora 38
- Installs and configures common development tools

### 2. Remote Linux Machine Setup (`linux-remote-setup/`)
- For setting up remote Linux development servers
- Same distribution support as local setup
- Includes additional security configurations
- Configurable via `group_vars/all.yaml`

### 3. macOS Setup (`macos-setup/`)
- For setting up your local macOS development machine
- Uses Homebrew for package management
- Includes macOS-specific optimizations

## Prerequisites

### For Linux Setup (Local or Remote)
```bash
# On Ubuntu/Debian
sudo apt-get update
sudo apt-get install -y python3 python3-pip
pip3 install ansible

# On Fedora
sudo dnf install -y python3 python3-pip
pip3 install ansible
```

### For macOS Setup
```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Ansible
brew install ansible
```

## Quick Start Guide
You can check the getting started guide [here](docs/getting-started.md).

### 1. Setting Up Local Linux Machine
```bash
# Clone this repository
git clone <repository-url>
cd setup-machine

# Run the local setup
cd linux-setup
ansible-playbook -i inventory.yml playbook.yml
```

### 2. Setting Up Remote Linux Machine
1. Edit the inventory file:
```bash
cd linux-remote-setup
cp inventory.yml my-servers.yml  # Create your own inventory file

# Edit my-servers.yml with your server details:
# Example:
# all:
#   hosts:
#     dev-server:
#       ansible_host: 192.168.1.100
#       ansible_user: your-username
```

2. Run the setup:
```bash
ansible-playbook -i my-servers.yml playbook.yml
```

### 3. Setting Up macOS Machine
```bash
cd macos-setup
ansible-playbook -i inventory.yml playbook.yml
```

## What Gets Installed

### Common Tools (All Platforms)
- Git
- Zsh with Oh My Zsh
- Powerlevel10k theme
- Python 3 with virtualenv
- Node.js and npm
- Docker
- Common utilities (curl, wget, htop, tmux, vim)

### Additional Tools for macOS
- Homebrew
- Docker Desktop
- Android Studio
- SDKMAN
- macOS-specific settings (key repeat, Finder preferences)

### Additional Features for Remote Setup
- UFW/firewalld configuration
- SSH security hardening
- No root login
- Key-based authentication

## Customization

### Linux Remote Setup
Edit `linux-remote-setup/group_vars/all.yaml` to customize:
- Package lists
- Security settings
- Docker configuration
- Shell preferences

Example customization:
```yaml
# Disable Docker installation
docker_settings:
  install: false

# Change security settings
security_settings:
  ufw_enabled: true
  ssh_settings:
    permit_root_login: false
    password_authentication: false
```

### macOS Setup
Edit `macos-setup/playbook.yml` to:
- Modify package lists
- Change macOS settings
- Add/remove applications

## Troubleshooting

### Common Issues

1. **Homebrew Update Fails (macOS)**
   - This is usually due to network connectivity
   - The playbook will retry automatically
   - You can safely ignore update failures

2. **SSH Connection Issues (Remote Setup)**
   - Ensure your SSH key is added to the remote server
   - Verify the server is reachable
   - Check the username and host in inventory file

3. **Permission Issues**
   - For local setup: Run with sudo if required
   - For remote setup: Ensure your user has sudo privileges

## License

MIT License - see [LICENSE](./LICENSE) file