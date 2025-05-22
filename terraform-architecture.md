# ☁️ Terraform Architecture Overview

![Terraform Architecture](../diagrams/terraform-arch.png)

---

## 📌 Overview

Terraform by HashiCorp is an open-source Infrastructure as Code (IaC) tool that enables you to define, provision, and manage cloud infrastructure using a declarative configuration language (`.tf` files).

This architecture includes:

- **User (DevOps/Infra Engineer)**
- **Terraform Core**
- **Providers and Provisioners**
- **State Management**
- **Cloud Service Providers**

---

## 👨‍💻 DevOps/Infra Engineer

A DevOps or infrastructure engineer writes **Terraform manifest files** (`.tf`) using HCL (HashiCorp Configuration Language). These files define the desired infrastructure state.

### 🔹 Common Commands

- `terraform plan`: Shows what Terraform will do.
- `terraform apply`: Applies the changes to reach the desired infrastructure state.

---

## 🧠 Terraform Core

This is the heart of Terraform, responsible for:

- Parsing configuration files
- Managing state
- Creating execution plans
- Communicating with providers

It reads `.tf` files and interacts with the **Terraform Providers** and **Provisioners** to manage infrastructure.

---

## 🧩 Providers

These are plugins that allow Terraform to interact with various platforms such as:

- **Cloud Providers**: AWS, Azure, GCP, VMware, OpenShift, etc.
- **Other Services**: DNS, monitoring, databases, etc.

Providers expose **resources** (like `aws_instance`, `azurerm_storage_account`) that Terraform can manage.

---

## 🛠️ Provisioners

Used to execute scripts or commands on a local or remote machine as part of resource creation or destruction.

Examples:
- `remote-exec`
- `local-exec`

Note: Provisioners are not always recommended unless absolutely necessary.

---

## 🗂️ State File (`terraform.tfstate`)

Terraform stores the current state of your infrastructure in a state file:
- Tracks what resources exist
- Helps Terraform know what to change, add, or destroy
- Can be stored locally or remotely (e.g., in S3 with locking via DynamoDB)

---

## ☁️ Cloud Service Providers

Terraform uses providers to create resources on platforms like:

- **Amazon Web Services (AWS)**
- **Microsoft Azure**
- **Google Cloud Platform (GCP)**
- **VMware**
- **OpenShift**

---

## ✅ Summary of Flow

1. **Engineer** writes `.tf` files and runs Terraform commands.
2. **Terraform Core** processes those files and interacts with:
   - Providers (to communicate with cloud APIs)
   - Provisioners (to run commands)
3. Terraform updates the **State File**.
4. Changes are applied to the **Cloud Service Providers** to match the desired infrastructure.

---

## 📂 Files to Include in GitHub

- `README.md` (this file)
- `a44c8643-4828-4d92-ad82-2e9884c9a54c.png` (Terraform Architecture Image)
