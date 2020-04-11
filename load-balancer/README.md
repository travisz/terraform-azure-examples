# Azure Load Balancer
This creates a public IP resource, load balancer, NAT rule and backend address pool within Azure.

### Create vars file or entries in main.tf in root of repo
If you add a main.tf in the root of the repo, you can use the following as an example:
```
provider "azurerm" {
  version = "~>2.0"
  features {}
}

### Load Balancer
module "lb" {
  source                    = "./load-balancer"
  lb_name                   = "sandbox-lb"
  region                    = "eastus2"
  rg_name                   = "rg-sandbox"
  backend_address_pool_name = "sandbox-be-address-pool"
  nat_frontend_port         = "80"
  nat_backend_port          = "80"
  nat_protocol              = "Tcp"
}
```

**NOTE**: Make sure to modify any variables as needed.

If you used the `remote-state` folder in conjunction with the `load-balancer`, make sure to initialize your backend now!

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
