# v1.1.3

### What:

> Fix

resource "aws_security_group_rule" "ingress" replaced from count to for_each input.

> Note



#### Why:

count input was replacing and re-adding existing security group ids for any new input for the variable `security_groups` .

#### info:


