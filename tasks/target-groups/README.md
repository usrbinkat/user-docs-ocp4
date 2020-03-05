# [Task 04](../tasks/security-groups/) - Setup Target Groups
### Prerequisites:
  + [Task 05 Setup Load Balancers]
--------------------------------------------------------------------------------
### Step 01\. Configure `Security Groups`
###### Navigate: [AWS Console] > [EC2] > Left Panel > Network & Security > [Security Groups]
######  > For both `{vpc_name}-master-sg` & `{vpc_name}-worker-sg` do the following:

  1. Click: `Create Security Group`
  2. Complete with an allow all rule in the following value format:
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

  3. Click: `Create`
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
  + [Task 07 Setup IAM Roles]
--------------------------------------------------------------------------------
[Task 05 Setup Load Balancers]:../manual/05_LoadBalancers.md
[Task 07 Setup IAM Roles]:../manual/07_IAMRoles.md
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
[Security Groups]:https://console.amazonaws-us-gov.com/ec2/v2/home#SecurityGroups
