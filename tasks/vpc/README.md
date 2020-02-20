# Part 02 - Setup AWS VPC
[Task Module](../task/vpc/)
### Prerequisites:
  + [01 Prerequisites]
--------------------------------------------------------------------------------
#### 01\. Create VPC
  + Navigate: [AWS Console] > [VPC Service] > Start VPC Wizard
  1. Select VPC with Public and Private subnets
  2. Confirm IPv4 CIDR block: (Example `10.0.0.0/16`)
  3. Confirm IPv6 CIDR block: (Example `No IPv6 CIDR Block`)
  4. Provide VPC Name (This is arbitrary and not important for OCP4 Deploy)
  5. Select Elastic IP Allocation: (Use Dropdown)
  5. Set Availability Zone (Example `us-gov-west-1a`)
  6. Set Enable DNS hostnames: `yes`
  8. Click `Create VPC`

#### 02\. Configure Tagging on Public & Private Subnets
  + Navigate: [AWS Console] > [VPC Service] > [Subnets] 
  1. Navigate: Public Subnet > Tags
  1. Click `Add/Edit Tags`
  2. Create the following    
  TODO: make table    
    ```
    Name ocp4io-public-us-gov-west-1a
    kubernetes.io/cluster/ocp4io owned
    ```
  3. Click `Save`
  4. Navigate: Private Subnet > Tags
  5. Click `Add/Edit Tags`
  6. Create the following    
  TODO: make table    
    ```
    Name ocp4io-private-us-gov-west-1a
    kubernetes.io/cluster/ocp4io owned
    ```
  7. Click `Save`
  
---------------------------------------------------------------------------------
### Next Steps:
  + [03 Next Task Name]
--------------------------------------------------------------------------------
[01 Prerequisites]:/manual/01_Prerequisites.md
[03 Next Task Name]:/manual/00_NextTaskName.md
[AWS Console]:https://console.amazonaws-us-gov.com/console/home?region=us-gov-west-1#
[VPC Service]:https://console.amazonaws-us-gov.com/vpc/home?region=us-gov-west-1
[Subnets]:https://console.amazonaws-us-gov.com/vpc/home?region=us-gov-west-1#dashboard:

```
Refrence
https://console.amazonaws-us-gov.com/vpc/home?region=us-gov-west-1#subnets:
https://console.amazonaws-us-gov.com/vpc/home?region=us-gov-west-1#dashboard:
```
