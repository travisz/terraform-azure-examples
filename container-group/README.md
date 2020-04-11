# Container Groups
This creates a container group with nginx and PHP-FPM containers.

**NOTE**

Some of the resources depend on the `is_private_ip` variable being set to "true". If this is set to true a separate subnet is created for the containers along with a `network_profile`.

If you leave `is_private_ip` set to false, it will not create the separate subnet and `network_profile` and the `ip_address_type` will be `Public`.

### Create vars file or entries in `main.tf` in root of repo
If you add a `main.tf` in the root of the repo, you can use the following as an example:
```
provider "azurerm" {
  version = "~>2.0"
  features {}
}

### Container Group
module "container-group" {
  source                   = "./container-group"
  container_group_name     = "sandbox-containers"
  container_address_prefix = "172.16.100.0/24"
  is_private_ip            = "true"
  dns_name                 = "sandbox-test-containers"
  container_os             = "Linux"
  virtual_network_name     = module.network.virtual_network_name
  region                   = "eastus2"
  rg_name                  = "rg-sandbox"
}
```

**NOTES**:
* Make sure to modify any variables as needed.
* This will require using the `virtual-network` module.
* Multiple containers is currently limited to **Linux** Containers.

If you used the `remote-state` folder in conjunction with the `container-group`, make sure to initialize your backend now!

```
terraform {
 backend "azurerm" {
    resource_group_name  = "<rg_name>"
    storage_account_name = "<sa_name>"
    container_name       = "<container_name>"
    key                  = "<state_file_name>"
  }
}
```

**NOTE**: Make sure to replace the values with the output from `remote-state`.

### Subnet Delegation
**IMPORTANT**: If you are managing your network in another module, make sure that the subnet for the containers has a delegation set:

```
  delegation {
    name = "delegation"

    service_delegation {
      name    = "Microsoft.ContainerInstance/containerGroups"
      actions = ["Microsoft.Network/virtualNetworks/subnets/action"]
    }
  }
```
