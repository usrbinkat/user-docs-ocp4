
---------------------------------------------------------------------------------
### Step 05\. Provision ACME Lets Encrypt SSL Certificates
  1. Make letsencrypt directories
```
mkdir -p ${HOME}/${CLUSTER_DOMAIN}/ssl ; cd ${HOME}/${CLUSTER_DOMAIN}/ssl
```
  2. Run letsencrypt container to acquire certificates    
```
podman run                                                           \
    --rm                                                             \
    --net=host                                                       \
    --privileged                                                     \
    --volume ${HOME}/${CLUSTER_DOMAIN}/ssl:/etc/letsencrypt          \
    --volume ${HOME}/${CLUSTER_DOMAIN}/ssl:/var/lib/letsencrypt      \
  docker.io/certbot/certbot certonly                                 \
    --agree-tos                                                      \
    --standalone                                                     \
    --non-interactive                                                \
  -m "${CERT_EMAIL}" -d "registry.${CLUSTER_DOMAIN}"
```
TODO: Complete Section
TODO: Align all certificate asset naming convention & local & registry host paths
