# Getting Started with Setup Machine

This guide will help you set up your development environment using Ansible, whether you're configuring a local machine or a remote server.

## Prerequisites

- Ansible 2.9 or later
- Python 3.8 or later
- SSH access (for remote setup)
- sudo privileges (for local setup)

For macOS, install Ansible first if it is not already available:

```bash
brew install ansible
```

## Installation

1. Install Ansible:
```bash
pip install ansible
```

2. Install Git (if not already installed):

   **macOS:**
   ```bash
   brew install git
   ```

   **Ubuntu/Debian:**
   ```bash
   sudo apt update
   sudo apt install git
   ```

   **Fedora/RHEL/CentOS:**
   ```bash
   sudo dnf install git
   ```

## Local Machine Setup

### macOS Setup

The macOS setup will configure your development environment with:
- Common development tools (git, zsh, curl, etc.)
- Homebrew packages
- Node.js and npm
- Docker Desktop
- Visual Studio Code
- iTerm2
- Oh My Zsh with Powerlevel10k theme
- SDKMAN for managing Java tooling
- Python packages
- macOS system preferences

To run the macOS setup:
```bash
cd macos-setup
ansible-playbook -i inventory.yml playbook.yml
```

### Linux Setup

The Linux setup will configure your development environment with:
- Common development tools (git, zsh, curl, etc.)
- Node.js and npm
- Docker
- Oh My Zsh with Powerlevel10k theme
- Python tools
- Basic security setup

To run the Linux setup:
```bash
cd linux-setup
ansible-playbook -i inventory.yml playbook.yml
```

## Remote Linux Setup

The remote Linux setup will configure a server with:
- Common development tools (git, zsh, curl, etc.)
- Node.js and npm
- Docker
- Oh My Zsh with Powerlevel10k theme
- Python tools
- Enhanced security for remote servers

To run the remote Linux setup:
```bash
cd linux-remote-setup
ansible-playbook -i inventory.yml playbook.yml
```

## Testing with Multipass VMs

[Multipass](https://multipass.run/) is a lightweight VM manager that makes it easy to test your Ansible scripts in an isolated environment before deploying to production servers.

### Installing Multipass

#### macOS
```bash
# Using Homebrew
brew install --cask multipass

# Or download the installer from https://multipass.run/download/macos
```

#### Linux
```bash
# Ubuntu
sudo snap install multipass

# Other distributions: Visit https://multipass.run/download/linux
```

#### Windows
```
# Download the installer from https://multipass.run/download/windows
```

### Creating and Managing VMs

1. Create a new VM:
```bash
# Create a VM with default resources (1GB RAM, 5GB disk)
multipass launch --name ansible-test

# Create a VM with custom resources
multipass launch --name ansible-test --cpus 2 --mem 2G --disk 10G
```

2. List your VMs:
```bash
multipass list
```

3. Get VM information (including IP address):
```bash
multipass info ansible-test
```

4. Shell into the VM:
```bash
multipass shell ansible-test
```

5. Stop or delete a VM:
```bash
# Stop a VM
multipass stop ansible-test

# Delete a VM
multipass delete ansible-test
multipass purge  # Permanently remove deleted VMs
```

## Multipass VM Setup

If you're using Multipass for virtualization, you can use the provided transfer-and-run.sh script to easily set up your VMs:

1. Create a Multipass VM (if you don't have one yet):
```bash
multipass launch --name ansible-test
```

2. Run the transfer-and-run.sh script from the project root directory:
```bash
chmod +x transfer-and-run.sh
./transfer-and-run.sh
```

This script will:
- Copy the Ansible playbook to your Multipass VM
- Install Ansible and Git inside the VM
- Create a local inventory file
- Run the playbook directly inside the VM

For custom VM names, edit the VM_NAME variable in the script:
```bash
# Open the script
vim transfer-and-run.sh

# Change this line to match your VM name
VM_NAME="your-vm-name"
```

## SSH Setup for Remote Machine

1. Generate SSH key (if you don't have one):
```bash
ssh-keygen -t rsa -b 4096
```

2. Copy your SSH key to the remote machine:
```bash
ssh-copy-id user@remote-machine-ip
```

3. Test the connection:
```bash
ssh user@remote-machine-ip
```

## Troubleshooting

Common issues and solutions:

1. **Permission Denied**
   - Ensure you have sudo privileges
   - Check SSH key permissions

2. **Package Installation Failures**
   - Check internet connectivity
   - Verify package names
   - Check system repositories

3. **SSH Connection Issues**
   - Verify SSH keys
   - Check firewall settings
   - Confirm network connectivity

4. **Multipass VM Connection Issues**
   - Standard SSH doesn't work with Multipass by default
   - Use the transfer-and-run.sh script instead
   - Or use `multipass shell <vm-name>` to access the VM directly

5. **Git Issues**
   - Check that Git is properly installed (`git --version`)
   - Ensure proper credentials are configured (`git config --global user.name` and `git config --global user.email`)
   - Set up SSH keys for Git if using SSH authentication