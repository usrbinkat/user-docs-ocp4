# [Task 09](../tasks/registry) - Setup Image Registry Artifacts & Services | Lets Encrypt Method
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
  3. Start tmux session for safety
    `tmux`

---------------------------------------------------------------------------------
### Step 03\. Stage Pull Secret
  1. CMD: ` mkdir /root/.docker `
  2. CMD: ` vi /root/.docker/config.json `
  3. Paste Pull Secret from clipboard & save/close

---------------------------------------------------------------------------------
### Step 04\. Export required variables
  1. Export AWS Region Variable
  - Example: `export AWS_REGION='us-east-1'; echo ${AWS_REGION}`
>   Supported Values:
>
>   ap-northeast-1, ap-northeast-2, ap-south-1,    
>   ap-southeast-1, ap-southeast-2, ca-central-1,    
>   eu-central-1, eu-north-1, eu-west-1,    
>   eu-west-2, eu-west-3, sa-east-1,    
>   us-east-1, us-east-2, us-west-1, us-west-2    
>   

  2. Export cluster name
```
export CLUSTER_NAME='ocp'; echo ${CLUSTER_NAME} 
```
  3. Export cluster domain
```
export DOMAIN_NAME='cluster.com'; echo ${DOMAIN_NAME}
```
  4. Export cluster domain name
```
export CLUSTER_DOMAIN="${CLUSTER_NAME}.${DOMAIN_NAME}"; echo ${CLUSTER_DOMAIN}
```
  5. Export letsencrypt certificate registry email address
```
export CERT_EMAIL="admin@${DOMAIN_NAME}"; echo ${CERT_EMAIL}
```

---------------------------------------------------------------------------------
### Step 05\. Provision ACME Lets Encrypt SSL Certificates
  1. Make letsencrypt directories
```
mkdir /etc/letsencrypt /var/lib/letsencrypt
```
  2. Run letsencrypt container to acquire certificates    
  TODO: [Fix http port 80 enablement](https://dccscr.dsop.io/levelup-automation/infrastucture/user-docs-ocp4/issues/2)
```
podman run                                              \
    --rm                                                \
    --net=host                                          \
    --privileged                                        \
    --volume /etc/letsencrypt:/etc/letsencrypt          \
    --volume /var/lib/letsencrypt:/var/lib/letsencrypt  \
  docker.io/certbot/certbot certonly                    \
    --agree-tos                                         \
    --standalone                                        \
    --non-interactive                                   \
  -m "${CERT_EMAIL}" -d "registry.${CLUSTER_DOMAIN}"
```
---------------------------------------------------------------------------------
### Step 06\. Test Quay.io image pull
  1. Pull ocp-release container image
```
podman pull quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64
```

---------------------------------------------------------------------------------
### Step 07\. Build Image Registry Service
  1. Start registry container
```
podman run \
  -d                                                                                              \
  --net=host                                                                                      \
  --name registry                                                                                 \
  --restart=always                                                                                \
  --volume /etc/letsencrypt:/etc/letsencrypt                                                      \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443                                                               \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/etc/letsencrypt/live/registry.${CLUSTER_DOMAIN}/fullchain.pem \
  -e REGISTRY_HTTP_TLS_KEY=/etc/letsencrypt/live/registry.${CLUSTER_DOMAIN}/privkey.pem           \
docker.io/library/registry:2
```

---------------------------------------------------------------------------------
### Step 08\. Load images into mirror
  1. Pull ocp-release container image
```
oc adm release mirror \
    --from=quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64          \
    --to=registry.${CLUSTER_DOMAIN}/ocp/release                                 \
    --to-release-image=registry.${CLUSTER_DOMAIN}/ocp/release:4.3.0-rc.3-x86_64
```
  + [Example Success Message]    
---------------------------------------------------------------------------------
---------------------------------------------------------------------------------
### Step 09\. Save `oc adm mirror` success output for writing `install-config.yaml`
---------------------------------------------------------------------------------
### Step 10\. Pull Openshift Installer
  1. CMD:    
```
oc adm release extract --command=openshift-install quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64 --to='/root/'
```

---------------------------------------------------------------------------------
### Step 11\. Write `install-config.yaml`
  1. Prep directories - CMD: 
```
mkdir /root/${CLUSTER_DOMAIN} /root/bak ; ls /root/
```
  2. Write yaml - CMD: 
```
cat <<EOF >/root/bak/install-config.yaml
apiVersion: v1
baseDomain: ${DOMAIN_NAME}
imageContentSources:
- mirrors:
  - registry.${CLUSTER_DOMAIN}/ocp/release
  source: quay.io/openshift-release-dev/ocp-release
- mirrors:
  - registry.${CLUSTER_DOMAIN}/ocp/release
  source: quay.io/openshift-release-dev/ocp-v4.0-art-dev
compute:
- name: worker
  replicas: 2
  platform:
    aws:
      type: t2.xlarge
      zones:
      - ${AWS_REGION}a
controlPlane:
  name: master
  replicas: 3
  platform:
    aws:
      type: t2.xlarge
      zones:
      - ${AWS_REGION}a
controlPlane:
metadata:
  name: ${CLUSTER_NAME}
platform:
  aws:
    amiID: ami-e9426288 
    region: ${AWS_REGION}
pullSecret: '`cat /root/.docker/config.json`'
sshKey: '`cat /home/core/.ssh/authorized_keys`'
publish: Internal
EOF
```
  3. Stage install-config.yaml file - CMD: 
```
cp -f /root/bak/install-config.yaml /root/${CLUSTER_DOMAIN}/install-config.yaml 
```
  4. Generate Manifests
```
 cd /root/ && ./openshift-install create manifests --dir=/root/${CLUSTER_DOMAIN}/
```
  5. Provide Commercial AWS Access Key ID & Secret Access Key when prompted
  6. 

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
[Red Hat OpenShift Cluster Manager]:https://cloud.redhat.com/openshift/
[Example Success Message]:../tasks/registry/lib/install-config/oc_adm_success_example.txt
