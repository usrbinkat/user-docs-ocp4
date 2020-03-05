# [Task 11](../tasks/registry) - Setup Image Registry Services & Publish Artifacts
### Prerequisite:
  + [08 Build Image Registy Instance]
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
