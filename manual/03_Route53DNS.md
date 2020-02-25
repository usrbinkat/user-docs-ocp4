# [Task 03](../task/route53/) - Configure Route53 DNS
### Prerequisites:
  + [02 Setup AWS VPC]
--------------------------------------------------------------------------------
### Step 01\. Create `Hosted Zone`
###### Navigate: [AWS Console] > [Route 53] 
  1. Click: 'Create Hosted Zone'
  2. Complete with the following values format:
> Example values:
>
>   | Key          | Value               |
>   |:-------------|--------------------:|
>   | Domain Name  | `ocp.{domain_name}` |
>   | Type         | Private Hosted Zone |
>   | VPC ID       | `{your_new_vpc}`    |

  3. Click: 'Create'

--------------------------------------------------------------------------------
### Step 02\. Create master etcd node `SRV Records`
###### Navigate: [AWS Console] > [Route 53] > {domain_name}
  1. Click 'Create Record Set'
  2. Fill in with the following values:
>   Example:
>
>   | Key   | Value                               |
>   |-------|-------------------------------------|
>   | Name  | `_etcd-server-ssl._tcp`             |
>   | Type  | `SRV - Service Locator`             |
>   | Value |  0 10 2380 etcd-0.ocp.{domain_name} |
>   |       |  0 10 2380 etcd-1.ocp.{domain_name} |
>   |       |  0 10 2380 etcd-2.ocp.{domain_name} |

   `Value` Field:                          
```
0 10 2380 etcd-0.ocp.{domain_name}
0 10 2380 etcd-1.ocp.{domain_name}
0 10 2380 etcd-2.ocp.{domain_name}
```
  3. Click: 'Create'

--------------------------------------------------------------------------------
### Step 03\. Create  api-int `CNAME Record`
###### Navigate: [AWS Console] > [Route 53] > {domain_name}
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
###### Navigate: [AWS Console] > [Route 53] > {domain_name}
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
###### Navigate: [AWS Console] > [Route 53] > {domain_name}
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
###### Navigate: [AWS Console] > [Route 53] > {domain_name}
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
###### Navigate: [AWS Console] > [Route 53] > {domain_name}
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
  + [04 Create Load Balancers]
--------------------------------------------------------------------------------
[02 Setup AWS VPC]:../manual/02_SetupVPC.md
[04 Create Load Balancers]:../manual/04_LoadBalancer.md
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Route 53]:https://console.amazonaws-us-gov.com/route53/home
