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
  3. Start tmux session for safety
    `tmux`

---------------------------------------------------------------------------------
### Step 03\. Stage Pull Secret
  1. CMD: ` mkdir /root/.docker `
  2. CMD: ` vi /root/.docker/config.json `
  3. Paste Pull Secret from clipboard & save/close

---------------------------------------------------------------------------------
### Step 04\. Export required variables
  1. Export cluster domain name
  - Example: `export CLUSTER_DOMAIN='ocp.cluster.com'`
  2. Export letsencrypt certificate registry email address
  - Example: `export CERT_EMAIL='admin@cluster.com'`

---------------------------------------------------------------------------------
### Step 05\. Provision ACME Lets Encrypt SSL Certificates
  1. Make letsencrypt directories
  ` mkdir /etc/letsencrypt /var/lib/letsencrypt `
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
  -m "${CERT_EMAIL} -d "registry.${CLUSTER_DOMAIN}"
```

---------------------------------------------------------------------------------
### Step 06\. Test Quay.io image pull
  1. Pull ocp-release container image
    `podman pull quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64`

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
