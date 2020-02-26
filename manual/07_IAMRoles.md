# [Task 07](../tasks/iam-roles) - Setup IAM Roles
### Prerequisite:
  + [06 Setup Security Groups]
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
  5. Provide `Name` value: `{vpc\_name}-master-profile`
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
  5. Provide `Name` value: `{vpc\_name}-worker-profile`
  6. Click: `Create Policy`

---------------------------------------------------------------------------------
### Step 03\. Create IAM Roles `worker` & `master`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Roles]
###### - For both profiles `{vpc\_name}-master-profile` & `{vpc\_name}-worker-profile` tag per the following
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

  7. Fill in with the following content

---------------------------------------------------------------------------------
### Step 03\. Create IAM Profile `worker`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Roles]
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
  5. Provide `Name` value: `{vpc\_name}-worker-profile`
  6. Click: `Create Policy`

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
[Roles]:https://console.amazonaws-us-gov.com/iam/home#/roles
[Policies]:https://console.amazonaws-us-gov.com/iam/home#/policies
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
