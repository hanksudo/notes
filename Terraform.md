# Terraform note

*example.tf*

```tf
provider "aws" {
  profile    = "default"
  region     = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-2757f631"
  instance_type = "t2.micro"
}
```

```bash
terraform init
terraform plan
terraform apply
terraform show
terraform destroy
terraform version
terraform apply \
  -var-file="secret.tfvars" \
  -var-file="production.tfvars"
```

## Reference

- [Build Infrastructure | Terraform - HashiCorp Learn](https://learn.hashicorp.com/terraform/getting-started/build)
- [Backend Type: remote - Terraform by HashiCorp](https://www.terraform.io/docs/backends/types/remote.html)
