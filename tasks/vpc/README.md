# [Task 04](../tasks/vpc/) - Setup AWS VPC
### Prerequisite:
  + [Task 03 Certificates]
--------------------------------------------------------------------------------
### Step 01\. Create VPC
TODO: create EIP    
TODO: add EIP owner tag {stake holder / ownerName}    
TODO: add EIP owner tag {projectName}    
TODO: https://console.amazonaws-us-gov.com/vpc/home#Addresses    
### Step 01\. Create VPC
###### Navigate: [AWS Console] > [VPC Service] > Start VPC Wizard
  1. Select VPC with Public and Private subnets
  2. Configure with the following value types:
> Example values:
>
>   | Option               | Value                 |
>   |---------------------:|:----------------------|
>   | IPv4                 | '10.0.0.0/16'         |
>   | IPv6                 | No IPv6 CIDR Block    |
>   | VPC Name             | '{vpc_name}'          |
>   | IP Allocation        | Select from available |
>   | Availability Zone    | 'us-gov-west-1a'      |
>   | Enable DNS Hostnames | 'yes'                 |

  3. Click `Create VPC` to complete

---------------------------------------------------------------------------------
### Step 02\. Configure Tagging on Public & Private Subnets
###### Navigate: [AWS Console] > [VPC Service] > [Subnets] 
  1. Navigate: Public Subnet > Tags
  2. Click `Add/Edit Tags`
  3. Create the following    
> Example values:
>
>   | Key                             | Value                             |
>   |:--------------------------------|----------------------------------:|
>   |Name                             | {vpc_name}-public-us-gov-west-1a  |
>   |kubernetes.io/cluster/{vpc_name} | owned                             |
TODO: add owner tag {stake holder / ownerName}

  3. Click `Save`
  4. Navigate: Private Subnet > Tags
  5. Click `Add/Edit Tags`
  6. Create the following    
> Example values:
>
>   | Key                             | Value                             |
>   |:--------------------------------|----------------------------------:|
>   |Name                             | {vpc_name}-private-us-gov-west-1a |
>   |kubernetes.io/cluster/{vpc_name} | owned                             |
TODO: add owner tag {stake holder / ownerName}

  7. Click `Save`
  
---------------------------------------------------------------------------------
### Next Steps:
  + [Task 05 Configure Route53 DNS]

--------------------------------------------------------------------------------
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[VPC Service]:https://console.amazonaws-us-gov.com/vpc/home
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
