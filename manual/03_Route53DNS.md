# [Task 03](../task/route53/) - Configure Route53 DNS
### Prerequisites:
  + [02 Setup AWS VPC]
--------------------------------------------------------------------------------
### Step 01\. Create Hosted Zone
###### Navigate: [AWS Console] > [Route 53] 
  1. Click: 'Create Hosted Zone'
  2. Complete with the following values format:
> Example values:
>
>   | Key          | Value               |
>   |:-------------|--------------------:|
>   | Domain Name  | ocp.{domain_name}   |
>   | Type         | Private Hosted Zone |
>   | VPC ID       | {your_new_vpc}      |

  3. Click: 'Create'

--------------------------------------------------------------------------------
#### Step 02\. Create SRV Records for etcd masters
###### Navigate: [AWS Console] > [Route 53] > {domain_name}
  1. Click `Create Record Set`
  2. Fill in with the following values:

>   Example:
>   | Key   | Value                   |
>   |-------|-------------------------|
>   | Name: | `_etcd-server-ssl._tcp` |
>   | Type: | `SRV - Service Locator` |

   Value                          
```
0 10 2380 etcd-0.ocp.{domain_name}
0 10 2380 etcd-1.ocp.{domain_name}
0 10 2380 etcd-2.ocp.{domain_name}
```
  5. Click: `Create`

--------------------------------------------------------------------------------
#### Step 03\. Create CNAME Record `api-int`
  1. Click `Create Record Set`
  2. Name: ` api-int `
  3. Type: ` CNAME - Canonical name `
  4. Value: ` replaceme ` (this is a temporary value)

--------------------------------------------------------------------------------
#### Step 04\. Create CNAME Record `api`
  1. Click `Create Record Set`
  2. Name: ` api `
  3. Type: ` CNAME - Canonical name `
  4. Value: ` replaceme ` (this is a temporary value)

--------------------------------------------------------------------------------
#### Step 05\. Create CNAME Record wildcard `*.apps`
  1. Click `Create Record Set`
  2. Name: ` *.apps `
  3. Type: ` CNAME - Canonical name `
  4. Value: ` replaceme ` (this is a temporary value)

--------------------------------------------------------------------------------
#### Step 06\. Create A Records for etcd Master Nodes
###### For each of the records {etcd-0,etcd-1,etcd-2} do the following:
  1. Click `Create Record Set`
  2. Name: ` etcd-0 `
  3. Type: ` A - IPv4 Address `
  4. Value: ` 192.168.1.100 ` 

--------------------------------------------------------------------------------
#### Step 07\. Create A Record for registry
  1. Click `Create Record Set`
  2. Name: ` registry `
  3. Type: ` A - IPv4 Address `
  4. Value: ` 192.168.1.100 ` 

---------------------------------------------------------------------------------
### Next Steps:
  + [04 Create Load Balancers]
--------------------------------------------------------------------------------
[02 Setup AWS VPC]:../manual/02_SetupVPC.md
[04 Create Load Balancers]:../manual/04_LoadBalancer.md
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Route 53]:https://console.amazonaws-us-gov.com/route53/home
