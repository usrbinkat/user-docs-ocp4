# [Task 06](../tasks/target-groups/) - Setup Target Groups
### Prerequisites:
  + [Task 03 Configure Route53 DNS]
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
> Example shown with vpc_name-aint / 6443
>    
>   | Option            | Value                 |
>   |-------------------|-----------------------|
>   | Target Group Name | `{vpc_name}-aint`     |
>   | Target Type       | IP                    |
>   | Protocol          | TCP                   |
>   | Port              | `6443`                |
>   | VPC               | {match your VPC Name} |
>

  3. Click: `Create`
###### Verify: You should now have 3 new Target Groups for ` *-aint *-aent *-sint `
TODO: add ownerName tags    
---------------------------------------------------------------------------------
### Next Steps:
  + [Task 05 Setup Load Balancers]
--------------------------------------------------------------------------------
[Task 03 Configure Route53 DNS]:../manual/03_Route53DNS.md
[Task 05 Setup Load Balancers]:../manual/05_LoadBalancers.md
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
[Load Balancers]:https://console.amazonaws-us-gov.com/ec2/v2/home#LoadBalancers
