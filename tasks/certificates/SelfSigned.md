# Create Self Signed SSL Certificate Assets
```
openssl req \
  -x509 \
  -nodes \
  -sha256 \
  -days   3650 \
  -newkey rsa:4096 \
  -subj   "/CN=${CLUSTER_DOMAIN}" \
  -out    ${HOME}/${CLUSTER_DOMAIN}/ssl/${CLUSTER_DOMAIN}.crt  \
  -keyout ${HOME}/${CLUSTER_DOMAIN}/ssl/${CLUSTER_DOMAIN}.key  \
  -addext "subjectAltName=DNS:registry.${CLUSTER_DOMAIN},DNS:${DOMAIN_NAME},IP:10.0.1.1" 

```
TODO: correct brittle hard coded IP address    
TODO: Describe creation of certificate bundle    
TODO: Link to stage for certificate use in install-config.yaml    
TODO: Align naming convention and path placement across all certificate handling instructions

---------------------------------------------------------------------------------
### Next Steps: Resume [Configure Assets] @ [Write install-config.yaml]

---------------------------------------------------------------------------------
[Write install-config.yaml]:https://dccscr.dsop.io/levelup-automation/infrastucture/user-docs-ocp4/-/blob/aws-gov-disconnected/manual/02_StageAssets.md#step-10-write-install-configyaml
[Configure Assets]:https://dccscr.dsop.io/levelup-automation/infrastucture/user-docs-ocp4/-/blob/aws-gov-disconnected/manual/02_StageAssets.md#step-10-write-install-configyaml
[Task 01 Prerequisites]:manual/01_Prerequisites.md
[Task 02 Stage Assets]:manual/02_StageAssets.md
[Task 03 Write Manifest & Ignition]:manual/03_ManifestIgnition.md
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
