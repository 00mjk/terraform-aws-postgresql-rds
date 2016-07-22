# terraform-aws-postgresql-rds

A Terraform module to create an Amazon Web Services (AWS) PostgreSQL Relational Database Server (RDS).

## Usage

```javascript
module "postgresql_rds" {
  source = "github.com/azavea/terraform-aws-postgresql-rds"

  vpc_id = "vpc-20f74844"

  allocated_storage = "32"
  engine_version = "9.4.4"
  instance_type = "db.t2.micro"
  storage_type = "gp2"
  database_identifier = "jl23kj32sdf"
  database_name = "hector"
  database_username = "hector"
  database_password = "secret"
  backup_retention_period = "30"
  backup_window = "04:00-04:30"
  maintenance_window = "sun:04:30-sun:05:30"
  auto_minor_version_upgrade = false
  multi_availability_zone = true
  storage_encrypted = false
  subnet_group = "${aws_db_subnet_group.default.name}"
  parameter_group = "${aws_db_parameter_group.default.name}"

  alarm_cpu_threshold = 75
  alarm_disk_queue_threshold = 10
  alarm_free_disk_threshold = 5000000000
  alarm_free_memory_threshold = 128000000
  alarm_actions = "arn:aws:sns..."

  project = "Something"
  environment = "Staging"
}
```

## Variables

- `project` - Name of project this VPC is meant to house (default: `Unknown`)
- `environment` - Name of environment this VPC is targeting (default: `Unknown`)
- `vpc_id` - ID of VPC meant to house database
- `allocated_storage` - Storage allocated to database instance (default: `32`)
- `engine_version` - Database engine version (default: `9.4.4`)
- `instance_type` - Instance type for database instance (default: `db.t2.micro`)
- `storage_type` - Type of underlying storage for database (default: `gp2`)
- `database_identifier` - Identifier for RDS instance
- `database_name` - Name of database inside storage engine
- `database_username` - Name of user inside storage engine
- `database_password` - Database password inside storage engine
- `backup_retention_period` - Number of days to keep database backups (default:
  `30`)
- `backup_window` - 30 minute time window to reserve for backups (default:
  `04:00-04:30`)
- `maintenance_window` - 60 minute time window to reserve for maintenance
  (default: `sun:04:30-sun:05:30`)
- `auto_minor_version_upgrade` - Minor engine upgrades are applied automatically
 to the DB instance during the maintenance window (default: `true`)
- `multi_availability_zone` - Flag to enable hot standby in another availability
  zone (default: `false`)
- `storage_encrypted` - Flag to enable storage encryption (default: `false`)
- `subnet_group` - Database subnet group
- `parameter_group` - Database engine parameter group (default:
  `default.postgres9.4`)
- `alarm_cpu_threshold` - CPU alarm threshold as a percentage (default: `75`)
- `alarm_disk_queue_threshold` - Disk queue alarm threshold (default: `10`)
- `alarm_free_disk_threshold` - Free disk alarm threshold in bytes (default: `5000000000`)
- `alarm_free_memory_threshold` - Free memory alarm threshold in bytes (default: `128000000`)
- `alarm_actions` - Comma delimited list of ARNs to be notified via CloudWatch

## Outputs

- `id` - The database instance ID
- `database_security_group_id` - Security group ID of the database
- `hostname` - Public DNS name of database instance
- `port` - Port of database instance
- `endpoint` - Public DNS name and port separated by a `:`
