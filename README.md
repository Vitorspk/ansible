# Ansible Infrastructure Automation

This repository contains Ansible playbooks and roles for automating infrastructure configuration and management tasks across multiple servers.

## Overview

This project provides a comprehensive Ansible automation solution for managing web and database servers. It includes playbooks for system updates, package management, and service configuration across different Linux distributions (CentOS, RedHat, Ubuntu, Debian).

## Project Structure

```
ansible/
├── ansible.cfg                 # Ansible configuration file
├── hosts                       # Inventory file defining target hosts
├── group_vars/                 # Group-specific variables
├── playbook.yml               # Main playbook for running updates
├── playbook2.yml              # Nginx installation and management
├── playbook3.yml              # Package removal and installation
├── roles/                     # Ansible roles
│   ├── common/                # Common tasks for all servers
│   │   ├── handlers/
│   │   ├── meta/
│   │   └── tasks/
│   ├── php/                   # PHP installation role
│   │   ├── handlers/
│   │   └── tasks/
│   └── updates/               # System update role
│       ├── handlers/
│       └── tasks/
└── vagrant_ubuntu_*/          # Vagrant configurations

```

## Requirements

- Ansible 2.9 or higher
- SSH access to target hosts
- Python installed on target hosts
- Sudo privileges on target hosts

## Configuration

### Inventory

The `hosts` file defines the target servers:

- **Web Servers** (`servidores_web`): 192.168.33.10
- **Database Servers** (`servidores_db`): 192.168.33.11

### Ansible Configuration

Key settings in `ansible.cfg`:
- Default user: `vagrant`
- SSH port: 22
- Privilege escalation: sudo to root
- Host key checking: disabled (for development)

## Playbooks

### 1. Main Playbook (`playbook.yml`)
Runs system updates across all hosts using the `updates` role.

```bash
ansible-playbook playbook.yml
```

### 2. Nginx Installation (`playbook2.yml`)
Installs and configures Nginx on web servers.

```bash
ansible-playbook playbook2.yml
```

Features:
- Installs latest Nginx version
- Includes handlers for service management
- Targets only web servers group

### 3. Package Management (`playbook3.yml`)
Manages package installation and removal on web servers.

```bash
ansible-playbook playbook3.yml
```

Manages packages:
- Removes: nginx, httpd (if present)
- Installs: nmap, nginx, vim, tcpdump

## Roles

### Common Role
General server configuration tasks:
- System package updates (yum-based systems)
- Essential tools installation (net-tools, nano, nmap)
- Timezone configuration (America/Sao_Paulo)
- File backup and template deployment
- Nginx installation (for Debian-based systems)

### Updates Role
Cross-platform system updates:
- **Red Hat/CentOS**: Security updates via yum
- **Debian/Ubuntu**: Full system upgrade via apt
- Automatically detects distribution and applies appropriate update method

### PHP Role
Simple PHP5 installation for web servers (Debian-based systems).

## Usage Examples

### Run updates on all servers
```bash
ansible-playbook -i hosts playbook.yml
```

### Install Nginx on web servers only
```bash
ansible-playbook -i hosts playbook2.yml
```

### Apply common configuration to specific hosts
```bash
ansible-playbook -i hosts playbook.yml --limit servidores_web
```

### Check playbook syntax
```bash
ansible-playbook --syntax-check playbook.yml
```

### Dry run (check mode)
```bash
ansible-playbook -i hosts playbook.yml --check
```

## Development Environment

This project includes Vagrant configurations for local testing:
- `vagrant_ubuntu_01/`: Ubuntu test environment
- `vagrant_ubuntu_02/`: Additional Ubuntu test environment

To start the Vagrant environments:
```bash
cd vagrant_ubuntu_01
vagrant up
```

## Security Considerations

- SSH keys are used for authentication (no password prompts)
- Privilege escalation is configured for sudo operations
- Consider enabling host key checking in production
- Review and customize the `ansible.cfg` for production use

## Best Practices

1. Always test playbooks in development before production
2. Use `--check` mode to preview changes
3. Keep sensitive data in encrypted vault files
4. Use version control for all playbook changes
5. Document any custom modifications to roles

## Troubleshooting

### Common Issues

1. **Connection refused**: Verify SSH access and host availability
2. **Permission denied**: Check sudo configuration on target hosts
3. **Package not found**: Ensure repository configuration is correct
4. **Handler not found**: Verify handler names match between tasks and handlers

### Debug Mode
Run playbooks with increased verbosity:
```bash
ansible-playbook -i hosts playbook.yml -vvv
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Test your changes thoroughly
4. Submit a pull request with clear description

## License

This project is licensed under the terms included in the LICENSE file.

## Support

For issues, questions, or suggestions, please open an issue in the repository.
