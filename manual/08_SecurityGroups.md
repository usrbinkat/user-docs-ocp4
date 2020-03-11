# [Task 08](../tasks/security-groups/) - Setup Security Groups
### Prerequisites:
  + [Task 07 Setup Load Balancers]
--------------------------------------------------------------------------------
### Step 01\. Configure `Security Groups`
###### Navigate: [AWS Console] > [EC2] > Left Panel > Network & Security > [Security Groups]
######  > For both `{vpc_name}-master-sg` & `{vpc_name}-worker-sg` do the following:

  1. Click: `Create Security Group`
  2. Click Tab: `Inbound`
  3. Complete with an allow all rule in the following value format:
> Record Record Name & Port Sets:
>    
>   |                  |  `{vpc_name}-master-sg` | `{vpc_name}-worker-sg`  |
>   |------------------|------------------------:|:------------------------|
>   | Name             | {vpc_name}-master-sg    | {vpc_name}-worker-sg    |
>   | Description      | {vpc_name}-master-sg    | {vpc_name}-worker-sg    |
>   | VPC              | {your_vpc}              | {your_vpc}              |
>   | Rule Type        | All TCP                 | All TCP                 |
>   | Rule Source      | Anywhere                | Anywhere                |
>   | Rule Description | allow ALL from ANYWHERE | allow ALL from ANYWHERE |
>

  4. Click Tab: `Outbound`
  5. Complete with an allow all rule in the following value format:
> Record Record Name & Port Sets:
>    
>   |                  |  `{vpc_name}-master-sg` | `{vpc_name}-worker-sg`  |
>   |------------------|------------------------:|:------------------------|
>   | Name             | {vpc_name}-master-sg    | {vpc_name}-worker-sg    |
>   | Description      | {vpc_name}-master-sg    | {vpc_name}-worker-sg    |
>   | VPC              | {your_vpc}              | {your_vpc}              |
>   | Rule Type        | All TCP                 | All TCP                 |
>   | Rule Source      | Anywhere                | Anywhere                |
>   | Rule Description | allow ALL from ANYWHERE | allow ALL from ANYWHERE |
>

  6. Click: `Create`
###### Verify: You should now have 2 new Security Groups for both of `{vpc_name}-master-sg` & `{vpc_name}-worker-sg`

--------------------------------------------------------------------------------
### Step 02\. Add Target Group `Tags`
######  > For both `{vpc_name}-master-sg` & `{vpc_name}-worker-sg` create the coresponding tag:
> Security Group Tags
>    
>   | Security Group       | Key                     | Value                |
>   |----------------------|------------------------:|:---------------------|
>   | {vpc_name}-master-sg | Name                    | {vpc_name}-master-sg |
>   | {vpc_name}-worker-sg | Name                    | {vpc_name}-worker-sg | 

###### Navigate: [AWS Console] > [EC2] > [Security Groups] > `{vpc_name}-master-sg` > Lower Panel > Tags

  1. Click: `Add/Edit Tags`
  2. Click: `Create Tag`
  3. Complete per the Security Group Tags table
  3. Click: `Save`

###### Navigate: [AWS Console] > [EC2] > [Security Groups] > `{vpc_name}-worker-sg` > Lower Panel > Tags

  1. Click: `Add/Edit Tags`
  2. Click: `Create Tag`
  3. Complete per the Security Group Tags table
  3. Click: `Save`

###### Verify: Both Secrity groups should be tagged per the Security Group Tags table
    
---------------------------------------------------------------------------------
### Next Steps:
  + [Task 09 Setup IAM Roles]
--------------------------------------------------------------------------------
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
[Security Groups]:https://console.amazonaws-us-gov.com/ec2/v2/home#SecurityGroups
--------------------------------------------------------------------------------
[Task 01 Prerequisites]:manual/01_Prerequisites.md
[Task 02 Stage Assets]:manual/02_StageAssets.md
[Task 03 Certificates]:manual/03_Certificates.md
[Task 04 Setup AWS VPC]:manual/04_SetupVPC.md
[Task 05 Configure Route53 DNS]:manual/05_Route53DNS.md
[Task 06 Setup Target Groups]:manual/06_TargetGroups.md
[Task 07 Setup Load Balancers]:../manual/07_LoadBalancer.md
[Task 08 Setup Security Groups]:manual/08_SecurityGroups.md
[Task 09 Setup IAM Roles]:manual/09_IAMRoles.md
[Task 10 Image Registry Instance]:manual/10_ImageRegistryInstance.md
[Task 11 Image Registry Mirror & Services]:manual/11_ImageRegistryServices.md
[Task 12 Build Nodes]:manual/12_BuildNodes.md
[Task 13 Deploy]:manual/13_Deploy.md
