# ☁️ Azure Enterprise Modular Infrastructure with Terraform

[![Terraform](https://img.shields.io/badge/Terraform-1.5+-623CE4.svg?logo=terraform&logoColor=white)](https://www.terraform.io/)
[![Azure](https://img.shields.io/badge/Microsoft_Azure-Cloud-0089D6.svg?logo=microsoft-azure&logoColor=white)](https://azure.microsoft.com/)
[![CI/CD](https://img.shields.io/badge/GitHub_Actions-Automated_CI%2FCD-2088FF.svg?logo=github-actions&logoColor=white)](https://github.com/features/actions)
[![Security](https://img.shields.io/badge/Azure_Key_Vault-Secret_Management-0078D4.svg?logo=microsoft-azure&logoColor=white)](https://azure.microsoft.com/services/key-vault/)

Production-ready, highly modular Infrastructure as Code (IaC) repository for provisioning secure, highly available Azure Virtual Machines and network topologies adhering to enterprise security best practices.

---

## 🏛️ Architecture Overview

`
+-----------------------------------------------------------------------------------+
|                               Azure Resource Group                                |
|                                                                                   |
|   +---------------------------------------------------------------------------+   |
|   |                       Virtual Network (VNet)                              |   |
|   |   +-------------------------------------------------------------------+   |   |
|   |   |                          Subnet                                   |   |   |
|   |   |                                                                   |   |   |
|   |   |   +---------------------+               +---------------------+   |   |   |
|   |   |   |     NIC (eth0)      |               |  Public IP (Static) |   |   |   |
|   |   |   +----------+----------+               +----------+----------+   |   |   |
|   |   |              |                                     |              |   |   |
|   |   |              +------------------+------------------+              |   |   |
|   |   |                                 |                                 |   |   |
|   |   |                     +-----------v-----------+                     |   |   |
|   |   |                     |     Virtual Machine   |                     |   |   |
|   |   |                     |     (Linux / NGINX)   |                     |   |   |
|   |   |                     +-----------^-----------+                     |   |   |
|   |   +---------------------------------|---------------------------------+   |   |
|   +-------------------------------------|-------------------------------------+   |
|                                         |                                         |
|   +--------------------------+          |           +-------------------------+   |
|   |  Network Security Group  +----------+           |     Azure Key Vault     |   |
|   |   (SSH 22, HTTP 80/443)  |                      |   (Admin SSH Credentials|   |
|   +--------------------------+                      +-------------------------+   |
+-----------------------------------------------------------------------------------+
`

---

## 📂 Repository Structure

`
azure-terraform-modular-infra/
├── .github/
│   └── workflow/
│       └── terraform-pipeline.yaml     # Automated GitHub Actions deployment pipeline
└── VM-For_each-Data_block-key_vault-terraform_nginx/
    ├── child-module/                   # Reusable, independent infrastructure modules
    │   ├── resource-group/             # Azure Resource Group provisioning
    │   ├── virtual-network/            # VNet configuration
    │   ├── subnet/                     # Subnet segmentation & routing
    │   ├── public-ip/                  # Dynamic/Static Public IP allocation
    │   ├── network-interface-card/     # NIC binding with Private & Public IP
    │   ├── NSG/                        # Network Security Group inbound/outbound rules
    │   └── virtual-machine/            # Linux VM with Key Vault data lookup & NGINX
    └── env/
        └── prod/                       # Production environment configuration
            ├── prod-provider.tf        # HashiCorp azurerm provider setup
            ├── prod-main.tf            # Root module wiring child modules together
            ├── prod-variable.tf        # Variable declarations & type validations
            └── terraform.tfvars        # Environment-specific configuration values
`

---

## 🌟 Key Engineering Features

- **Decoupled Modular Design**: Standardized child modules for zero configuration drift and high reusability across Dev, UAT, and Prod environments.
- **Dynamic Resource Iteration**: Built with Terraform or_each and count meta-arguments for flexible multi-VM and multi-subnet deployments.
- **Zero Hardcoded Secrets**: Secure credentials management retrieving VM admin secrets dynamically from **Azure Key Vault** using Terraform data blocks.
- **GitOps CI/CD Integration**: Fully automated GitHub Actions workflow executing 	erraform init, plan, and pply with Azure Service Principal authentication.

---

## 🚀 Deployment Instructions

### Prerequisites
1. [Terraform CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli) (v1.5+)
2. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) logged in (z login)
3. Pre-existing Azure Key Vault containing the VM admin password secret

### Local Execution

`ash
# 1. Clone repository
git clone https://github.com/Mit-Yadav/azure-terraform-modular-infra.git
cd azure-terraform-modular-infra/VM-For_each-Data_block-key_vault-terraform_nginx/env/prod

# 2. Initialize Terraform providers and backend
terraform init

# 3. Validate syntax and format
terraform fmt -check
terraform validate

# 4. Generate and inspect execution plan
terraform plan -var-file="terraform.tfvars" -out=tfplan

# 5. Apply infrastructure safely
terraform apply tfplan
`

---

## 🔒 Security Best Practices Implemented
- Least privilege Network Security Group (NSG) rules restrict access strictly to specified ports.
- Sensitive credentials never stored in version control; managed centrally via Azure Key Vault.
- Remote state locking and encryption in transit and at rest.

---
**Maintained by [Mithilesh Yadav](https://github.com/Mit-Yadav)** | Enterprise DevOps & SRE Engineer