# Ansible Nginx Deployment on AWS EC2

## Project Overview

This project demonstrates automated installation, configuration, and deployment of the Nginx web server on an AWS EC2 Linux instance using Ansible.

Ansible connects to the EC2 managed node over SSH and uses a reusable Ansible role to install Nginx, manage its service, and deploy a custom HTML webpage.

The project demonstrates practical skills in AWS Cloud, Linux administration, configuration management, infrastructure automation, Ansible Vault, and version control.

## Project Objectives

- Provision and configure an AWS EC2 Linux instance.
- Establish secure SSH connectivity between Ansible and EC2.
- Configure Ansible inventory and project settings.
- Organize deployment tasks into a reusable Ansible role.
- Manage configuration using variables and Jinja2 templates.
- Use Ansible handlers for configuration changes.
- Protect sensitive variables using Ansible Vault.
- Deploy a custom HTML webpage.
- Verify web server availability.
- Test playbook syntax and idempotency.
- Manage project code using Git and GitHub.

## Project Architecture

The project uses an agentless Ansible architecture.

The Ansible control node runs on Ubuntu through WSL2 on Windows 11. It connects to an AWS EC2 managed node running Amazon Linux 2023 using SSH.

Ansible executes the deployment playbook, which calls the Nginx role to install and configure the web server and deploy the custom webpage.

### Architecture Flow

```text
Windows 11 Laptop
       |
       v
Ubuntu (WSL2)
Ansible Control Node
       |
       | SSH Connection - Port 22
       v
AWS EC2 Instance
Amazon Linux 2023
       |
       v
Ansible Playbook
       |
       v
Nginx Role
       |
       +---- Install Nginx
       |
       +---- Manage Nginx Service
       |
       +---- Deploy Jinja2 HTML Template
       |
       +---- Verify Vault Variable
       |
       v
Nginx Web Server
       |
       | HTTP Port 80
       v
Custom Webpage
```

## Technologies and Tools

| Technology | Purpose |
|---|---|
| AWS EC2 | Cloud infrastructure |
| Amazon Linux 2023 | Managed server operating system |
| Ubuntu WSL2 | Ansible control node |
| Ansible | Configuration management and automation |
| Ansible Roles | Reusable automation structure |
| Ansible Vault | Encryption of sensitive variables |
| Jinja2 | Dynamic HTML template rendering |
| Nginx | Web server |
| YAML | Ansible configuration |
| SSH | Secure remote connectivity |
| Git | Version control |
| GitHub | Source code hosting |

## Project Structure

```text
ansible-nginx-ec2/
│
├── ansible.cfg
├── .gitignore
├── README.md
│
├── inventory/
│   ├── hosts.ini
│   └── hosts.ini.example
│
├── playbooks/
│   └── nginx-setup.yml
│
├── roles/
│   └── nginx/
│       ├── defaults/
│       │   └── main.yml
│       ├── handlers/
│       │   └── main.yml
│       ├── tasks/
│       │   └── main.yml
│       └── templates/
│           └── index.html.j2
│
├── vars/
│   └── vault.yml
│
├── files/
│   └── index.html
│
└── screenshots/
    ├── 01-ec2-instance-running.png
    ├── 02-security-group-rules.png
    ├── 03-ssh-connection.png
    ├── 04-ansible-inventory.png
    ├── 05-nginx-playbook.png
    ├── 06-successful-playbook-execution.png
    ├── 07-nginx-service-running.png
    ├── 08-custom-html-browser.png
    ├── 09-idempotency-test.png
    └── 10-ansible-ping-success.png
```

**Note:** `inventory/hosts.ini` is a local configuration file and is excluded from Git. The encrypted `vars/vault.yml` is maintained separately from the role defaults.

## Prerequisites

Before running this project, ensure you have:

- An AWS account.
- An AWS EC2 instance running Amazon Linux 2023.
- Ubuntu Linux with Ansible installed.
- Python available on the managed node.
- SSH access to the EC2 instance.
- An SSH private key.
- Git installed.
- EC2 Security Group rules allowing SSH (port 22) and HTTP (port 80).

## Deployment Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/taiwoj14/ansible-nginx-ec2.git

cd ansible-nginx-ec2
```

### 2. Configure the Inventory

Create your working inventory from the example file:

```bash
cp inventory/hosts.ini.example inventory/hosts.ini
```

Edit the inventory:

```bash
code inventory/hosts.ini
```

Replace the placeholders with your EC2 public IP address and SSH private key location.

Example:

```ini
[webservers]
YOUR_EC2_PUBLIC_IP ansible_user=ec2-user ansible_ssh_private_key_file=/path/to/your/private-key.pem
```

Set appropriate private key permissions:

```bash
chmod 400 /path/to/your/private-key.pem
```

### 3. Configure Ansible Vault

The project uses Ansible Vault to encrypt a demonstration variable.

The encrypted file is:

```text
vars/vault.yml
```

The variable used by the playbook is:

```yaml
app_demo_api_token: "DEMO-ONLY-CHANGE-ME"
```

The demonstration value is not a production credential.

The Vault password must be kept private and must never be committed to GitHub.

If reproducing the project with your own Vault password, create your own encrypted variables file containing the required variable.

### 4. Test Ansible Connectivity

```bash
ansible webservers -m ping
```

A successful response should contain:

```text
"ping": "pong"
```

### 5. Check Playbook Syntax

Because the playbook loads an encrypted Vault file, provide the Vault password:

```bash
ansible-playbook --syntax-check playbooks/nginx-setup.yml --ask-vault-pass
```

Expected result:

```text
playbook: playbooks/nginx-setup.yml
```

### 6. Execute the Deployment Playbook

```bash
ansible-playbook playbooks/nginx-setup.yml --ask-vault-pass
```

Ansible prompts for the Vault password.

The playbook performs the following operations:

1. Gathers facts from the EC2 managed node.
2. Verifies that the encrypted Vault variable is loaded.
3. Installs Nginx using the DNF package manager.
4. Removes an obsolete duplicate Nginx configuration file.
5. Starts and enables the Nginx service.
6. Deploys the custom HTML page using a Jinja2 template.

### 7. Verify the Nginx Service

Check Nginx configuration:

```bash
ansible webservers -b -m command -a "nginx -t"
```

Check the HTTP response from the EC2 instance:

```bash
curl -I http://YOUR_EC2_PUBLIC_IP
```

A successful response should contain:

```text
HTTP/1.1 200 OK
Server: nginx
```

### 8. Access the Deployed Webpage

Open a browser and navigate to:

```text
http://YOUR_EC2_PUBLIC_IP
```

The custom Ansible-managed webpage should be displayed.

## Ansible Role Design

The Nginx deployment is organized into a reusable role.

- **defaults/main.yml:** Defines default package, service, web directory, ownership, file permissions, and environment variables.
- **tasks/main.yml:** Installs Nginx, removes the obsolete configuration, manages the service, and deploys the HTML template.
- **handlers/main.yml:** Defines the Nginx reload handler.
- **templates/index.html.j2:** Generates the custom HTML page using Jinja2 variables.

The playbook calls the role rather than containing all deployment tasks directly.

## Testing and Verification

The project was tested successfully through the following activities:

- SSH connectivity to the AWS EC2 instance.
- Ansible ping test returned `pong`.
- Ansible Vault decryption and variable verification.
- Playbook syntax validation.
- Successful Nginx installation.
- Nginx service confirmed active and enabled.
- Nginx configuration test passed.
- Custom HTML webpage successfully deployed.
- HTTP request returned `200 OK`.
- Repeated playbook execution confirmed idempotent behaviour.

### Ansible Vault Verification

A pre-task uses the Ansible `assert` module to verify that `app_demo_api_token` is defined without displaying its value.

### Idempotency Test

The deployment playbook was executed repeatedly.

The latest successful execution returned:

```text
PLAY RECAP

ok=6
changed=0
unreachable=0
failed=0
skipped=0
```

This confirms that Ansible detected the desired configuration was already in place and did not make unnecessary changes.

## Screenshots

The screenshots directory contains evidence of the infrastructure setup, configuration, deployment, and verification.

### AWS Infrastructure

![EC2 Instance Running](screenshots/01-ec2-instance-running.png)

![Security Group Rules](screenshots/02-security-group-rules.png)

### Ansible Configuration

![SSH Connection](screenshots/03-ssh-connection.png)

![Ansible Inventory](screenshots/04-ansible-inventory.png)

![Nginx Playbook](screenshots/05-nginx-playbook.png)

### Deployment and Verification

![Successful Playbook Execution](screenshots/06-successful-playbook-execution.png)

![Nginx Service Running](screenshots/07-nginx-service-running.png)

![Custom HTML Webpage](screenshots/08-custom-html-browser.png)

![Idempotency Test](screenshots/09-idempotency-test.png)

![Ansible Ping Success](screenshots/10-ansible-ping-success.png)

## Security Considerations

- SSH private keys are excluded from Git using `.gitignore`.
- Actual inventory files containing server details are excluded from version control.
- A sample inventory file is provided for demonstration.
- SSH access should be restricted to trusted IP addresses.
- AWS Security Group rules should follow the principle of least privilege.
- Sensitive credentials should be encrypted using Ansible Vault.
- Vault passwords must never be committed to GitHub.
- The demonstration Vault variable is not a production secret.

## Skills Demonstrated

- AWS EC2 administration.
- Linux server administration.
- Ansible configuration management.
- Ansible roles and reusable automation.
- Ansible variables and Jinja2 templates.
- Ansible handlers.
- Ansible Vault.
- SSH connectivity and troubleshooting.
- Nginx installation and deployment.
- Infrastructure automation.
- Idempotency testing.
- Git and GitHub version control.

## Author

**Taiwo Joseph**

GitHub: https://github.com/taiwoj14

## Project Repository

https://github.com/taiwoj14/ansible-nginx-ec2
