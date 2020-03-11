# [Task 05](../task/route53/) - Configure Route53 DNS
### Prerequisites:
  + [Task 04 Setup AWS VPC]
--------------------------------------------------------------------------------
### Step 01\. Create `Hosted Zone`
###### Navigate: [AWS Console] > [Route 53] 
  1. Click: 'Create Hosted Zone'
  2. Complete with the following values format:
> Example values:
>
>   | Key          | Value               |
>   |:-------------|--------------------:|
>   | Domain Name  | `{cluster_domain}`  |
>   | Type         | Private Hosted Zone |
>   | VPC ID       | `{your_new_vpc}`    |

  3. Click: 'Create'

--------------------------------------------------------------------------------
### Step 02\. Create master etcd node `SRV Records`
###### Navigate: [AWS Console] > [Route 53] > {cluster_domain}
  1. Click 'Create Record Set'
  2. Fill in with the following values:
>   Example:
>
>   | Key   | Value                             |
>   |-------|-----------------------------------|
>   | Name  | `_etcd-server-ssl._tcp`           |
>   | Type  | `SRV - Service Locator`           |
>   | Value |  0 10 2380 etcd-0.{domain_domain} |
>   |       |  0 10 2380 etcd-1.{domain_domain} |
>   |       |  0 10 2380 etcd-2.{domain_domain} |

   `Value` Field:                          
```
0 10 2380 etcd-0.ocp.{domain_name}
0 10 2380 etcd-1.ocp.{domain_name}
0 10 2380 etcd-2.ocp.{domain_name}
```
  3. Click: 'Create'

--------------------------------------------------------------------------------
### Step 03\. Create  api-int `CNAME Record`
###### Navigate: [AWS Console] > [Route 53] > {cluster_domain}
  1. Click 'Create Record Set'
  2. Fill in with the following values:
>   Example:
>
>   | Key   | Value                   |
>   |-------|-------------------------|
>   | Name  | ` api-int `             |
>   | Type  | CNAME - Canonical name  |
>   | Value | ` replaceme `           |

  3. Click: 'Create'

--------------------------------------------------------------------------------
### Step 04\. Create  api `CNAME Record`
###### Navigate: [AWS Console] > [Route 53] > {cluster_domain}
  1. Click `Create Record Set`
  2. Fill in with the following values:
>   Example:
>
>   | Key   | Value                   |
>   |-------|-------------------------|
>   | Name  | ` api `                 |
>   | Type  | CNAME - Canonical name  |
>   | Value | ` replaceme `           |

  3. Click: 'Create'

--------------------------------------------------------------------------------
### Step 05\. Create wildcard  *.apps `CNAME Record`
###### Navigate: [AWS Console] > [Route 53] > {cluster_domain}
  1. Click `Create Record Set`
  2. Fill in with the following values:
>   Example:
>
>   | Key   | Value                   |
>   |-------|-------------------------|
>   | Name  | ` *.apps `              |
>   | Type  | CNAME - Canonical name  |
>   | Value | ` replaceme `           |

  3. Click: 'Create'

--------------------------------------------------------------------------------
### Step 06\. Create etcd Master Node `A Records`
###### Navigate: [AWS Console] > [Route 53] > {cluster_domain}
###### For each of the records {etcd-0,etcd-1,etcd-2} do the following:
  1. Click `Create Record Set`
  2. Fill in with the following values:
>   Example:
>
>   | Key   | Value                   |
>   |-------|-------------------------|
>   | Name  | ` etcd-0 `              |
>   | Type  | A - IPv4 Address        |
>   | Value | ` 192.168.1.100 `       |

  3. Click: 'Create'
  4. You should now have 3 records, one for each of ` etcd-0 etcd-1 etcd-2 `

--------------------------------------------------------------------------------
### Step 07\. Create registry `A Record`
###### Navigate: [AWS Console] > [Route 53] > {cluster_domain}
  1. Click `Create Record Set`
  2. Fill in with the following values:
>   Example:
>
>   | Key   | Value                   |
>   |-------|-------------------------|
>   | Name  | ` registry `            |
>   | Type  | A - IPv4 Address        |
>   | Value | ` 192.168.1.100 `       |

  3. Click: 'Create'

---------------------------------------------------------------------------------
### Next Steps:
  + [Task 06 Setup Target Groups]
--------------------------------------------------------------------------------
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Route 53]:https://console.amazonaws-us-gov.com/route53/home
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
[Task 12 Build Nodes]:manual/12_BuildNodes.md
[Task 13 Deploy]:manual/13_Deploy.md
