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
13. `terraform plan` and `terraform apply` in the background does a refresh.
14. Directly doing `terrafom refresh` is a bad idea as it will update the state file without any confirmation prompt. If there is any change in the credentials or region of the provider, terraform will assume all the resources are deleted and it will lead to a wrong update on the state file. This is the reason it also deprecated.
15. Instead of `terraform refresh`, we should use `terraform plan -refresh-only` or `terraform apply -refresh-only`. Before commiting the changes to the state file, there will be a confirmation prompt and the list of changes can be seen.
16. Terraform has planning modes:
    i. Destroy: It will destroy all remote objects that currently exists using `-destroy` flag.
    ii. Refresh-only: It will update the terraform state with that of the current remote state using `-refresh-only` flag.
17. Terraform has planning options:
    i. `-refresh=false`: Disables the default behavior of synchronizing the Terraform state with remote objects before checking for configuration changes.
    ii. `-replace=<address/object>`: Instructs Terraform to plan to replace the resource instance with the given address. This is basically a forced deletion and creation.
    iii. `-target=<address/object>`: Instructs Terraform to focus its planning efforts only on resource instances which match the given address and on any objects that those instances depend on.
18. Resources can be cross-referenced using address from resource type, local name and attribute. \
Example: `public_ip = [aws_eip.myeip.public_ip]`
19. `terraform plan -refresh-only` will fetch the latest remote state and showcase the changes. It won't change the state file.
20. `terraform apply -refresh-only` if approved, will update the state file with the latest remote state. It won't make any infrastructure changes.
21. Place variables in _terraform.tfvars_ file.
22. `count` helps in creating copies of the resources. `count.index` gets the loop index. It can also be used for conditional creation of resources. \
Example: `count = var.env == "dev" ? 1:0`
23. Terraform doesn't support user-defined functions.
24. `terraform console` opens up terraform console to test and run terraform commands.
25. Data sources allow data to be fetched or computed for using in terraform. Data sources allow Terraform to use information defined outside of Terraform \
Example: 
```tf
data "aws_ami" "web" {
  most_recent = true

  owners = ["self"] # self for self created AMIs
  tags = {
    Name   = "web-server"
    Tested = "true"
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.web.id
  instance_type = "t1.micro"
}

```
26. Terraform log level can be changed through `TF_LOG` environment variable and possible options are TRACE, DEBUG, INFO, WARN, ERROR
27. Setting `TF_LOG_PATH` to a file path will store the logs in that file and won't print out the logs.
28. `terraform fmt` takes care of formatting of the terraform files in the same directory.
29. `terraform validate` checks and validates terraform files in the directory. These include checks for unsupported fields, undeclared variables etc.
30. `terraform plan` already does the validation.
31. Terraform files (.tf or .tf.json) are loaded in alphabetical order.
32. `terraform plan -out <file>` can be used to store the terraform plan and `terraform apply <file>` can be used to execute terraform with the plan file.
33. Dynamic blocks can be used to dynamically construct repeatable nested blocks. They are supported inside data, resources, provider, provisioner. \
Example:
```tf
locals {
  inbound_ports = [80, 443]
}

dynamic "ingress" {
  for_each = local.inbound_ports
  content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = [ "0.0.0.0/0" ]
  }
}
```
In the above example, ingress acts as a default iterator.

34. `terraform taint` was there in terraform version 0.15.2 and earlier. It used to do something similar to `terraform apply -replace=<ADDRESS>`
35. Splat (*) will get list of all attribute values. \
Example: `value = aws_iam_user.<name>[*].arn` will give the arn for all users.
36. `terraform graph` gets a visual representation of configurations or execution plan in DOT format, which can be converted to image.
37. `terraform output` can be used to extract value of an output variable from the state file.
38. For larger infrastructure, `terraform plan` will make a lot of API calls to get refresh the state. Hence, you can either run `terraform plan -refresh=false` or keep the configuration files in separate flders so that terraform doesn't refresh all at once.
39. There is another way for the above. Terraform target can be specified to refresh specific resources.
40. `zipmap` function of terraform creates a map from a list of keys ad list of values. Example: [a,b] and [1,2] to {a=1,b=2}
41. Comments in terraform by # (single line), // (single line), /* */ (multi line)
42. Meta arguments allow customized behavior on per resource basis like depends_on, count, for_each, lifecycle, provider. Examples:
  - `lifecycle`:
    * create_before_destroy: first new replacement is created and then existing is deleted
    * prevent_destroy: cannot destroy using `destroy` command, useful for DBs
    * replace_triggered_by: replaces esources if any of the referenced resources change
    * ignore_changes: ignores the changes mentioned, say tags if changed it will ignore it, eg. `ignore_changes = [tags]`
  - `count`: loop used for resources where fields are almost identical
  - `for_each`: loop used for resources where fields are distinct and needs customized names
43. If the order changes with variables associated with `count`, then it would lead to issues with the resources. Example, if the count is applied on a list [a,b,c] and it gets changed to [z,a,b,c], it will re-create the 3 existing resources due to change in order.
44. `toset()` fucntion helps to convert objects into set, which are unordered but unique. Example, `toset(['a','b','c','a'])` -> (['a','b','c']), may not be in that order but unique
45. In `for_each` resource addresses are stored as elements of a map. Example:
```tf
resource "azurerm_resource_group" "rg" {
  for_each = tomap({
    a_group       = "eastus"
    b_group       = "westus2"
  })
  name     = each.key # a_group, b_group
  location = each.value # eastus, westus2
}

```
46. DRY -> Don't Repeat Yourself. This is the concept for modules in terraform, where a single skeleton can be re-used. It can be a centralized source or shared library type. To install a module, do `terraform init` Example:
```tf
module <module-name> {
  source = "<module-source>"
  ...
}
```
47. We can override the variables declared at the module level by providing our own variables and values.
48. Terraform workspaces provide separate environment to work and assign different set of configuration.
49. Terraform will maintain separate state files in _terrafom.tfstate.d_ directory for each workspace.
50. If created workspaces are **dev** & **prod**, we can use them like -
```tf
variable "instance_type" {
  type    = map
  default = {
    default = "t2.nano"
    dev     = "t2.micro"
    prod    = "t2.large"
  }
}
resource "aws_instance" "demo_ec2" {
  ...
  instance_type = lookup(var.instance_type,terrafom.workspace)
}
```
51. Terraform module sources can be local paths, TF registry, GitHub, BitBucket, HTTPS URL, S3 buckets etc.
52. Backends are places where TF state is stored. By default it is local, but there are remote backends too, which can be stored in S3 Buckets, Azure Blob Storage etc. Example:
```tf
terrafom {
  backend "s3" {
    bucket = "bucket-name"
    key    = "path/to/the/tfstate/file"
    region = "region"
  }
}
```
53. Whenever write operations are being done on the tfstate file, terraform locks the state file, so that there are no parallel write operations on the tfstate file at the same time. It will show error acquiring the state lock.
54. Locking is identified by _.terrafom.tfstate.lock.info_ file. Not all backends can support state file locking.
55. Terraform locking can be undone by `terraform force-unlock`
56. Terraform state:
  - `terraform state list`: list resources within a terraform state
  - `terraform state show <resourceAddress>`: shows attributes of a single resource in a state
  - `terraform state pull`: pull the terrafor state file (even from remote backend)
  - `terraform state rm <resourceAddress>`: remove items from state, stop managing it by terraform
  - `terraform state mv <oldResourceAddress> <newResourceAddress>`: used to modify address of resources, without making terraform destry & re-create it. Example, `aws_iam_user.dev` -> `aws_iam_user.prod`, otherwise would destroy and re-create the user resource.
  - `terraform state replace-provider <currentProvider> <newProvider>`: replaces the provider
