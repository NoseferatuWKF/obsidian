# [Terraform](https://developer.hashicorp.com/terraform)

>go based

## Getting Started

some commands
```bash
terraform init
terraform validate
terraform plan
terraform apply # use --auto-approve for unattended
terraform destroy # use --auto-approve for unattended

# targeting
terraform plan -target "module.<module-name>"
terraform plan -target "<resource-name>.<tag>"
```

working with terraform using docker container
```bash
docker run --rm -it -v /path/to/main.tf:/terraform hashicorp/terraform -chdir=/terraform <command>
```

variable
```tf
variable "foo" {
	type = string
	default = ""
}
```

locals
```tf
locals = {
	map = {
		foo = {
			key1 = "value"
		},
		bar = {
			 key1 = "another value"
		}
	}
}

# to be used externally
l = lookup(local, "map", {})
```

tfvars
```tf
# file must be same as folder name?
```

dynamic block
```tf
dynamic "resource_block" {
	for_each = var.some_value
	content {
		 key: each.value.field
		 key2: each.value.field2
	}
}
```