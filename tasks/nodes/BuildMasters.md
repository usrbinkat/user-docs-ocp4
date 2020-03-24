# [Task 13](../tasks/nodes/) - Build Master Nodes
### Prerequisite:
  + [Task 12 Build Bootstrap Node]
---------------------------------------------------------------------------------
### Step 01\. Create Master RHCOS Instances
###### Navigate: [AWS Console] > [EC2] > [AMIs]
  1. Select: `rhcos` image
  2. Click: `Launch`
  3. Select Instance Type: minimum of `t2.xlarge`
  4. Click: `Next: Configure Instance Details`
  5. Set values as per below

>   Master Nodes AMI `Configure Instance` Options Table
>
>   |                |                      |
>   |:--------------:|:--------------------:|
>   | Instance Type  | t2.xlarge            |
>   | Network        | {vpc_name}           |
>   | Subnet         | {vpc_name}-private-* |

  6. Add 'Configure Instance' `Advanced Details` User data        
  - Copy/Paste the following user data        
  - Substitute '{CLUSTER\_DOMAIN}' for your cluster domain name (eg: cluster.openshift.com)
```
{"ignition":{"config":{"append":[{"source":"http://registry.{CLUSTER_DOMAIN}/master.ign","verification":{}}]},"security":{},"timeouts":{},"version":"2.2.0"},"networkd":{},"passwd":{},"storage":{},"systemd":{}}
```

  7. Click: `Next: Add Storage`
  8. Configure with following minimums
>   Image Regristy AMI `Add Storage` Options Table
>
>   | Option         | Value (Minimum)     |
>   |:--------------:|:-------------------:|
>   | Device         | /dev/sda1           |
>   | Size           | 30 GiB              |

  9. Click: `Next: Add Tags`
 10. Click: `Add Tag`
>   Image Registry AMI `Add Tags` Table
>   - (where `n` is master node number {1,2,3})
>
>   | Key                              | Value                     |
>   |:--------------------------------:|:-------------------------:|
>   | Name                             | {vpc_name}-master-{n}     |
>   | kubernetes.io/cluster/{vpc_name} | owned                     |

 11. Click: `Select an Existing Security Group`
 12. Click: `Existing Security Group`
 13. Select: `{vpc_name}-master-sg`
 14. Click: `Review and Launch`
 15. Click: `Launch`
 16. Select Option: Proceed without a key pair
 17. Select acknowledge & continue check box
 18. Click: `Launch Instances`

---------------------------------------------------------------------------------
### Step 02\. Add Node to Target Groups
 01. Copy 'Private IP'

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

  1. Click: `add filter` and filter by your {vpc\_name}
  2. Select a Target Group
  3. Navigate: Lower Pannel > Targets
  4. Click: `edit` 
  5. Click: Top Bar ` + ` plus symbol to register new targets
  6. Complete with the following values format:
> Example Register Targets Table:
>    
>   | Option  | Value                       |
>   |---------|-----------------------------|
>   | Network | {vpc_subnet}                |
>   | IP      | {master_node_ip}            |
>   | Port    | (default target group port) |
>   | VPC     | {match your VPC Name}       |
>    

  7. Click: `Add to list` 
  8. Click: `Register` 
  9. Click: ` < ` left arrow symbol to return to target group menu
 10. Repeat for all 3 target groups

---------------------------------------------------------------------------------
### Next Steps:
  + [Task 13 Deploy]
--------------------------------------------------------------------------------
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[VPC]:https://console.amazonaws-us-gov.com/vpc/home
[AMIs]:https://console.amazonaws-us-gov.com/ec2/home#Images
[Instances]:https://console.amazonaws-us-gov.com/ec2/home#Instances
[Elastic IPs]:https://console.amazonaws-us-gov.com/vpc/home#Addresses
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Route 53 DNS]:https://console.amazonaws-us-gov.com/route53/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
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
[Task 12 Build Nodes]:manual/12_BuildBootstrap.md
[Task 13 Deploy]:manual/13_Deploy.md
