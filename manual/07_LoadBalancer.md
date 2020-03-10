# [Task 07](../tasks/loadbalancer/) - Setup Load Balancers
### Prerequisite:
  + [Task 06 Setup Target Groups]
---------------------------------------------------------------------------------
### Step 01\. Create `External Load Balancer`
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers]

  1. Click: `Create Load Balancer`
  2. Select Type: `Network Load Balancer`
  3. Complete with the following values format:    
> Example shown creating with {az_public}, {vpc_name}, Port: 6443
>
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Name              | {vpc_name}-ext        |
>   | Scheme            | internet facing       |
>   | Protocol          | TCP                   |
>   | Port              | 6443                  |
>   | VPC               | {vpc_name}            |
>   | AZ: {your_az}     | {your_public_zone}    |
>
>   | Tags: Key                        | Value        |
>   |---------------------------------:|:-------------|
>   | kubernetes.io/cluster/{vpc_name} | shared       |
>   | ownerName                        | {owner_name} |
>

  4. Continue click: `Next: Configure Security Settings`
  5. Confirm Security Warning; Click: `Next: Configure Routing`
  6. Fill in Target Group values per the following:
> Example:
>
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Target Group      | Existing target group |
>   | Name              | {vpc_name}-aext       |
>

  7. Click: `Next: Register Targets`
  8. Click: `Next: Review`
  9. Click: `Create`

---------------------------------------------------------------------------------
### Step 02\. Create `Internal Load Balancer`
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers]

  1. Click: `Create Load Balancer`
  2. Select Type: `Network Load Balancer`
  2. Click: `Add Listener`
  2. Click: `Tags` to expand all fields
  2. Click: `Add Tag`
  3. Complete all fields with the following values format:    
> Example shown creating with {az_private}, {vpc_name}, Port: 6443
>
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Name              | {vpc_name}-int        |
>   | Scheme            | internal              |
>   | Protocol          | TCP                   |
>   | Port              | 6443                  |
>   | Port              | 22623                 |
>   | VPC               | {vpc_name}            |
>   | AZ: {your_az}     | {your_private_zone}   |
>
>   | Tags: Key                        | Value        |
>   |---------------------------------:|:-------------|
>   | kubernetes.io/cluster/{vpc_name} | shared       |
>   | ownerName                        | {owner_name} |
>

  4. Continue click: `Next: Configure Security Settings`
  5. Confirm Security Warning; Click: `Next: Configure Routing`
  6. Fill in Target Group values per the following:
> Example:
>   
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Target Group      | Existing target group |
>   | Name              | {vpc_name}-aint       |

  7. Click: `Next: Register Targets`
  8. Click: `Next: Review`
  9. Click: `Create`

---------------------------------------------------------------------------------
### Step 04\. Add Internal Listener `DNS Name Record(s)`
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers] > {vpc_name}-int > Lower Tab `Description`
  1. Copy `DNS Name value` 

###### Navigate: [AWS Console] > [Route 53] > Domain Name `{cluster_domain}.{domain_name}` > `api-int.{cluster_name}.{domain_name}`
  2. Paste {vpc\_name}-int `DNS Name value` into api-int.{cluster\_name}.{domain\_name} > CNAME > Value field
  3. Click: `Save Record Set`

###### Navigate: [AWS Console] > [Route 53] > Domain Name {cluster_domain} > api.{cluster_domain}
  4. Paste {vpc\_name}-int 'DNS Name' value into api.{cluster\_domain} > CNAME > Value field
  5. Click: `Save Record Set`

---------------------------------------------------------------------------------
### Step 05\. Add Upstream DNS Records
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers] > {vpc_name}-ext > Lower Tab 'Description'
  1. Copy `DNS Name value` 

###### Navigate: [Your Domain Registrar > Your DNS Records Settings]
  2. Create the following records values:
> Example:
>   
>   | Record Type       | Name                      | Value                       |
>   |------------------:|:-------------------------:|-----------------------------|
>   | CNAME             | ` *.apps.{cluster_name} ` | (Paste) `DNS Name Value`    |
>

  3. Confirm Resolution: `dig fake.apps.{cluster_domain}`
  - Should resolve to your AWS `DNS Name` value from LB `{vpc_name}-ext`

---------------------------------------------------------------------------------
### Next Steps:
  + [Task 08 Setup Security Groups]    
--------------------------------------------------------------------------------    
    
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[Route 53]:https://console.amazonaws-us-gov.com/route53/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
[Load Balancers]:https://console.amazonaws-us-gov.com/ec2/v2/home#LoadBalancers
--------------------------------------------------------------------------------
[Task 01 Prerequisites]:manual/01_Prerequisites.md
[Task 02 Stage Assets]:manual/02_StageAssets.md
[Task 03 Certificates]:manual/03_Certificates.md
[Task 04 Setup AWS VPC]:manual/04_SetupVPC.md
[Task 05 Configure Route53 DNS]:manual/05_Route53DNS.md
[Task 06 Setup Target Groups]:manual/06_TargetGroups.md
[Task 07 Setup Load Balancers]:manual/07_LoadBalancers.md
[Task 08 Setup Security Groups]:manual/08_SecurityGroups.md
[Task 09 Setup IAM Roles]:manual/09_IAMRoles.md
[Task 10 Image Registry Instance]:manual/10_ImageRegistryInstance.md
[Task 11 Image Registry Mirror & Services]:manual/11_ImageRegistryServices.md
[Task 12 Build Nodes]:manual/12_BuildNodes.md
[Task 13 Deploy]:manual/13_Deploy.md
