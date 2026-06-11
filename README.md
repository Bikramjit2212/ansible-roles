# 🚀 Ansible Roles for Automated Application Deployment

This project demonstrates how to use **Ansible Roles** to automate application deployment on an AWS EC2 instance. It showcases role-based organization of Ansible code, inventory management, templating, package installation, and deployment of a simple Flask backend application.

Although the playbook references separate `frontend` and `backend` roles, the repository currently contains a single role implementation that deploys a Flask application. The project serves as a hands-on example of structuring Ansible automation using roles.

---

## 📌 Project Overview

The automation workflow performs the following tasks:

* Connects to an AWS EC2 instance using SSH authentication.
* Uses an Ansible inventory to define target hosts.
* Executes deployment tasks through Ansible Roles.
* Installs Python dependencies required for the application.
* Deploys a Flask-based backend application.
* Starts the Flask application in the background.
* Demonstrates the use of Jinja2 templates.
* Includes the standard Ansible role directory structure.

---

## 🏗️ Project Structure

```text
ansible-roles-main/
├── inventory.yml
├── playbook.yml
└── command/
    ├── defaults/
    │   └── main.yml
    ├── handlers/
    │   └── main.yml
    ├── meta/
    │   └── main.yml
    ├── tasks/
    │   └── main.yml
    ├── templates/
    │   └── index.html.j2
    ├── tests/
    │   └── test.yml
    ├── vars/
    │   └── main.yml
    └── README.md
```

---

## ⚙️ Technologies Used

* Ansible
* YAML
* AWS EC2
* Ubuntu
* Python 3
* Flask
* Jinja2 Templates
* SSH

---

## 📋 Inventory Configuration

The inventory defines the target EC2 instance:

```yaml
all:
  children:
    web:
      hosts:
        aws1:
          ansible_host: ec2-xx-xx-xx-xx.compute-1.amazonaws.com
          ansible_user: ubuntu
          ansible_ssh_private_key_file: ~/.ssh/testkey.pem
```

---

## 🔧 Playbook Execution

The playbook invokes roles against the EC2 host:

```yaml
---
- hosts: aws1
  become: yes
  roles:
    - frontend
    - backend
```

> **Note:** The repository currently contains only the `command` role. The `frontend` and `backend` roles referenced in the playbook are not present and would require creation or modification before successful execution.

---

## 🚀 Role Tasks Implemented

The existing role performs the following operations:

### 1. Install Dependencies

Installs:

* Python 3
* Python Pip
* Flask package

```yaml
- python3
- python3-pip
- python3-flask
```

---

### 2. Deploy Flask Application

Creates an application file:

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Backend"

if __name__ == '__main__':
    app.run(port=5000)
```

The file is copied to:

```text
/home/ubuntu/app.py
```

---

### 3. Start the Application

Runs the Flask application in the background:

```bash
nohup python3 /home/ubuntu/app.py &
```

---

## 🧩 Jinja2 Template Usage

The role contains a template file:

```html
<html>
    <head>
        <title>{{ nginx_title }}</title>
        <body>
            <h1>{{ nginx_message }}</h1>
        </body>
    </head>
</html>
```

Default template variables:

```yaml
nginx_title: "Welcome to my Website"
nginx_message: "Deployed using Ansible Role"
```

> This template is currently not referenced by any task.

---

## ▶️ Running the Playbook

### Clone the Repository

```bash
git clone <repository-url>
cd ansible-roles-main
```

### Verify SSH Access

Ensure your SSH key exists:

```bash
~/.ssh/testkey.pem
```

Set proper permissions:

```bash
chmod 400 ~/.ssh/testkey.pem
```

### Execute the Playbook

```bash
ansible-playbook -i inventory.yml playbook.yml
```

---

## 🌐 Accessing the Application

After deployment, the Flask application listens on:

```text
http://<EC2-Public-IP>:5000
```

Expected output:

```text
Hello from Backend
```

---

## ⚠️ Improvements Recommended

This repository is a learning project and can be enhanced further by:

* Creating dedicated `frontend` and `backend` roles.
* Using the `template` module for frontend deployment.
* Replacing shell-based execution with systemd services.
* Making deployments idempotent.
* Using Ansible Vault for secrets management.
* Adding handlers to restart services automatically.
* Implementing role dependencies properly.
* Using group variables and host variables.

---

## 📚 Key DevOps Concepts Demonstrated

* Infrastructure Automation
* Configuration Management
* Role-Based Ansible Design
* Inventory Management
* Remote Package Installation
* Application Deployment Automation
* SSH-Based Server Provisioning
* Jinja2 Templating Fundamentals

---

# 👨‍💻 Author

**Bikramjit Roy**

DevOps & Cloud Engineering Enthusiast passionate about automation, CI/CD, cloud-native practices, and building reliable software delivery pipelines.

GitHub:
https://github.com/Bikramjit2212

---

## ⭐ If you found this project useful, consider giving it a star.
