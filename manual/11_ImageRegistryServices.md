# [Task 11](../tasks/registry) - Setup Image Registry Services & Publish Artifacts
### Prerequisite:
  + [Task 10 Image Registry Instance]
--------------------------------------------------------------------------------
### Step 01\. SSH to the registry instance & stage assets
  1. Test SSH to the registry instance
  - Example1: ` ssh -i ~/.ssh/id_rsa_cluster core@registry.ocp.cluster.com `
  - Example2: ` ssh -i ~/.ssh/id_rsa_cluster core@57.200.96.1000 `
  2. exit - CMD:
```
exit
```
  3. tar artifacts
```
tar -C ${HOME} -Jcvf /tmp/${CLUSTER_DOMAIN}.tar.xz ${CLUSTER_DOMAIN}
```
  4. scp artifact bundle - CMD:
```
scp -i ssh/id_rsa_${CLUSTER_DOMAIN} /tmp/${CLUSTER_DOMAIN}.tar.xz core@registry.${CLUSTER_DOMAIN}:~
```
  5. SSH Back to Registry Node
```
ssh core@{REGISTRY_NODE_IP}
```
  5. Acquire root
```
sudo -i
```
  6. Extract tar archive
```
tar -xvf $(ls /home/core/*.tar.xz) -C ./ 
```
  7. Start tmux session for safety
```
tmux
```
  8. Source environment & change to artifact directory
```
source ./*/environment && cd ${HOME}/${CLUSTER_DOMAIN}
```

---------------------------------------------------------------------------------
### Step 02\. Load registry & nginx images from file
```
podman load -i ${HOME}/${CLUSTER_DOMAIN}/images/docker-nginxlatest-image.tar
podman load -i ${HOME}/${CLUSTER_DOMAIN}/images/docker-registry2-image.tar
```
---------------------------------------------------------------------------------
### Step 03. Setup NGINX Artifact Service
  1. Create base artifact service web root
```
chmod -R 755 ${HOME}/${CLUSTER_DOMAIN}/data
```
  2. Start NGINX container
```
podman run \
  --name=nginx                                                     \
  --rm                                                             \
  --detach                                                         \
  --net=host                                                       \
  --privileged                                                     \
  --volume ${HOME}/${CLUSTER_DOMAIN}/data:/usr/share/nginx/html:z  \
docker.io/library/nginx:latest
```
  3. Test curl bootstrap.ign json block
```
curl localhost:80/bootstrap.ign
```

---------------------------------------------------------------------------------
### Step 04\. Build Image Registry Service
  1. Start registry container
```
podman run \
  --name registry                                                        \
  --detach                                                               \
  --net=host                                                             \
  --privileged                                                           \
  --restart=always                                                       \
  --publish 443:443                                                      \
  --publish 5000:5000                                                    \
  --env    REGISTRY_HTTP_ADDR=0.0.0.0:443                                \
  --env    "REGISTRY_AUTH=htpasswd"                                      \
  --env    REGISTRY_AUTH_HTPASSWD_PATH=/root/auth/htpasswd               \
  --volume ${HOME}/${CLUSTER_DOMAIN}/auth/htpasswd:/root/auth/htpasswd:z \
  --env    REGISTRY_HTTP_TLS_KEY=/root/ssl/${CLUSTER_DOMAIN}.key         \
  --env    REGISTRY_HTTP_TLS_CERTIFICATE=/root/ssl/${CLUSTER_DOMAIN}.crt \
  --volume ${HOME}/${CLUSTER_DOMAIN}/ssl:/root/ssl                       \
  --env    "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm"                 \
  --volume ${HOME}/${CLUSTER_DOMAIN}/registry:/var/lib/registry:z        \
docker.io/library/registry:2
```
TODO: test registry flags

---------------------------------------------------------------------------------
### Step 05\. Load & Update CA
>    For Self Signed & Organization Provided CA

```
cp /root/${CLUSTER_DOMAIN}/ssl/${CLUSTER_DOMAIN}.crt /etc/pki/ca-trust/source/anchors/${CLUSTER_DOMAIN}.crt && update-ca-trust
```

---------------------------------------------------------------------------------
### Step 06\. Load images into retistry mirror
TODO: BROKEN VARS need to resolve hard coded copy paste example items

  1. Pull ocp-release container image
```
oc adm release mirror --from=quay.io/openshift-release-dev/ocp-release:4.3.5-x86_64 --to-dir=${HOME}/${CLUSTER_DOMAIN}/mirror
```
```
oc image mirror -a .docker/config.json --dir=/root/cluster.dsop.dev/mirror file://openshift/release:4.3.5* registry.cluster.dsop.dev/ocp-4.3
```
  + [Example Success Message]    
---------------------------------------------------------------------------------
### Step 07\. Test registry catalog & auth
```
curl -u ${VPC_NAME}:${VPC_NAME} -k https://registry.${CLUSTER_DOMAIN}/v2/_catalog
```
---------------------------------------------------------------------------------
### Next Steps:
  + [Task 12 Build Nodes]
--------------------------------------------------------------------------------
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[VPC]:https://console.amazonaws-us-gov.com/vpc/home
[AMIs]:https://console.amazonaws-us-gov.com/ec2/home#Images
[Instances]:https://console.amazonaws-us-gov.com/ec2/home#Instances
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
[Elastic IPs]:https://console.amazonaws-us-gov.com/vpc/home#Addresses
[Route 53 DNS]:https://console.amazonaws-us-gov.com/route53/home
[User-provisioned Infrastructure]:https://cloud.redhat.com/openshift/install/aws/user-provisioned
[Red Hat OpenShift Cluster Manager]:https://cloud.redhat.com/openshift/
[Example Success Message]:../tasks/registry/lib/install-config/oc_adm_success_example.txt
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
