# [Task 07](../tasks/iam-roles) - Setup IAM Roles
### Prerequisite:
  + [06 Setup Security Groups]
--------------------------------------------------------------------------------
### Step 00\. First
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Roles]
  1. Click: `Create Role`
  2. Select "json" Tab 
  3. Erase default content
  4. Fill in with the following content
    ```
    Add this policy and click review
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
> Example
>
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Name              | {vpc_name}-ext        |

---------------------------------------------------------------------------------
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Policy]
---------------------------------------------------------------------------------
### Step 00\. Second
###### Navigate: [AWS Console] > [EC2] 

---------------------------------------------------------------------------------
### Next Steps:
  + [08 Create Image Registry]
--------------------------------------------------------------------------------
[06 Setup Security Groups]:../manual/06_SecurityGroups.md
[08 Create Image Registry]:../manual/08_ImageRegistry.md
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[IAM]:https://console.amazonaws-us-gov.com/iam/home
[Roles]:https://console.amazonaws-us-gov.com/iam/home#/roles
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
