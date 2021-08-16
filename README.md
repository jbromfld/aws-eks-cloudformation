## EKS Cloudformation Template
- This template will build an EKS cluster on AWS.
### The following need to be configured before running
- In workflows `.github/workflows/*`, variable `stackName` should be defined with desired Cloudformation Stack name.
- A user with permissions to both provision resources (EC2, EKS) and manage IAM accounts and permissions needs to define the following Github Secrets in the repo:
```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```
- All other parameters are in the `templates/awscf.yaml` file under the heading `Parameters` and all have default values. The following are necessary to edit:
- `RolePrincipal`: The arn of the user associated with the Github Secrets
- `EksClusterName`: Cluster name
- `EksNodeGroupName`: Node group name
### Process flow
- Running workflow `aws.yaml` will create a default batch user and an EKS cluster and update the aws-auth configmap.
- Outputs will include user key and secret, which should then be given to batch users. These can be accessed through the AWS Cloudformation console (`Outputs` tab), or using the CLI by running:
```
aws cloudformation describe-stacks --stack-name <stack_name> --region <region> --query "Stacks[*].Outputs[*]
```
- After the stack has been created, the parameters can be edited in Github or through the AWS Console
- To delete the stack, run workflow `teardown.yaml`.
