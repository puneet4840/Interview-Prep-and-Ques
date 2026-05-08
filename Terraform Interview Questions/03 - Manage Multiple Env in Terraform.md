# How do you manage multiple environments in terraform

Terraform mein multiple environments jaise Dev, Qa, Prod ko manage karne ke liye hum teeno envs ki configuration ko alag-alag folder mein rakhte hain. Aur in environments ki state file bhi alag hoti hain. 

Resource ke liye reusable modules create karte hain for particular environment ke liye un modules ko call karte hain. Jis bhi environment mein work karna ho to usi folder mein jake terraform ki commands ko run karte hain.

Interviewer ka question “How do you manage multiple environments in Terraform?” bahut common aur important hai.

<br>

### Sabse Pehle Samjho Problem Kya Hai

Suppose tumhari company mein ye environments hain:
- Dev
- QA
- Prod

Aur sabmein same infrastructure chahiye:
- Resource Group
- Virtual Network
- Subnet
- Linux VM
- Storage Account

Difference sirf:
- size
- naming
- scaling
- secrets
- region
- counts

mein hota hai.

<br>

### Production-Level Best Practice

Industry mein generally ye architecture use hota hai:
```
terraform-azure/
│
├── modules/
│   ├── resource-group/
│   ├── network/
│   └── vm/
│
├── environments/
│   ├── dev/
│   ├── qa/
│   └── prod/
│
└── backend/
```

Is architecture mein envrionments ki config ko alag-alag folder mein rakhte hain. Jisme sabhi environment ki alag ```terraform.tfstate``` file hoti hai.

```terraform apply``` command bhi alag-alag teeno folders mein jake run karni hoti hai.

<br>
<br>

## real Azure production-style example

<br>

### Step 1 — Azure Backend Setup

Production mein Terraform state local machine par nahi rakhte.

Azure mein generally Storage Account ke ander Blob Container mein ```.tfstate``` store karte hain.

<br>

**Azure Backend Resources**:

```.tfstate``` file ko backend mein yani azure ke blob storage mein store karne ke liye, Terraform script likhne se pehle hi storage account mein blob container create karna hota hai.

Pehle manually ya bootstrap Terraform se create karte hain:
- Resource Group
- Storage Account
- Blob Container

**Example**:

Resource Group
```
rg-terraform-state
```
Storage Account
```
tfstateprod001
```
Blob Container
```
tfstate
```

**Terraform Backend Config**:
```
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-state"
    storage_account_name = "tfstateprod001"
    container_name       = "tfstate"
    key                  = "dev.terraform.tfstate"
  }
}
```

**Important**:

Har environment ka alag state file rahegi:
```
dev.terraform.tfstate
qa.terraform.tfstate
prod.terraform.tfstate
```

Why?

Agar prod mein issue aaye tp dev impact nahi hoga. Ye enterprise-grade isolation hai.

<br>
<br>

### Step 2 — Create Reusable Modules 

Ab reusable modules banayenge.

**Module 1 — Resource Group**:

Structure
```
modules/resource-group/
│
├── main.tf
├── variables.tf
└── outputs.tf
```

main.tf
```
resource "azurerm_resource_group" "rg" {
  name     = var.rg_name
  location = var.location
}
```

variables.tf
```
variable "rg_name" {}
variable "location" {}
```

<br>

**Module 2 — Network Module**:

main.tf
```
resource "azurerm_virtual_network" "vnet" {
  name                = var.vnet_name
  address_space       = ["10.0.0.0/16"]
  location            = var.location
  resource_group_name = var.rg_name
}

resource "azurerm_subnet" "subnet" {
  name                 = var.subnet_name
  resource_group_name  = var.rg_name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.1.0/24"]
}
```

<br>

**Module 3 — VM Module**:

main.tf:
```
resource "azurerm_linux_virtual_machine" "vm" {
  name                = var.vm_name
  resource_group_name = var.rg_name
  location            = var.location
  size                = var.vm_size

  admin_username = "azureuser"

  network_interface_ids = [
    var.nic_id
  ]

  admin_ssh_key {
    username   = "azureuser"
    public_key = file("~/.ssh/id_rsa.pub")
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "0001-com-ubuntu-server-jammy"
    sku       = "22_04-lts"
    version   = "latest"
  }
}
```

<br>
<br>

### Step 3 — Environment Folders

Ab har environment ka folder.

**Dev Environment**:
```
environments/dev/
│
├── main.tf
├── variables.tf
├── dev.tfvars
└── backend.tf
```

Dev main.tf
```
provider "azurerm" {
  features {}
}

module "rg" {
  source   = "../../modules/resource-group"
  rg_name  = var.rg_name
  location = var.location
}

module "network" {
  source      = "../../modules/network"
  rg_name     = var.rg_name
  location    = var.location
  vnet_name   = var.vnet_name
  subnet_name = var.subnet_name
}
```

Dev variables.tf
```
variable "rg_name" {}
variable "location" {}
variable "vnet_name" {}
variable "subnet_name" {}
```

dev.tfvars
```
rg_name    = "rg-dev"
location   = "Central India"
vnet_name  = "dev-vnet"
subnet_name = "dev-subnet"
```

<br>
<br>

### Step 4 — Prod Environment

Ab prod ke values alag hongi.

Ese hi same struture ke saath files banani hain, modules use karne hain but unke variables ke ander values alag hongi. Ye hi difference rahega teeno envs mein.

prod.tfvars
```
rg_name    = "rg-prod"
location   = "Central India"
vnet_name  = "prod-vnet"
subnet_name = "prod-subnet"
```

**Difference Samjho**

Infrastructure code SAME hai. Bas values different hain. Ye hi real enterprise Terraform philosophy hai.

<br>
<br>

### Step 5 — Initialize Terraform

Dev folder ke andar:
```
terraform init
```
Ye:
- provider download karega
- backend connect karega
- state initialize karega

<br>
<br>

### Step 6 — Plan

```
terraform plan -var-file=dev.tfvars
```

<br>
<br>

### Step 7 — Apply

```
terraform apply -var-file=dev.tfvars
```

**Result**:

Azure mein create ho jayega:
- rg-dev
- dev-vnet
- subnet
- vm

<br>
<br>

### Step 8 — QA & Prod

Bas folder change karo:
```
cd environments/prod
```
Then:
```
terraform apply -var-file=prod.tfvars
```

<br>
<br>

### Important Enterprise Concept

**Separate Backend Per Environment**

Dev backend:
```
key = "dev.tfstate"
```

Prod backend:
```
key = "prod.tfstate"
```

**Why Critical?**

Agar kisi ne:
```
terraform destroy
```

Dev mein chalaya, toh prod safe rahega.

<br>
<br>

### Step 10 — Secrets Management

Kabhi bhi:
```
password = "admin123"
```
mat likho.

Use:
- Azure Key Vault

**Example**:
```
data "azurerm_key_vault_secret" "vm_password" {
  name         = "vm-password"
  key_vault_id = var.keyvault_id
}
```

<br>

Ese hi terraform se multiple environments manage kiye jaate hain.

