# v1.3.0

### What:

> Feature

Performance Insights and Database Insights can now be configured at the cluster level.

- New variable `database_insights_mode` (`standard` or `advanced`), passed to `aws_rds_cluster`.
- When `database_insights_mode` is set, the existing `performance_insights_enabled`, `performance_insights_kms_key_id` and `performance_insights_retention_period` variables are applied to the cluster and are no longer set on `aws_rds_cluster_instance`.
- When `database_insights_mode` is set, Enhanced Monitoring is also applied to the cluster: `rds_monitoring_interval` is set on `aws_rds_cluster` (with the monitoring role only when the interval is greater than 0), and `monitoring_role_arn` is no longer set on `aws_rds_cluster_instance`. This fixes the `MonitoringRoleArn conflicts with cluster level parameter` error.
- When `database_insights_mode` is not set, behaviour is unchanged (Performance Insights and Enhanced Monitoring are set on the cluster instances).

> Note

Minimum AWS provider version raised from `>= 4.23.0` to `>= 5.92.0`. `database_insights_mode` on `aws_rds_cluster` was added in 5.89.0, cluster level `monitoring_interval`/`monitoring_role_arn` in 5.86.0, and 5.92.0 fixes `InvalidParameterCombination: To enable the Advanced mode of Database Insights` on existing clusters and `performance_insights_enabled` not taking effect on cluster create.

Example for Advanced mode:

```hcl
database_insights_mode                = "advanced"
performance_insights_enabled          = true
performance_insights_retention_period = 465
```

#### Why:

Enabling Performance Insights on an RDS instance that is part of a cluster returned an error, and Database Insights Advanced mode can only be enabled on the cluster.

#### info:

https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/rds_cluster#database_insights_mode

# v1.2.2

### What:

> Fix

Error: When applying this module for the first time, resource "aws_security_group_rule" "ingress" for_each not known before apply
Fix: Roll back changes made in `v1.1.3`

> Note

For once when upgrading to this version of module, SG rules will be recreated as with same configuration.

#### Why:

Error: When applying this module for the first time, resource "aws_security_group_rule" "ingress" for_each not known before apply

#### info:

# v1.2.1

### What:

> Fix

AutoScaled instances tagging added to resource "aws_appautoscaling_target".

#### Why:

AutoScaled instance did not have tags until this change.

#### info:

# v1.2.0

### What:

> Enhance

Security group name is suffixed with `rds` for easy identity.

> Breaking change

SG will be recreated with same configs. If you have created SG rules for this RDS SG, you may have to remove those rules as well temporarily and add it back if `Terraform apply` gets stuck at destroying the RDS SG.

#### Why:

Earlier SG name had only Label Context label `id`.

#### info:

# v1.1.3

### What:

> Fix

resource "aws_security_group_rule" "ingress" replaced from count to for_each input.

> Note

For once when upgrading to this version of module, SG rules will be recreated as result of changing to for_each.

#### Why:

count input was replacing and re-adding existing security group ids for any new input for the variable `security_groups` .

#### info:
