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
>   | Scheme            | internet facing       |
>   | Protocol          | TCP                   |
>   | Port              | 6443                  |
>   | VPC               | {vpc_name}            |
>   | AZ: {your_az}     | {your_public_zone}    |
>
>   | Tags: Key                        | Value  |
>   |---------------------------------:|:-------|
>   | kubernetes.io/cluster/{vpc_name} | shared |
    
  4. Continue click: [Next: Configure Security Settings]
  5. Confirm Security Warning; Click: [Next: Configure Routing]
  6. Fill in Target Group values per the following:
> Example:
>
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Target Group      | Existing target group |
>   | Name              | {vpc_name}-aext       |

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
>   | Name              | {vpc_name}-aint       |
>   | Scheme            | internal              |
>   | Protocol          | TCP                   |
>   | Port              | 6443                  |
>   | VPC               | {vpc_name}            |
>   | AZ: {your_az}     | {your_private_zone}   |
>
>   | Tags: Key                        | Value  |
>   |---------------------------------:|:-------|
>   | kubernetes.io/cluster/{vpc_name} | shared |
    
  4. Continue click: [Next: Configure Security Settings]
  5. Confirm Security Warning; Click: [Next: Configure Routing]
  6. Fill in Target Group values per the following:
> Example:
>   
>   | Option            | Value                 |
>   |------------------:|:----------------------|
>   | Target Group      | Existing target group |
>   | Name              | {vpc_name}-aint       |

  7. Click: [Next: Register Targets]
  8. Click: [Next: Review]
  9. Click: [Create]

---------------------------------------------------------------------------------
### Step 03\. Add Listener to Internal Load Balancer 
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers] > {vpc_name}-int > Lower Tab 'Listeners'
  1. Click: [Add listener]
  2. Fill in the following:
> Example:
>   
>   | Protocol | Port   |
>   |---------:|:-------|
>   | TCP      | 22623  |
  3. Click: [Save]

---------------------------------------------------------------------------------
### Step 04\. Add Internal Listener to DNS Name Records
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers] > {vpc_name}-int > Lower Tab 'Description'
  1. Copy 'DNS Name' value 
###### Navigate: [AWS Console] > [Route 53] > Domain Name ocp.{domain_name} > api-int.ocp.{domain_name}
  2. Paste {vpc_name}-int 'DNS Name' value into api-int.ocp.{domain_name} > CNAME > Value field
  3. Click: [Save Record Set]
###### Navigate: [AWS Console] > [Route 53] > Domain Name ocp.{domain_name} > api.ocp.{domain_name}
  4. Paste {vpc_name}-int 'DNS Name' value into api.ocp.{domain_name} > CNAME > Value field
  5. Click: [Save Record Set]

---------------------------------------------------------------------------------
### Step 05\. Add Upstream DNS Records
###### Navigate: [AWS Console] > [EC2] > Left Panel > Load Balancing > [Load Balancers] > {vpc_name}-int > Lower Tab 'Description'

---------------------------------------------------------------------------------
### Next Steps:
  + [Task 06 Setup]
--------------------------------------------------------------------------------
[Task 04 Setup Target Groups]:../manual/04_TargetGroups.md
[Task 06 Setup ]:../manual/06_.md
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[Route 53]:https://console.amazonaws-us-gov.com/route53/home
[Target Groups]:https://console.amazonaws-us-gov.com/ec2/home#TargetGroups
[Load Balancers]:https://console.amazonaws-us-gov.com/ec2/v2/home#LoadBalancers
