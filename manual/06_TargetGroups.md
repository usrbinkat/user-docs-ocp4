# [Task 06](../tasks/target-groups/) - Setup Target Groups
### Prerequisites:
  + [Task 05 Configure Route53 DNS]
--------------------------------------------------------------------------------
### Step 01\. Configure `Target Groups`
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Target Groups]
######  > For each of the 3 record name & port sets do the following:
> Record Record Name & Port Sets:
>    
>   | Record Name     | Port  |
>   |-----------------|-------|
>   | {vpc_name}-aext |  6443 |
>   | {vpc_name}-aint |  6443 |
>   | {vpc_name}-sint | 22623 |
>

  1. Click: `Create Target Group`
  2. Complete with the following values format:
> Example Target Group Table:
>    
>   | Option            | Value                 |
>   |-------------------|-----------------------|
>   | Target Group Name | `{vpc_name}-aint`     |
>   | Target Type       | IP                    |
>   | Protocol          | TCP                   |
>   | Port              | `6443`                |
>   | VPC               | {match your VPC Name} |
>    
> Example Health Check Settings
>
>   | Option   | Value |
>   |----------|-------|
>   | Protocol | TCP   |
>

  3. Click: `Create`
###### Verify: You should now have 3 new Target Groups for ` *-aext *-aint *-sint `
TODO: add ownerName tags    
---------------------------------------------------------------------------------
### Next Steps:
  + [Task 07 Setup Load Balancers]
--------------------------------------------------------------------------------
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
[Task 01 Prerequisites]:manual/01_Prerequisites.md
[Task 02 Stage Assets]:manual/02_StageAssets.md
[Task 03 Certificates]:manual/03_Certificates.md
[Task 04 Setup AWS VPC]:manual/04_SetupVPC.md
[Task 05 Configure Route53 DNS]:manual/05_Route53DNS.md
[Task 06 Setup Target Groups]:manual/06_TargetGroups.md
[Task 07 Setup Load Balancers]:manual/07_LoadBalancer.md
[Task 08 Setup Security Groups]:manual/08_SecurityGroups.md
[Task 09 Setup IAM Roles]:manual/09_IAMRoles.md
[Task 10 Image Registry Instance]:manual/10_ImageRegistryInstance.md
[Task 11 Image Registry Mirror & Services]:manual/11_ImageRegistryServices.md
[Task 12 Build Nodes]:manual/12_BuildNodes.md
[Task 13 Deploy]:manual/13_Deploy.md
