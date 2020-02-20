# Part 02 - Configure Route53 DNS
[Task Module](../task/route53/)
### Prerequisites:
  + [02 Setup AWS VPC]
--------------------------------------------------------------------------------
#### 00\. Create Hosted Zone
  + Navigate: [AWS Console] > [Route 53] > Create Hosted Zone
#### 01\. Create Hosted Zone
  Fill In:
  1. Domain Name: ` ocp.{domain_name} `
  2. Type: Private Hosted Zone
  3. VPC ID: {your new vpc} (Use Drop down)
  4. Click `Create`

#### 02\. Create SRV Records for etcd masters
  + Navigate: [AWS Console] > [Route 53] > {domain_name}
  1. Click `Create Record Set`
  2. Name: `_etcd-server-ssl._tcp`
  3. Type: ` SRV - Service Locator `
  4. Value:
```
0 10 2380 etcd-0.ocp.{domain_name}
0 10 2380 etcd-1.ocp.{domain_name}
0 10 2380 etcd-2.ocp.{domain_name}
```
  5. Click: `Create`

#### 02\. Create CNAME Record `api-int`
  1. Click `Create Record Set`
  2. Name: ` api-int `
  3. Type: ` CNAME - Canonical name `
  4. Value: ` replaceme ` (this is a temporary value)

#### 03\. Create CNAME Record `api`
  1. Click `Create Record Set`
  2. Name: ` api `
  3. Type: ` CNAME - Canonical name `
  4. Value: ` replaceme ` (this is a temporary value)

#### 04\. Create CNAME Record wildcard `*.apps`
  1. Click `Create Record Set`
  2. Name: ` *.apps `
  3. Type: ` CNAME - Canonical name `
  4. Value: ` replaceme ` (this is a temporary value)

#### 05\. Create A Records for etcd Master Nodes
###### For each of the records {etcd-0,etcd-1,etcd-2} do the following:
  1. Click `Create Record Set`
  2. Name: ` etcd-0 `
  3. Type: ` A - IPv4 Address `
  4. Value: ` 192.168.1.100 ` 

#### 06\. Create A Record for registry
  1. Click `Create Record Set`
  2. Name: ` registry `
  3. Type: ` A - IPv4 Address `
  4. Value: ` 192.168.1.100 ` 

---------------------------------------------------------------------------------
### Next Steps:
  + [04 Next Task Name]
--------------------------------------------------------------------------------
[02 Previous Task Name]:../manual/02_SetupVPC.md
[04 Next Task Name]:manual/00_NextTaskName.md
[AWS Console]:https://console.amazonaws-us-gov.com/console/home?region=us-gov-west-1#
[Route 53]:https://console.amazonaws-us-gov.com/route53/home
