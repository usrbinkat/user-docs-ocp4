# [Task 04](../tasks/vpc/) - Setup AWS VPC
### Prerequisite:
  + [Task 03 Write Manifest & Ignition]
--------------------------------------------------------------------------------
### Step 01\. Create Elastic IP
###### Navigate: [AWS Console] > [VPC Service] > [Elastic IPs]
  1. Click: `Allocate new address`
  2. Set IPv4 address pool option: `Amazon pool`
  3. Click: `Allocate`
  4. Copy new Elastic IP Addess
  5. Click: `Close`

TODO: add EIP owner tag {stake holder / ownerName}    
TODO: add EIP owner tag {projectName}    
TODO: https://console.amazonaws-us-gov.com/vpc/home#Addresses    

###### Navigate: [Your Domain Registrar > Your DNS Records Settings]
  6. Create new up stream DNS A Record
>   DNS A Record Table
>
>   | Subdomain Name | Type | Value                |
>   |:--------------:|:----:|:--------------------:|
>   | registry.ocp   | A    | {elastic_ip_address} |

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
```
add VPC endpoints
When running in an air gapped environment the private VPC will 3 three vpc endpoints for the OCP nodes to bootstrap and function. 

com.amazonaws.us-gov-west-1.s3 
com.amazonaws.us-gov-west-1.elasticloadbalancing
com.amazonaws.us-gov-west-1.ec2

The ec2 and ELB endpoints are interface and need to be attached to the private subnet. The security groups should be the master and worker groups attached to each.
```
---------------------------------------------------------------------------------
### Next Steps:
  + [Task 05 Configure Route53 DNS]

--------------------------------------------------------------------------------
[Elastic IPs]:https://console.amazonaws-us-gov.com/vpc/home#Addresses
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[VPC Service]:https://console.amazonaws-us-gov.com/vpc/home
[Subnets]:https://console.amazonaws-us-gov.com/vpc/home#subnets
[Task 01 Prerequisites]:manual/01_Prerequisites.md
[Task 02 Stage Assets]:manual/02_StageAssets.md
[Task 03 Write Manifest & Ignition]:manual/03_ManifestIgnition.md
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
