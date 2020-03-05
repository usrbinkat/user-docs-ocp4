
  2. Export vpc name
```
export VPC_NAME='cluster'; echo ${VPC_NAME} 
```
  3. Export cluster name
```
export CLUSTER_NAME='ocp'; echo ${CLUSTER_NAME} 
```
  4. Export cluster domain
```
export DOMAIN_NAME='cluster.com'; echo ${DOMAIN_NAME}
```
  5. Export cluster domain name
```
export CLUSTER_DOMAIN="${CLUSTER_NAME}.${DOMAIN_NAME}"; echo ${CLUSTER_DOMAIN}
```
  6. Export letsencrypt certificate registry email address
```
export CERT_EMAIL="admin@${DOMAIN_NAME}"; echo ${CERT_EMAIL}
```

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
