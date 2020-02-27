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
### Step 04\. Provision ACME Lets Encrypt SSL Certificates
  1. Make letsencrypt directories
  ` mkdir /etc/letsencrypt /var/lib/letsencrypt `
  2. Run letsencrypt container to acquire certificates
  - NOTE: Replace `email_address` & `cluster_domain_name` variables
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
  -m 'email_address' -d 'registry.{cluster_domain_name}'
```

---------------------------------------------------------------------------------
### Step 05\. Build Image Registry Service
  1. Start registry container
```
podman run \
    -d                                                                                       \
    --restart=always                                                                         \
    --name registry                                                                          \
    --volume /etc/letsencrypt:/etc/letsencrypt                                               \
    -e REGISTRY_HTTP_ADDR=0.0.0.0:443                                                        \
    -e REGISTRY_HTTP_TLS_CERTIFICATE=/etc/letsencrypt/live/registry.ocp.domain/fullchain.pem \
    -e REGISTRY_HTTP_TLS_KEY=/etc/letsencrypt/live/registry.ocp.domain/privkey.pem           \
--net=host docker.io/library/registry:2
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
