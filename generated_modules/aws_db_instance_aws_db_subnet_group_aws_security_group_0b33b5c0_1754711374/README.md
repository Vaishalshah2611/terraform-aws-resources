# RDS Terraform Module

This Terraform module creates an RDS instance in AWS, along with the necessary networking components.

## Features

- Creates a VPC with two subnets across different Availability Zones
- Sets up a DB subnet group
- Creates a security group for the RDS instance
- Provisions an RDS instance with PostgreSQL

## Usage

```hcl
module "rds" {
  source = "./rds-module"

  region      = "ca-central-1"
  project     = "myproject"
  vpc_cidr    = "10.0.0.0/16"
  db_name     = "mydb"
  db_username = "admin"
  db_password = "password123"
}
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| region | AWS region | string | "ca-central-1" | no |
| project | Project name | string | "myproject" | no |
| vpc_cidr | CIDR block for VPC | string | "10.0.0.0/16" | no |
| db_name | Name of the database | string | n/a | yes |
| db_username | Username for the database | string | n/a | yes |
| db_password | Password for the database | string | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| rds_endpoint | The connection endpoint for the RDS instance |
| rds_port | The port the RDS instance is listening on |
| vpc_id | The ID of the VPC |
| subnet_ids | The IDs of the subnets |

## Notes

- The RDS instance is created with `skip_final_snapshot = true`. For production use, you may want to set this to `false` and configure final snapshot settings.
- The security group allows inbound traffic on port 5432 (PostgreSQL) from within the VPC. Adjust as necessary for your security requirements.
- Remember to handle the `db_password` securely. Consider using AWS Secrets Manager or similar service for production deployments.
