# [Task 07](../tasks/iam-roles) - Setup IAM Roles
### Prerequisite:
  + [06 Setup Security Groups]
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
### Step 01\. Create IAM Policies `master`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Policies]
  1. Click: `Create Policies`
  2. Select "json" Tab 
  3. Erase default content
  4. Fill in with the following content
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:*",
                "ec2:*",
                "elasticloadbalancing:*"
            ],
            "Resource": "*"
        }
    ]
}
```
  5. Provide `Name` value: `{vpc_name}-master-profile`
  6. Click: `Create Policy`

---------------------------------------------------------------------------------
### Step 02\. Create IAM Policy `worker`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Policies]
  1. Click: `Create Policy`
  2. Select "json" Tab 
  3. Erase default content
  4. Fill in with the following content
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*"
        }
    ]
}
```
  5. Provide `Name` value: `{vpc_name}-worker-profile`
  6. Click: `Create Policy`

---------------------------------------------------------------------------------
### Step 03\. Create IAM Roles `worker` & `master`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Roles]
###### - For both profiles `{vpc_name}-master-profile` & `{vpc_name}-worker-profile` tag per the following
  1. Click: `Create Role`
  2. Select "EC2" under heading "Choose a use case" 
  3. Click: `Next: Permissions`
  4. Filter for term `{vpc_name}-master-profile` & Check left selection box
  5. Click: `Next: Tags`
  6. Fill in with the following Key:Value format
>   Role Tags Table:
>
>   | Role                      | Key  | Value                       |
>   |---------------------------|-----:|:----------------------------|
>   | {vpc_name}-master-profile | Name | `{vpc_name}-master-profile` |
>   | {vpc_name}-worker-profile | Name | `{vpc_name}-worker-profile` |

  7. Click: `Next: Review`
  8. Fill in Role Name (eg: `{vpc_name}-master-profile` )
  9. Click: `Create Role`
    
        
--------------------------------------------------------------------------------
### Step 04\. Create IAM User Role
###### Navigate: [AWS Console] > [EC2] > [IAM] > [Users] > Lower Pannel > Permissions
###### - For each of the 3 IAM User Roles in the `IAM User Role Index` below, do the following:

  1. Filter for your aws administrator user name & click on user
  2. Click: `Add inline policy`
  3. Click: `JSON` Tab
  4. Erase default content
  5. Fill in with associated JSON content
  6. Click: `Create Policy`
  7. Provide `Name` value
  8. Click: `Create Policy`

---------------------------------------------------------------------------------
### IAM User Role Index
    
>   Name: IngressPolicy    
>   JSON:    
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "elasticloadbalancing:DescribeLoadBalancers",
                "route53:ListHostedZones",
                "route53:ChangeResourceRecordSets",
                "tag:GetResources"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```    
>   Name: MachineAPIPolicy    
>   JSON:    
```    
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateTags",
                "ec2:DescribeAvailabilityZones",
                "ec2:DescribeImages",
                "ec2:DescribeInstances",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcs",
                "ec2:RunInstances",
                "ec2:TerminateInstances",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeTargetGroups",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
                "elasticloadbalancing:RegisterTargets",
                "iam:PassRole"
            ],
            "Resource": "*"
        }
    ]
}
```    
>   Name: ImageRegistryPolicy    
>   JSON:    
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:PutBucketTagging",
                "s3:GetBucketTagging",
                "s3:PutBucketPublicAccessBlock",
                "s3:GetBucketPublicAccessBlock",
                "s3:PutEncryptionConfiguration",
                "s3:GetEncryptionConfiguration",
                "s3:PutLifecycleConfiguration",
                "s3:GetLifecycleConfiguration",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:HeadBucket",
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:ListBucketMultipartUploads",
                "s3:AbortMultipartUpload"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```    

---------------------------------------------------------------------------------
### Step 00\. Second

---------------------------------------------------------------------------------
### Next Steps:
  + [08 Create Image Registry]
--------------------------------------------------------------------------------
[06 Setup Security Groups]:../manual/06_SecurityGroups.md
[08 Create Image Registry]:../manual/08_ImageRegistry.md
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[IAM]:https://console.amazonaws-us-gov.com/iam/home
[Users]:https://console.amazonaws-us-gov.com/iam/home#/users
[Roles]:https://console.amazonaws-us-gov.com/iam/home#/roles
[Policies]:https://console.amazonaws-us-gov.com/iam/home#/policies
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
