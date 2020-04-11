# Storage Account and Remote State
This module will initialize a blob container on Azure to use for the Terraform backend state file.

**NOTE**: This should only need to be ran **once** unless you need to make modifications to the resources contained in this module.

**NOTE**: Make sure to review variables.tf for some of the defaults related to the Storage Account

### Create vars file
Create a new file named `vars.tfvars` with the following content:
```text
rg_name = ""
sa_name = ""
container_name = ""
```

**NOTE**: Make sure to fill in the variables with actual data.  There are some restrictions on naming, for example `sa_name` only be numbers / letters between 3-24 characters.

### Terraform Plan
Once you have the variable file filled out, run terraform plan:
```bash
terraform plan -var-file=vars.tfvars
```

### Terraform Apply
If everything looks correct, apply it:
```bash
terraform apply -var-file=vars.tfvars
```

### Note Output
Take note of the outputs and use them as part of the state initialization in the root modules `main.tf` file.
