# Part 04 - Setup Target Groups & Load Balancers
[Task Module](../task/loadbalancer/)
### Prerequisites:
  + [03 Route 53 DNS]
--------------------------------------------------------------------------------
#### 01\. Configure Target Groups
  + Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Target Groups]
###### For each of the 3 record/port sets do the following:
  - Record & Port Sets
| Name            | Port  |
| {vpc_name}-aint |  6443 |
| {vpc_name}-aent |  6443 |
| {vpc_name}-sint | 22623 |
    
  1. Click: | Create Target Group |
  2. Fill in the following values: 
    -   (Example shown with vpc_name-aint & 6443)
  | Option            | Value                 |
  |------------------:|:----------------------|
  | Target Group Name | {vpc_name}-aint       |
  | Target Type       | IP                    |
  | Protocol          | TCP                   |
  | Port              | 6443                  |
  | VPC               | {match your VPC Name} |
    
######  - You should now have 3 new Target Groups
  + Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers]
###### For both Load Balancers {vpc_name}-ext & {vpc_name}-int do the following:
  1. Click: | Create Load Balancer |
  2. Select Type: 'Network Load Balancer'
  3. Fill in the following values:
  | Option            | Value                 |
  |------------------:|:----------------------|
  | Name              | {vpc_name}-aint       |
  | Scheme            | internal              |
  | Protocol          | TCP                   |
  | Port              | 6443                  |
  | VPC               | {vpc_name}            |
  | AZ {your_az}      | {your public_zone}    |
  - Tags
  | Key                              | Value  |
  |---------------------------------:|:-------|
  | kubernetes.io/cluster/{vpc_name} | shared |
    
  4. Continue click: - Next: Configure Security Settings - 
  5. Confirm Security Warning; Click: - Next: Configure Routing -
  6. Fill in Target Group values per the following:
  | Option            | Value                 |
  |------------------:|:----------------------|
  | Target Group      | {vpc_name}-aint       |
  | Scheme            | internal              |
  | Protocol          | TCP                   |


#### 02\. Create 2 Load Balancers
#### 00\. Second Task Step
---------------------------------------------------------------------------------
### Next Steps:
  + [03 Next Task Name]
--------------------------------------------------------------------------------
[03 Route 53 DNS]:/manual/03_Route53DNS.md
[03 Next Task Name]:/manual/00_NextTaskName.md
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
[Load Balancers]:https://console.amazonaws-us-gov.com/ec2/v2/home#LoadBalancers
