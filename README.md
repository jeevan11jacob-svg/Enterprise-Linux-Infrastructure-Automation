# Enterprise Linux Infrastructure Automation using Ansible

## Project Overview

Enterprise Linux Infrastructure Automation is an Ansible-based automation project designed to simplify the configuration and management of multiple RHEL 9 servers.

The project uses a single Ansible Control Node to automate server administration tasks such as user management, web server deployment, database server configuration, backup automation, and secure management of sensitive information using Ansible Vault.

This project follows the Infrastructure as Code (IaC) approach, where server configurations are defined using reusable Ansible roles and YAML-based playbooks.

## Project Objectives

- Automate Linux server configuration and management.
- Reduce manual administration tasks.
- Manage multiple RHEL 9 servers from a single control node.
- Deploy and configure Apache web servers automatically.
- Install and manage MariaDB database servers.
- Automate backup operations.
- Secure sensitive information using Ansible Vault.
- Implement reusable Ansible roles for better organization.


## Technologies Used

| Technology | Purpose |
|------------|---------|
| RHEL 9 | Operating System for managed servers |
| Ansible | Automation and configuration management tool |
| YAML | Language used for Ansible playbooks and configuration |
| Jinja2 | Template engine for dynamic configuration files |
| Apache HTTP Server | Web server deployment and management |
| MariaDB | Database server installation and configuration |
| Ansible Vault | Secure storage of sensitive information |
| SSH | Secure communication between Ansible Control Node and managed nodes |
| Git & GitHub | Version control and project hosting |

## Project Architecture

The project follows a centralized automation architecture where one Ansible Control Node manages multiple RHEL 9 client servers through SSH.

```text
                  Ansible Control Node
                          |
                          |
              ---------------------------
              |                         |
              |                         |
        Web Server              Database Server
        RHEL 9                 RHEL 9
        Apache                 MariaDB
              |                         |
              |                         |
              ---------------------------
                          |
                          |
                    Backup Automation
```

## Project Structure

The project is organized using Ansible roles to provide a modular and reusable automation structure.

```text
my_project/
│
├── ansible.cfg
├── inventory
├── site.yml
├── README.md
├── .gitignore
│
└── roles/
    │
    ├── common/
    │   └── tasks/
    │
    ├── users/
    │   └── tasks/
    │
    ├── webserver/
    │   ├── tasks/
    │   ├── templates/
    │   └── handlers/
    │
    ├── database/
    │   └── tasks/
    │
    └── backup/
        ├── tasks/
        ├── files/
        └── templates/
```

## Ansible Roles Description

The project uses Ansible roles to organize automation tasks into reusable and manageable components.

### 1. Common Role

**Purpose:**
Provides common configuration tasks required for all managed servers.

**Responsibilities:**
- Performs basic system configuration.
- Installs required common packages.
- Prepares servers for further automation.

---

### 2. Users Role

**Purpose:**
Automates user account management across servers.

**Responsibilities:**
- Creates users on managed nodes.
- Configures user-related settings.
- Maintains consistent user management.

---

### 3. Webserver Role

**Purpose:**
Automates Apache web server deployment.

**Responsibilities:**
- Installs Apache HTTP Server (`httpd`).
- Starts and enables Apache service.
- Deploys website files using Jinja2 templates.
- Configures firewall rules for HTTP access.

**Verification:**

```bash
curl http://<web-server-ip>
```

---

### 4. Database Role

**Purpose:**
Automates MariaDB database server installation and configuration.

**Responsibilities:**
- Installs MariaDB server package.
- Starts and enables MariaDB service.
- Configures the database server environment.

**Verification:**

```bash
systemctl status mariadb
```

---

### 5. Backup Role

**Purpose:**
Automates backup operations on managed servers.

**Responsibilities:**
- Creates backup directories.
- Deploys backup scripts.
- Automates backup execution.
- Creates compressed backup archives using tar.

**Verification:**

```bash
ls -lh /backup
```

---

## Features

The project provides the following automation features:

- Centralized management of multiple RHEL 9 servers using Ansible.
- Automated server configuration using reusable Ansible roles.
- Automated user creation and management.
- Apache web server installation and configuration.
- Website deployment using Jinja2 templates.
- MariaDB server installation and service management.
- Automated backup creation using shell scripts and tar.
- Secure management of sensitive information using Ansible Vault.
- Consistent infrastructure deployment using Infrastructure as Code (IaC).


---

## Prerequisites

Before running this project, ensure the following requirements are available:

- RHEL 9 machines for the Ansible Control Node and managed nodes.
- Ansible installed on the Control Node.
- SSH connectivity configured between the Control Node and managed nodes.
- Proper inventory file configuration.
- Sudo privileges for the Ansible user.
- Required Ansible collections installed (if applicable).

---

## Installation and Setup

### 1. Clone the Repository

Clone the project repository from GitHub.

```bash
git clone https://github.com/jeevan11jacob-svg/Enterprise-Linux-Infrastructure-Automation.git
cd my_project
```

### 2. Configure the Inventory File

Edit the inventory file and add the IP addresses of the managed nodes.

Example:

```ini
[webservers]
192.168.33.122

[databases]
192.168.33.102
```

### 3. Verify SSH Connectivity

Verify communication between the Ansible Control Node and the managed nodes.

```bash
ansible all -m ping
```

### 4. Execute the Playbook

Run the main playbook to configure all managed servers.

```bash
ansible-playbook site.yml --ask-vault-pass
```

---

## Verification / Results

The successful execution of the Ansible automation was verified using the following checks:

### 1. Ansible Connectivity Verification

Connectivity between the Ansible Control Node and managed nodes was tested successfully.

```bash
ansible all -m ping
```

### 2. Web Server Verification

The Apache web server deployment was verified by accessing the hosted webpage.

```bash
curl http://192.168.33.122
```

Example output:

```html
<html>
<head>
    <title>Enterprise Linux Automation</title>
</head>

<body>
    <h1>Enterprise Linux Infrastructure Automation</h1>
    <p>Apache Web Server deployed successfully using Ansible.</p>
</body>
</html>
```

### 3. Database Server Verification

The MariaDB service status was checked on the database server.

```bash
systemctl status mariadb
```

Example output:

```text
● mariadb.service - MariaDB Database Server
     Loaded: loaded
     Active: active (running)
```

### 4. Backup Verification

The backup directory was checked to verify successful backup creation.

```bash
ls -lh /backup
```

The backup role successfully created compressed backup archives on the managed servers.

Example output:

```text
-rw-r--r--. 1 root root 4.6M Jul 14 07:57 etc_backup.tar.gz
```

---

## Ansible Playbook Execution Flow

The project follows a structured automation workflow where the Ansible Control Node manages all configured RHEL 9 managed nodes.

```text
              Administrator
                    |
                    |
             Run Ansible Playbook
                    |
                    |
             Read Inventory File
                    |
                    |
          Connect to Managed Nodes
                 using SSH
                    |
                    |
           Execute Ansible Roles
                    |
        ---------------------------
        |        |        |       |
     Common   Users  Webserver Database
        |        |        |       |
        ---------------------------
                    |
                 Backup
                    |
     Server Configuration Completed
        
```

The execution process:

1. The administrator runs the Ansible playbook from the Control Node.
2. Ansible reads the inventory file to identify managed servers.
3. SSH is used to establish communication with managed nodes.
4. Ansible executes the assigned roles.
5. Each role performs its specific configuration tasks.
6. The managed servers are configured automatically according to the defined infrastructure requirements.

---

## Security Management using Ansible Vault

Ansible Vault is used in this project to securely store sensitive information such as database passwords, secret keys, and other confidential configuration data.

Instead of storing sensitive values directly inside playbooks or variable files, Ansible Vault encrypts the data and allows it to be accessed securely during playbook execution.

### Benefits of Using Ansible Vault

- Protects sensitive information from unauthorized access.
- Prevents passwords and secrets from being exposed in GitHub repositories.
- Allows encrypted variable files to be managed along with the project.
- Provides secure automation without storing plain-text credentials.

### Vault Usage in the Project

Sensitive variables are stored in encrypted files and accessed during Ansible playbook execution.

Example:

```bash
ansible-vault create group_vars/all/vault.yml
```

To execute the playbook with encrypted variables:

```bash
ansible-playbook site.yml --ask-vault-pass
```

The vault password is requested during execution, allowing Ansible to decrypt and use the required sensitive information securely.

---

## Future Enhancements

Although this project automates major Linux infrastructure management tasks, it can be extended with additional enterprise-level features in the future.

### Possible Enhancements:

- **Monitoring Integration**
  - Integrate monitoring tools such as Prometheus and Grafana to monitor server performance and availability.

- **CI/CD Pipeline Integration**
  - Automate Ansible testing and deployment using Jenkins or GitHub Actions.

- **Dynamic Inventory**
  - Implement dynamic inventory management for cloud-based environments such as AWS, Azure, or Google Cloud.

- **Log Management**
  - Integrate centralized logging solutions such as ELK Stack for better troubleshooting and analysis.

- **Container Support**
  - Extend automation to deploy and manage containerized applications using Docker or Kubernetes.

- **Security Hardening**
  - Add automated security configurations such as firewall policies, SELinux management, and compliance checks.

These enhancements can help transform the project into a complete enterprise infrastructure automation platform.

---

## Conclusion

The Enterprise Linux Infrastructure Automation project demonstrates how Ansible can be used to automate and manage enterprise Linux environments efficiently.

By implementing reusable Ansible roles, the project automates important system administration tasks including user management, Apache web server deployment, MariaDB database configuration, and backup operations.

The project follows Infrastructure as Code (IaC) principles, reducing manual configuration errors and providing a consistent approach for managing multiple RHEL 9 servers.

Through the use of Ansible Vault, sensitive information is protected, ensuring secure automation practices.

This project provides a foundation for building scalable and reliable enterprise infrastructure automation solutions.
