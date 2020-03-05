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
```

```
```
```
```
```
