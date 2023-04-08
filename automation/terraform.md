# [Terraform](https://developer.hashicorp.com/terraform)

## Getting Started

some commands
```bash
terraform init
terraform plan
terraform apply
```

working with terraform using docker container
```bash
docker run --rm -it -v /path/to/main.tf:/terraform hashicorp/terraform -chdir=/terraform <command>
```
