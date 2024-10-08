# Facts

1. If you have a variables.tf file and the variable block in the main.tf file for the same variable, then you will get a duplicate variable error.
2. Variables & locals can be combined like -

```tf
variable "is_production" {
 description = "Is this a production environment?"
 type = bool
 default = false
}
locals {
 region = var.is_production ? "us-east-1" : "us-west-2"
 instance_size = var.is_production ? "t3.large" : var.instance_type
 tags = {
 Name = "SampleInstance"
 Owner = "TeamA"
 Env = var.is_production ? "production" : "development"
 }
}
```

3. Differece between terafform variables & locals:
![image](https://github.com/user-attachments/assets/80034d5b-6e60-4ba9-9a41-7c7f6727c6c2)

4. The order of precedence for variable sources for terraform, with first being the least:
  i. Environment variables, which start with **TF_VAR_** (Example: `export TF_VAR_bucket_name="bucket"`)
  ii. Default variables, which can be declared in the variables.tf
  iii. The terraform.tfvars file, if present
  iv. The terraform.tfvars.json file, if present.
  v. Any *.auto.tfvars or *.auto.tfvars.json files, processed in lexical order of their filenames
  vi. Any -var and -var-file options on the command line, in the order they are provided.
5. Provider in Terraform is just a plugin that helps manage an external API. Provider tiers are: \
    i. Official: Owned and maintained by Terraform, like AWS, Azure, GCP
    ii. Partners: Maintained by partners, like OCI, Alibaba
    iii. Community: Maintained by individual contributers
6. When `terraform init` is done the mentioned provider plugins are downloaded and stored in the _.terraform_ directory.
7. Resource blocks describe one or more infrastructure objects. They declare resources of a 'given type' and with a 'local name'.
8. Namespaces are ways to recognize a provider. Example: `/hashicorp/aws` or `/oracle/oci`
9. In case of partners/communitity tiers, `required_providers` must be declared. This block also helps in customizing the versions to be used. For example, `~>2.0.0` means any version in the range of 2.0.x
10. `terraform plan` shows the changes and `terraform apply` applies the changes.
11. State file stores the current configurations. Terraform is going to deploy the changes according to the **desired state** as mentioned in the configuration files.
12. **lock file** prevents changes to the providers. If any changes are required then either delete the lock file and do `terraform init` again or do an upgrade by `terraform init -upgrade` to get the new plugins accordingly.
13. 