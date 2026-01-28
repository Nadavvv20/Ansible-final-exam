# 🚀 AWS Infrastructure Manager | Ansible Magic
**Dynamic Infrastructure Management via AWS Tags**

---

## 📖 Overview
This project replaces static **AMI templates** with a dynamic **Ansible-driven workflow**. Using vanilla Amazon Linux 2023 instances, we automate software installation, versioning, and system maintenance based entirely on **AWS Instance Tags**.

---

## 🧠 Why Ansible over AMI?

### **The Pros**
* **Version Control:** All configuration is tracked in Git, not hidden in a disk image "black box".
* **Dynamic Flexibility:** One playbook manages multiple server types just by changing a tag in the AWS Console.
* **Cost Efficiency:** No storage costs or management overhead for dozens of custom AMIs.

### **The Cons**
* **Boot Time:** Fresh instances take longer to be "Ready" because they install packages on the fly.
* **Internet Dependency:** Deployment can fail if external package repositories (Nginx/MariaDB) are unreachable.

---

## 🏷️ Tag-Driven Logic
The playbook execution is controlled by these AWS Tags:

| Tag | Purpose | Example |
| :--- | :--- | :--- |
| **`Managed`** | Must be `true` for Ansible to execute. | `true` |
| **`Name`** | Sets the OS internal hostname. | `db-server-01` |
| **`Service`** | Software to install (nginx, mariadb105-server). | `nginx` |
| **`Version`** | Specific version or `latest`. Supports downgrades. | `1.24` |
| **`Restart`** | Human-readable schedule for Crontab reboots. | `Saturday 00:00` |
| **`DockerImage`**| (Bonus) Image for containerized deployment. | `mariadb:10.5` |

---

## 🛠️ Key Features

### **1. Smart Installation & Downgrade**
The playbook uses the `dnf` module with `allow_downgrade: true`. If the tag version is lower than the installed one, Ansible handles the rollback automatically to ensure environment parity.

### **2. Human-to-Cron Parser**
Instead of hardcoded values, the playbook features a generic parser. It takes a tag like `Sunday 06:00` and converts it into a valid Crontab entry for system reboots.

### **3. Docker Integration (Bonus)**
The `bonus_playbook.yaml` handles:
* Automatic Docker Engine installation.
* Container deployment with environment variables (e.g., `MYSQL_ROOT_PASSWORD`).
* Host-to-Container port mapping (e.g., `8080:80`).

---

## 🚀 Quick Start

1. **Configure Inventory:** Update `aws_ec2.yaml` with your AWS region.
2. **Run Standard Playbook:**
   ```bash
   ansible-playbook -i aws_ec2.yaml Exam_playbook.yaml


Developed by Nadav | DevOps Engineer
