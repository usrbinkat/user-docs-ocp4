# [Task 05](../tasks/loadbalancer/) - Setup Load Balancers
### Prerequisite:
  + [Task 04 Setup Target Groups]
---------------------------------------------------------------------------------
### Step 01\. Create External Load Balancer
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers]
 1. Click: [Create Load Balancer]
 2. Select Type: 'Network Load Balancer'
 3. Fill in the following values:    
> Example shown creating with {az_public}, {vpc_name}, Port: 6443
>
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Name              | {vpc_name}-ext        |
>   | Scheme            | internal              |
>   | Protocol          | TCP                   |
>   | Port              | 6443                  |
>   | VPC               | {vpc_name}            |
>   | AZ {your_az}      | {your_public_zone}    |
>
>   | Tags: Key                        | Value  |
>   |---------------------------------:|:-------|
>   | kubernetes.io/cluster/{vpc_name} | shared |
    
  4. Continue click: [Next: Configure Security Settings]
  5. Confirm Security Warning; Click: [Next: Configure Routing]
  6. Fill in Target Group values per the following:
> Example shown creating with {vpc_name}
>
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Target Group      | Existing target group |
>   | Name              | {vpc_name}-aint       |

  7. Click: [Next: Register Targets]
  8. Click: [Next: Review]
  9. Click: [Create]

---------------------------------------------------------------------------------
### Step 02\. Create Internal Load Balancer
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers]
  1. Click: [Create Load Balancer]
  2. Select Type: 'Network Load Balancer'
  3. Fill in the following values:    
> Example shown creating with {az_private}, {vpc_name}, Port: 6443
>
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Name              | {vpc_name}-int        |
>   | Scheme            | internal              |
>   | Protocol          | TCP                   |
>   | Port              | 6443                  |
>   | VPC               | {vpc_name}            |
>   | AZ {your_az}      | {your_private_zone}   |
>
>   | Tags: Key                        | Value  |
>   |---------------------------------:|:-------|
>   | kubernetes.io/cluster/{vpc_name} | shared |
    
  4. Continue click: [Next: Configure Security Settings]
  5. Confirm Security Warning; Click: [Next: Configure Routing]
  6. Fill in Target Group values per the following:
  - (Example shown creating with {vpc_name})    
    
    | Option            | Value                 |
    |------------------:|:----------------------|
    | Target Group      | Existing target group |
    | Name              | {vpc_name}-aint       |

  7. Click: [Next: Register Targets]
  8. Click: [Next: Review]
  9. Click: [Create]

---------------------------------------------------------------------------------
### Next Steps:
  + [Task 06 Setup]
--------------------------------------------------------------------------------
[Task 04 Setup Target Groups]:../manual/04_TargetGroups.md
[Task 06 Setup ]:../manual/06_.md
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
[Load Balancers]:https://console.amazonaws-us-gov.com/ec2/v2/home#LoadBalancers
