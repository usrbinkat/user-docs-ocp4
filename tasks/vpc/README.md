# [Task 02](../tasks/vpc/) - Setup AWS VPC
### Prerequisite:
  + [01 Prerequisites]
--------------------------------------------------------------------------------
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

  7. Click `Save`
  
---------------------------------------------------------------------------------
### Next Steps:
  + [03 Configure Route53 DNS]
--------------------------------------------------------------------------------
[01 Prerequisites]:/manual/01_Prerequisites.md
[03 Configure Route53 DNS]:../manual/03_Route53DNS.md
[AWS Console]:https://console.amazonaws-us-gov.com/console/home?region=us-gov-west-1#
[VPC Service]:https://console.amazonaws-us-gov.com/vpc/home?region=us-gov-west-1
[Subnets]:https://console.amazonaws-us-gov.com/vpc/home?region=us-gov-west-1#subnets:
