# Azure Databases for MySQL
This creates the following Azure resources:
* Random Password for MySQL Password
* Azure Databases for MySQL Server Instance

## Create vars file or entries in `main.tf` in root of repo
If you add a main.tf in the root of the repo, you can use the following as an example:

```
provider "azurerm" {
  version = "~>2.0"
  features {}
}

### MySQL
module "mysql" {
  source            = "./azure-databases-mysql"
  region            = "eastus2"
  rg_name           = "rg-sandbox"
  mysql_server_name = "travis-mysql-test"
  sku_name          = "B_Gen5_1"
  storage_mb        = "10240"
  backup_days       = "10"
  mysql_geo_backup  = "Disabled"
  mysql_user        = "mysqladmin"
  mysql_version     = "5.7"
  mysql_ssl         = "Disabled"
}
```

**NOTES**:
* Make sure to customize the variables as needed (**NOTE**: Read the `variables.tf` as there are some defaults).
* This will require the `remote-state` module from this repo **OR** at least have your resource and storage group initialized.

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
