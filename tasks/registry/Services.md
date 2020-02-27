# [Task 09](../tasks/registry) - Setup Image Registry Artifacts & Services
### Prerequisite:
  + [08 Build Image Registy Instance]
--------------------------------------------------------------------------------
### Step 01\. Acquire Pull Secret
###### Navigate: [Red Hat OpenShift Cluster Manager] > Install > AWS > [User-provisioned Infrastructure]
  1. Click: `Copy pull secret`

--------------------------------------------------------------------------------
### Step 02\. SSH to the registry instance & Acquire root
>   NOTE:
>   Continuing assumes you are able to use dig and or nslookup to resolve your
>   registry instance hostname to the public facing elastic ip address and that
>   ssh to that address works from your local machine as well.

  1. SSH to the registry instance
  - Example1: ` ssh -i ~/.ssh/id_rsa_cluster core@registry.ocp.cluster.com `
  - Example2: ` ssh -i ~/.ssh/id_rsa_cluster core@57.200.96.1000 `
  2. Acquire root: ` sudo -i `

---------------------------------------------------------------------------------
### Step 03\. Stage Pull Secret
  1. CMD: ` mkdir /root/.docker `
  2. CMD: ` vi /root/.docker/config.json `
  3. Paste Pull Secret from clipboard & save/close


---------------------------------------------------------------------------------
### Step 04\. Stage Pull Secret

>   Image Registry AMI `Add Tags` Table
>
>   | Key  | Value                    |
>   |:----:|:------------------------:|
>   | Name | {vpc_name}-registry-node |

 11. Click: `Review and Launch`
 12. Click: `Launch`
 13. Select Option: Proceed without a key pair

###### Navigate: [AWS Console] > [VPC] > [Elastic IPs]
 14. Select the EIP created in step 01
 15. Click: `Actions` menu
 16. Select: `Associate address`
 17. Select: Instance menu option for your instance `{vpc_name}-registry-node`
 18. Select: Private IP menu option for your `{vpc_name}-registry-node` instance ip
 19. Click: `Associate`

###### Navigate: [AWS Console] > [EC2] > [Instances] > {vpc_name}-registry-node > Lower Panel > Description
 20. Copy 'Private IP'

###### Navigate: [AWS Console] > [VPC] > [Route 53 DNS] > {your_domain_name}
 21. Select A Record for registry.{your\_domain\_name}
 22. Update Record with the registry node Private IP

---------------------------------------------------------------------------------
### Next Steps:
  + [10 Create Bootstrap Node]
--------------------------------------------------------------------------------
[08 Build Image Registy Instance]:../manual/08_ImageRegistryInstance.md
[10 Create Bootstrap Node]:../manual/10_Bootstrap.md
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[VPC]:https://console.amazonaws-us-gov.com/vpc/home
[AMIs]:https://console.amazonaws-us-gov.com/ec2/home#Images
[Instances]:https://console.amazonaws-us-gov.com/ec2/home#Instances
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Elastic IPs]:https://console.amazonaws-us-gov.com/vpc/home#Addresses
[Route 53 DNS]:https://console.amazonaws-us-gov.com/route53/home
[User-provisioned Infrastructure]:https://cloud.redhat.com/openshift/install/aws/user-provisioned
