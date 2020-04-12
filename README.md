# Azure Terraform Examples

A repository with a collection of Azure Terraform scripts I've written.

## Modules
* **container-group** - Creates an Azure Container Group with an nginx and a PHP-FPM container.  Has logic to create a private subnet if the `is_private_ip` variable is set.
* **load-balancer** - Create a Load Balancer within Azure as well as a public IP to assign to the load balancer, a NAT rule and backend address pool.
* **remote-state** - For Creating the RG, Storage Account and Container for Terraform State Storage.  **IMPORTANT**: Only run this **once** then reference the output as needed in the root or other modules.
* **ubuntu-virtual-machine** - Creates an Ubuntu Virtual Machine. See the README in the folder for additional information.
* **virtual-network** - Creates a virtual network within Azure. Creates two subnets by default. Make sure to update variables in the root modules `main.tf` file accordingly.
