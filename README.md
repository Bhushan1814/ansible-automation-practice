# Ansible Automation Practice

A hands-on collection of Ansible playbooks for practicing Linux server
automation and configuration management on AWS EC2.

This repository contains YAML playbooks for package installation,
service management, file and directory operations, web-server setup,
LAMP/LEMP stack configuration, and handler-based service restarts.

## Project Objectives

-   Automate repetitive Linux administration tasks.
-   Install and manage web-server and database packages.
-   Start and enable Linux services with Ansible.
-   Use variables and loops to make playbooks reusable.
-   Create, copy, and manage files and directories.
-   Practice inventory management and multi-server concepts.
-   Use handlers to restart Nginx when its configuration changes.

## Technologies Used

-   Ansible
-   AWS EC2
-   Linux
-   YAML
-   Nginx
-   Apache HTTP Server (`httpd`)
-   MariaDB
-   PHP and PHP-FPM

## Repository Structure

``` text
ansible-automation-practice/
├── 2-tier.yml
├── add_content.yml
├── create_file.yml
├── install_lamp.yml
├── install_tree.yml
├── inventory.ini
├── lamp_install_target.yml
├── lemp_with_var.yml
├── optimize_script.yml
├── task-1.yml
├── with_handlers.yml
├── README.md
└── .gitignore
```

## Playbook Overview

  -----------------------------------------------------------------------
  File                                Purpose
  ----------------------------------- -----------------------------------
  `2-tier.yml`                        Practices a web-server and
                                      database-server setup using
                                      separate play sections.

  `add_content.yml`                   Creates a text file and adds
                                      content to it.

  `create_file.yml`                   Creates a file using the Ansible
                                      file module.

  `install_lamp.yml`                  Installs Apache, MariaDB, and
                                      PHP-related packages; manages
                                      services and deploys a PHP test
                                      page.

  `install_tree.yml`                  Installs the `tree` package.

  `inventory.ini`                     Defines example web-server and
                                      database-server inventory groups.

  `lamp_install_target.yml`           Installs LAMP-related packages and
                                      manages services on a target host
                                      group.

  `lemp_with_var.yml`                 Uses variables to install and
                                      configure Nginx, MariaDB, PHP, and
                                      PHP-FPM.

  `optimize_script.yml`               An additional practice playbook;
                                      review its tasks before execution.

  `task-1.yml`                        Practices Nginx installation,
                                      directory/file creation, and file
                                      copying.

  `with_handlers.yml`                 Configures an Nginx server block to
                                      listen on port 82 and uses a
                                      handler to restart Nginx after
                                      configuration changes.
  -----------------------------------------------------------------------

> Note: Some playbooks use `localhost`, while others refer to groups
> such as `targetserver` or `webservers`. Make sure the `hosts:` value
> in each playbook matches the groups in your inventory, or
> intentionally run it locally.

## Prerequisites

-   An AWS EC2 Linux instance or another supported Linux machine.
-   Ansible installed on the control node.
-   SSH connectivity from the control node to managed EC2 instances.
-   A valid SSH private key with restricted permissions.
-   Appropriate security-group and host firewall rules for the services
    you test.

The package tasks in this collection use `ansible.builtin.yum`, so they
are intended for compatible RHEL-family/Amazon Linux environments.
Package names and service names can differ on Ubuntu or other
distributions.

## Setup

### 1. Install Ansible on the control node

Install Ansible using the method recommended for your control-node
operating system. Verify the installation:

``` bash
ansible --version
```

### 2. Configure the inventory

Update `inventory.ini` with the private IP addresses or DNS names of
your EC2 instances.

Example format:

``` ini
[webserver]
WEB_SERVER_PRIVATE_IP ansible_user=ec2-user ansible_ssh_private_key_file=/path/to/your-key.pem

[dbserver]
DB_SERVER_PRIVATE_IP ansible_user=ec2-user ansible_ssh_private_key_file=/path/to/your-key.pem
```

Replace the example values with your own environment details. Do not
commit real private IP details if they are sensitive, SSH private keys,
or credentials.

### 3. Test connectivity

``` bash
ansible all -i inventory.ini -m ping
```

The inventory group names must match the host groups referenced by the
playbook. For example, if a playbook says `hosts: targetserver`, define
a `[targetserver]` group or update the playbook to use an existing
group.

## Running Playbooks

Run a playbook from the repository directory:

``` bash
ansible-playbook -i inventory.ini install_tree.yml
```

Examples:

``` bash
ansible-playbook -i inventory.ini install_lamp.yml
ansible-playbook -i inventory.ini lemp_with_var.yml
ansible-playbook -i inventory.ini lamp_install_target.yml
ansible-playbook -i inventory.ini with_handlers.yml
```

Use the correct inventory and host group for each playbook. Some
examples target `localhost`; for those, review the playbook and run it
on the intended control host.

For syntax validation:

``` bash
ansible-playbook --syntax-check -i inventory.ini with_handlers.yml
```

For a dry-run where supported:

``` bash
ansible-playbook --check -i inventory.ini with_handlers.yml
```

## Example: Nginx Handler Playbook

The `with_handlers.yml` playbook demonstrates this workflow:

1.  Installs Nginx.
2.  Ensures the Nginx service is enabled and running.
3.  Adds a server block configured to listen on port `82`.
4.  Deploys simple HTML content.
5.  Notifies a handler to restart Nginx when the configuration task
    changes.

To access the test page, allow inbound TCP port `82` in the EC2 security
group and any applicable host firewall, then visit:

``` text
http://EC2_PUBLIC_IP:82
```

Only expose ports that are required for your test, and restrict source
IP ranges where possible.

## Important Security Notes

-   Never upload `.pem` or other private-key files to GitHub.
-   Do not store passwords, access keys, tokens, or other secrets in
    playbooks or inventory files.
-   Use placeholders in example inventory files.
-   Restrict SSH access and application ports in security groups.
-   Review each playbook before running it, especially tasks that use
    shell commands or modify system configuration.

## What I Practiced

-   Writing Ansible playbooks in YAML.
-   Using modules such as `yum`, `systemd_service`, `copy`, `file`, and
    `blockinfile`.
-   Defining and reusing variables.
-   Iterating over service lists with loops.
-   Organizing hosts with inventory groups.
-   Using handlers for configuration-triggered service restarts.
-   Automating web-server and database package setup.

## Future Improvements

-   Add Ansible roles to organize reusable tasks.
-   Move environment-specific values into `group_vars` and `host_vars`.
-   Use Ansible Vault for sensitive variables.
-   Add validation tasks and handlers for all service configuration
    changes.
-   Test the playbooks on clean EC2 instances and document verified
    outputs.

## Author

**Bhushan Jagtap**

GitHub: [Bhushan1814](https://github.com/Bhushan1814)

------------------------------------------------------------------------

This repository is part of my hands-on learning journey in AWS Cloud,
Linux, and DevOps.
