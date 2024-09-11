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
