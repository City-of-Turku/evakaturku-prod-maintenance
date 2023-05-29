# Unlocking Terraform lock manually

CircleCI build might fail because of Terraform lock which have been left unopened. 
This usually means that some other task is being done and multiple simultaneous 
tasks are being prevented. If locked state stays on and no tasks are run lock 
needs to be opened manually.

## Error message

Error message on CircleCI looks something like this:

```
|
│ Error: Error acquiring the state lock
│ 
│ Error message: ConditionalCheckFailedException: The conditional request failed
│ Lock Info:
│   ID:        b2bc7060-a339-72e5-e6c4-4e25aec10931
│   Path:      evakaturku-dev-terraform/apps.tfstate
│   Operation: OperationTypeApply
│   Who:       root@2c9ff8a10c0c
│   Version:   1.1.6
│   Created:   2023-05-26 07:24:30.027533826 +0000 UTC
│   Info:
|
```
Important info to look at are the lock ID and environment within path variable

# Unlocking commands

To unlock the lock run there commands in `evakaturku-infra/terraform/apps` path
with `<PROFILE>` and `<LOCK-ID>` info from CircleCI error 

```
AWS_PROFILE=<PROFILE> terraform init -reconfigure -backend-config ../variables/evakaturku-dev-backend.tfvars
AWS_PROFILE=<PROFILE> terraform force-unlock <LOCK-ID>
``` 

In upper examples case the commands would be

```
AWS_PROFILE=evakaturku-dev terraform init -reconfigure -backend-config ../variables/evakaturku-dev-backend.tfvars
AWS_PROFILE=evakaturku-dev terraform force-unlock b2bc7060-a339-72e5-e6c4-4e25aec10931
```
