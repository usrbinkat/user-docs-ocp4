# [Task 11](../tasks/registry) - Setup Image Registry Services & Publish Artifacts
### Prerequisite:
  + [Task 10 Image Registry Instance]
--------------------------------------------------------------------------------
### Step 02\. SSH to the registry instance & stage assets
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
  5. Acquire root
```
sudo -i
```
  6. Extract tar archive
```
tar -xvf $(ls /home/core/*.tar.xz) -C ./ 
```
  6. Start tmux session for safety
```
tmux
```
  6. Source environment
```
source ./*/environment
```

--------------------------------------------------------------------------------
### Step 02\. SSH to the registry instance & Acquire root

  1. SSH to the registry instance
  - Example1: ` ssh -i ~/.ssh/id_rsa_${CLUSTER_DOMAIN} core@registry.${CLUSTER_DOMAIN} `
  - Example2: ` ssh -i ~/.ssh/id_rsa_${CLUSTER_DOMAIN} core@57.200.96.1000 `
  2. Acquire root: ` sudo -i `
  3. Start tmux session for safety
    `tmux`

---------------------------------------------------------------------------------
### Step 07\. scp artifacts
```
```
TODO write scp method & untar / stage all artifacts
---------------------------------------------------------------------------------
### Step 07\. source env vars
```
```
TODO write env sourcing method
---------------------------------------------------------------------------------
### Step 07\. Load registry & nginx images from file
```
podman load -i nginx.tar
podman load -i registry.tar
```
TODO write podman load method
---------------------------------------------------------------------------------
### Step 07\. Build Image Registry Service
  1. Start registry container
```
podman run \
  --name registry                                                                       \
  --detach                                                                              \
  --net=host                                                                            \
  --restart=always                                                                      \
  --env REGISTRY_HTTP_ADDR=0.0.0.0:443                                                  \
  --env REGISTRY_HTTP_TLS_KEY=/root/${CLUSTER_DOMAIN}/ssl/${CLUSTER_DOMAIN}.key         \
  --env REGISTRY_HTTP_TLS_CERTIFICATE=/root/${CLUSTER_DOMAIN}/ssl/${CLUSTER_DOMAIN}.pem \
  --volume /root/${CLUSTER_DOMAIN}/ssl:/root/${CLUSTER_DOMAIN}/ssl                      \
docker.io/library/registry:2
```
TODO: need to merge registry startup flags
```
podman run --name registry -p 5000:5000 \
  -v ~/registry1/data:/var/lib/registry:z \
  -v ~/registry1/auth:/auth:z \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -v ~/registry1/certs:/certs:z \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/examle.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/example.key \
  -d docker.io/library/registry:2
```

---------------------------------------------------------------------------------
### Step 08\. Load images into mirror
TODO: BROKEN VARS need to resolve hard coded copy paste example items

  1. Pull ocp-release container image
```
oc adm release mirror --from=quay.io/${PRODUCT_REPO}/${RELEASE_NAME}:${OCP_RELEASE} --to-dir=${HOME}/${CLUSTER_DOMAIN}/mirror
```
```
oc adm release mirror \
    --from=quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64          \
    --to=registry.${CLUSTER_DOMAIN}/ocp/release                                 \
    --to-release-image=registry.${CLUSTER_DOMAIN}/ocp/release:4.3.0-rc.3-x86_64
```
```
oc image mirror -a /tmp/pull-secret.json --dir=/tmp/mirror-file file://openshift/release:4.3.0-rc.3* registry.ocp.example.com:5000/ocp-4.3
```
  + [Example Success Message]    
---------------------------------------------------------------------------------
---------------------------------------------------------------------------------
### Step 14. Setup NGINX Artifact Service
  1. Create base artifact service web root
```
mkdir /home/core/html
```
  2. Start NGINX container
```
podman run \
    --rm                                           \
    --name=nginx                                   \
    --net=host                                     \
    --detach                                       \
    --privileged                                   \
    --volume /home/core/html:/usr/share/nginx/html \
  docker.io/library/nginx:latest
```
  3. Push bootstrap ignition to web root
```
cp /root/govlcloud/bootstrap.ign /home/core/html/
```
  4. Set Artifact Permissions
```
chmod -R 755 /home/core/html/
```

---------------------------------------------------------------------------------
### Step 08\. Load & Update CA
```
cp ~/registry1/certs/example.crt /etc/pki/ca-trust/source/anchors/ && update-ca-trust
```

---------------------------------------------------------------------------------
### Step 08\. Test registry catalog & auth
```
curl -u user:pass -k https://10.0.1.94:5000/v2/_catalog
curl -u user:pass -k https://registry.ocp.example.com:5000/v2/ocp-4.3/tags/list
```
---------------------------------------------------------------------------------
### Next Steps:
  + [Task 13 Deploy]
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
