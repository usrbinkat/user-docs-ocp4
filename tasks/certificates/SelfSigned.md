# Create Self Signed SSL Certificate Assets
```
cd ~ && mkdir {vpc_name}/ssl ; cd {vpc_name}/ssl
```
```
export DOMAIN_NAME="{{ domain_name }}"
export CLUSTER_DOMAIN="{{ cluster_name }}.{{ domain_name }}"
```
```
openssl req \
  -x509 \
  -nodes \
  -sha256 \
  -days 3650 \
  -newkey rsa:4096 \
  -out    ${CLUSTER_DOMAIN}.crt  \
  -keyout ${CLUSTER_DOMAIN}.key  \
  -subj  "/CN=${CLUSTER_DOMAIN}" \
  -addext "subjectAltName=DNS:registry.{CLUSTER_DOMAIN},DNS:{DOMAIN_NAME},IP:10.0.1.1" 
```
TODO: correct brittle hard coded IP address
```
```
```
```
```
```
