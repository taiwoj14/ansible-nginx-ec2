# Ansible Nginx Deployment on AWS EC2

## Project Overview

This project demonstrates automated installation and configuration of the Nginx web server on an AWS EC2 Linux instance using Ansible.

The project uses Ansible playbooks to automate server configuration, deploy a custom HTML webpage, and verify that the Nginx service is running correctly.

It demonstrates practical Infrastructure Automation, Linux Administration, AWS Cloud, and Configuration Management skills.

## Project Objectives

- Provision and access an AWS EC2 Linux instance.
- Establish secure SSH connectivity.
- Configure Ansible inventory.
- Automate Nginx installation using an Ansible playbook.
- Deploy a custom HTML webpage.
- Verify Nginx service availability.
- Test Ansible idempotency.
- Manage project files using Git and GitHub.

## Technologies Used

| Technology | Purpose |
|---|---|
| AWS EC2 | Cloud virtual server |
| Ubuntu | Ansible control node |
| Amazon Linux | Managed EC2 server |
| Ansible | Configuration management |
| Nginx | Web server |
| YAML | Playbook configuration |
| SSH | Secure remote connectivity |
| Git | Version control |
| GitHub | Source code hosting |

## Project Architecture

The Ansible control node runs from Ubuntu under WSL2 on Windows 11.

Ansible connects to the AWS EC2 managed node through SSH and executes tasks defined in the playbook.

The playbook installs Nginx, deploys the custom HTML page, and ensures that the Nginx service is running.

## Project Structure

```text
ansible-nginx-ec2/
├── files/
│   └── index.html
├── inventory/
│   └── hosts.ini.example
├── playbooks/
│   └── nginx-setup.yml
├── screenshots/
│   ├── 01-ec2-instance-running.png
│   ├── 02-security-group-rules.png
│   ├── 03-ssh-connection.png
│   ├── 04-ansible-inventory.png
│   ├── 05-nginx-playbook.png
│   ├── 06-successful-playbook-execution.png
│   ├── 07-nginx-service-running.png
│   ├── 08-custom-html-browser.png
│   ├── 09-idempotency-test.png
│   └── 10-ansible-ping-success.png
├── .gitignore
└── README.md
```

## Prerequisites

- An AWS account.
- An EC2 Linux instance.
- Ubuntu Linux with Ansible installed.
- SSH access to the EC2 instance.
- An SSH private key.
- Git installed.

## Deployment Instructions

### 1. Clone the repository

```bash
git clone https://github.com/taiwoj14/ansible-nginx-ec2.git
cd ansible-nginx-ec2
```

### 2. Configure the inventory

Create your working inventory from the example:

```bash
cp inventory/hosts.ini.example inventory/hosts.ini
```

Edit the inventory:

```bash
vi inventory/hosts.ini
```

Replace the placeholder with your EC2 public IP address and actual private key path.

Example:

```ini
[webservers]
YOUR_EC2_PUBLIC_IP ansible_user=ec2-user ansible_ssh_private_key_file=/path/to/your/private-key.pem
```

### 3. Test SSH connectivity

```bash
ansible webservers -i inventory/hosts.ini -m ping
```

A successful response should contain:

```text
"ping": "pong"
```

### 4. Run the Ansible playbook

```bash
ansible-playbook -i inventory/hosts.ini playbooks/nginx-setup.yml
```

### 5. Verify Nginx

Open a browser and navigate to:

```text
http://YOUR_EC2_PUBLIC_IP
```

The custom HTML webpage should be displayed.

## Testing and Verification

The project includes the following verification activities:

- Successful SSH connection to the EC2 instance.
- Successful Ansible ping test.
- Successful Nginx installation.
- Successful playbook execution.
- Nginx service verification.
- Custom webpage deployment.
- Idempotency testing to confirm repeated playbook execution does not produce unnecessary changes.

See the screenshots directory for evidence of the deployment and testing activities.

## Screenshots

The following screenshots document the implementation and successful testing of the project.

### AWS Infrastructure
![EC2 Instance Running](screenshots/01-ec2-instance-running.png)

![Security Group Rules](screenshots/02-security-group-rules.png)

### Ansible Configuration
![SSH Connection](screenshots/03-ssh-connection.png)

![Ansible Inventory](screenshots/04-ansible-inventory.png)

![Nginx Playbook](screenshots/05-nginx-playbook.png)

### Deployment and Testing
![Successful Playbook Execution](screenshots/06-successful-playbook-execution.png)

![Nginx Service Running](screenshots/07-nginx-service-running.png)

![Custom HTML Browser](screenshots/08-custom-html-browser.png)

![Idempotency Test](screenshots/09-idempotency-test.png)

![Ansible Ping Success](screenshots/10-ansible-ping-success.png)

## Security Considerations

- Private SSH keys are excluded using `.gitignore`.
- Actual inventory files containing server details are excluded from Git.
- A sample inventory file is provided for demonstration.
- SSH access should be restricted to trusted IP addresses.
- AWS security group rules should follow the principle of least privilege.
- Never commit passwords, private keys, or other sensitive credentials.

## Skills Demonstrated

- AWS EC2 administration.
- Linux server management.
- Ansible inventory configuration.
- YAML playbook development.
- Configuration management.
- SSH troubleshooting.
- Nginx deployment.
- Infrastructure automation.
- Git and GitHub version control.

## Author

**Taiwo Joseph**

GitHub: https://github.com/taiwoj14

## Project Repository

https://github.com/taiwoj14/ansible-nginx-ec2
