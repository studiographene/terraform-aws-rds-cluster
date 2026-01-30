# v1.3.0

### What:

> feat

`cloudwatch_log_group_kms_key_id` Optional KMS key ARN/ID for CloudWatch Log Group encryption."
removed `storage_type` magnetic type suggestion (deprecated)  .

#### Why:

Cloudwatch log groups for rds gets encrypted with kms key. Helpful for compliances and logs encryption.

#### info:

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
