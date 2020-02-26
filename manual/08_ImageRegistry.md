# [Task 08](../tasks/registry) - Setup Image Registry
### Prerequisite:
  + [07 Setup IAM Roles]
--------------------------------------------------------------------------------
### Step 01\. Create Elastic IP
###### Navigate: [AWS Console] > [VPC] > [Elastic IPs]
  1. Click: `Allocate new address`
  2. Set IPv4 address pool option: `Amazon pool`
  3. Click: `Allocate`
  4. Copy new Elastic IP Addess
  5. Click: `Close`

###### Navigate: [Your Domain Registrar > Your DNS Records Settings]
  6. Create new up stream DNS A Record
>   DNS A Record Table
>
>   | Subdomain Name | Type | Value                |
>   |:--------------:|:----:|:--------------------:|
>   | registry.ocp   | A    | {elastic_ip_address} |

---------------------------------------------------------------------------------
### Step 02\. Create Red Hat CoreOS Amazon Machine Image
###### Navigate: [AWS Console] > [EC2] > [AMIs]
  1. Select: `rhcos` image
  2. Click: `Launch`
  3. Select Instance Type: minimum of `t2.xlarge`
  4. Click: `Next: Configure Instance Details`
  5. Set values as per below

>   Image Regristy AMI Configure Instance Options Table
>
>   |                |                     |       |
>   |:--------------:|:-------------------:|:-----:|
>   | Instance Type  | t2.xlarge           |       |
>   | Network        | {vpc_name}          |       |
>   | Subnet         | {vpc_name}-public-* |       |

  6. Add 'Configure Instance' `Advance Details` User Data 
    - Copy/Paste the following user data
```
{"ignition":{"config":{},"security":{"tls":{}},"timeouts":{},"version":"2.2.0"},"networkd":{},"passwd":{"users":[{"name":"core","sshAuthorizedKeys":["Your public Key"]}]},"storage":{},"systemd":{}}
```

>   | Instance Type  |                     |       |
>   | Instance Type  |                     |       |
>   | Instance Type  |                     |       |
>   | Instance Type  |                     |       |
>   | Instance Type  |                     |       |
>   | Instance Type  |                     |       |
>   | Instance Type  |                     |       |
>   | Instance Type  |                     |       |
>   | Instance Type  |                     |       |

```
```

---------------------------------------------------------------------------------
### Step 03\. Create IAM Roles `worker` & `master`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Roles]
###### - For both profiles `{vpc_name}-master-profile` & `{vpc_name}-worker-profile` tag per the following
  1. Click: `Create Role`
  2. Select "EC2" under heading "Choose a use case" 
  3. Click: `Next: Permissions`
  4. Filter for term `{vpc_name}-master-profile` & Check left selection box
  5. Click: `Next: Tags`
  6. Fill in with the following Key:Value format
>   Role Tags Table:
>
>   | Role                      | Key  | Value                       |
>   |---------------------------|-----:|:----------------------------|
>   | {vpc_name}-master-profile | Name | `{vpc_name}-master-profile` |
>   | {vpc_name}-worker-profile | Name | `{vpc_name}-worker-profile` |

  7. Click: `Next: Review`
  8. Fill in Role Name: eg `{vpc_name}-master-profile`
  9. Click: `Create Role`
    
###### Verify: 'Lower Panel > Permissions > Policy Name > Expand all' should list all 3 new policies
        

--------------------------------------------------------------------------------
### Step 04\. Create IAM User Role
###### Navigate: [AWS Console] > [EC2] > [IAM] > [Users] > Lower Pannel > Permissions
###### - For each of the 3 IAM User Roles in the `IAM User Role Index` below, do the following:

  1. Filter for your aws administrator user name & click on user
  2. Click: `Add inline policy`
  3. Click: `JSON` Tab
  4. Erase default content
  5. Fill in with associated JSON content
  6. Click: `Create Policy`
  7. Provide `Name` value
  8. Click: `Create Policy`


---------------------------------------------------------------------------------
### Next Steps:
  + [09 Create Bootstrap Node]
--------------------------------------------------------------------------------
[07 Setup IAM Roles]:../manual/07_IAMRoles.md
[09 Create Bootstrap Node]:../manual/09_Bootstrap.md
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[VPC]:https://console.amazonaws-us-gov.com/vpc/home
[AMIs]:https://console.amazonaws-us-gov.com/ec2/home#Images
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Elastic IPs]:https://console.amazonaws-us-gov.com/vpc/home#Addresses
