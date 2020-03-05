### Step 00\. Prep Working Environment
  1. Export vpc name
```
export VPC_NAME='cluster'; echo ${VPC_NAME} 
```
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
  6. Prep directories - CMD: 
```
mkdir -p ${HOME}/${CLUSTER_DOMAIN}/{bak,ssh,ssl,data,.aws,.docker} 
cd ${HOME}/${CLUSTER_DOMAIN}
```
  7. Define target AWS Region
```
export AWS_REGION='us-gov-west-1'; echo ${AWS_REGION} 
```
>   Supported Values:
>
>   ap-northeast-1, ap-northeast-2, ap-south-1,    
>   ap-southeast-1, ap-southeast-2, ca-central-1,    
>   eu-central-1, eu-north-1, eu-west-1,    
>   eu-west-2, eu-west-3, sa-east-1,    
>   us-east-1, us-east-2, us-west-1, us-west-2    
>   

  8. Write Configuration File
```
cat <<EOF >${HOME}/${CLUSTER_DOMAIN}/${CLUSTER_NAME}.env
export DOMAIN_NAME="${DOMAIN_NAME}"
export CLUSTER_NAME="${CLUSTER_NAME}"
export VPC_NAME="${VPC_NAME}"
export CERT_EMAIL="admin@${DOMAIN_NAME}"
export CLUSTER_DOMAIN="${CLUSTER_NAME}.${DOMAIN_NAME}"
EOF
```

---------------------------------------------------------------------------------
### Step 01\. Acquire & Stage Pull Secret & AWS Secrets & SSH Public Key
###### Navigate: [Red Hat OpenShift Cluster Manager] > Install > AWS > [User-provisioned Infrastructure]
  1. Click: `Copy pull secret`
  2. CMD: 
```
vi ${HOME}/${CLUSTER_DOMAIN}/.docker/config.json
```
  3. Paste Pull Secret from clipboard && save/close
  4. Link for local use:
```
ln -s ${HOME}/${CLUSTER_DOMAIN}/.docker ${HOME}/.docker
```

###### Navigate: [Red Hat OpenShift Cluster Manager] > Install > AWS > [User-provisioned Infrastructure]
  5. Click: `Copy pull secret`
  6. CMD: 
  7. Write aws credential File
```
cat <<EOF >${HOME}/${CLUSTER_DOMAIN}/.aws/credentials

EOF
```
  8. Write authorized\_key to file
```
ssh-keygen -f ${HOME}/${CLUSTER_DOMAIN}/ssh/id_rsa_${CLUSTER_NAME}
cat ~/.ssh/id_rsa_${CLUSTER_NAME}.pub | tee -a ${HOME}/${CLUSTER_DOMAIN}/authorized_keys
chmod 600 ${HOME}/${CLUSTER_DOMAIN}/authorized_keys
```
---------------------------------------------------------------------------------
### Step 09\. Acquire Binaries {openshift-installer,kubectl,oc}
  1. Pull oc + kubectl CMD:    
```
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux-4.3.1.tar.gz | sudo tar xzvf - --directory /usr/local/bin/ kubectl oc
```
  2. Pull ocp-release container image
```
podman pull quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64
```
  3. Pull openshift-install - CMD:    
```
oc adm release extract --command=openshift-install quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64 --to=${HOME}/${CLUSTER_DOMAIN}/
```

---------------------------------------------------------------------------------
### Step 10\. Write `install-config.yaml`
  2. Write yaml - CMD: 
```
cat <<EOF >/${HOME}/${CLUSTER_DOMAIN}/bak/install-config.yaml
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
      - us-east-1a
controlPlane:
  name: master
  replicas: 3
  platform:
    aws:
      type: t2.xlarge
      zones:
      - us-east-1a
metadata:
  name: ${CLUSTER_NAME}
platform:
  aws:
    amiID: ami-e9426288 
    region: us-east-1
pullSecret: '`cat .docker/config.json`'
sshKey: '`cat authorized_keys`'
publish: Internal
EOF
```
  3. Stage install-config.yaml file - CMD: 
```
cp -f ${HOME}/${CLUSTER_DOMAIN}/bak/install-config.yaml ${HOME}/${CLUSTER_DOMAIN}/data/install-config.yaml
```
  4. Generate Manifests
```
 cd ${HOME}/${CLUSTER_DOMAIN} && ./openshift-install create manifests --dir=${HOME}/${CLUSTER_DOMAIN}
```
  5. Provide Commercial AWS Access Key ID & Secret Access Key when prompted
  6. Rewrite cluster-infrastructure-02-config.yml ` infrastructureName: ` line
```
sed -i "s/\(^  infrastructureName:\)\(.*\)/\1 ${VPC_NAME}/g" \
       /root/${CLUSTER_DOMAIN}/manifests/cluster-infrastructure-02-config.yml
```
  7. Rewrite manifest entries for us-east-1 to AWS Gov region
```
find  . -type f | xargs sed -i  "s/us-east-1/${AWS_REGION}/g"
```
  8. Rewrite manifest value 'infrastructureNAME' with unique tag
```
find  . -type f | xargs sed -i  "s/infrastructureName/${VPC_NAME}/g"
```
  9. Purge `publish: Internal` configuration
```
sed -i '/publish: Internal/d' /root/${CLUSTER_DOMAIN}/manifests/*
```
 10. Remove default ingress configuration & non-applicable cloud credential configs
```
rm \
  /root/${CLUSTER_DOMAIN}/openshift/99_cloud-creds-secret.yaml \
  /root/${CLUSTER_DOMAIN}/openshift/99_role-cloud-creds-secret-reader.yaml \
  /root/${CLUSTER_DOMAIN}/manifests/cluster-ingress-default-ingresscontroller.yaml
```

---------------------------------------------------------------------------------
### Step 11. Write Secrets Configurations
  1. Write `openshift/99_openshift-ingress-operator_cloud-credentials-secret.yaml`
```
cat <<EOF > /root/${CLUSTER_DOMAIN}/openshift/99_openshift-ingress-operator_cloud-credentials-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: cloud-credentials
  namespace: openshift-ingress-operator
data:
  aws_access_key_id: `awk '/aws_access_key_id/ {print $3}' .aws/credentials | base64`
  aws_secret_access_key: `awk '/aws_secret_access_key/ {print $3}' .aws/credentials | base64`
EOF
```
  2. Write `openshift/openshift/99_openshift-machine-api_aws-cloud-credentials-secret.yaml`
```
cat <<EOF > /root/${CLUSTER_DOMAIN}/openshift/99_openshift-machine-api_aws-cloud-credentials-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: aws-cloud-credentials
  namespace: openshift-machine-api
data:
  aws_access_key_id: `awk '/aws_access_key_id/ {print $3}' .aws/credentials | base64`
  aws_secret_access_key: `awk '/aws_secret_access_key/ {print $3}' .aws/credentials | base64`
EOF
```
  3. Write `openshift/99_openshift-image-registry_installer-cloud-credentials-secret.yaml`
```
cat <<EOF > /root/${CLUSTER_DOMAIN}/openshift/99_openshift-image-registry_installer-cloud-credentials-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: installer-cloud-credentials
  namespace: openshift-image-registry
data:
  aws_access_key_id: `awk '/aws_access_key_id/ {print $3}' .aws/credentials | base64`
  aws_secret_access_key: `awk '/aws_secret_access_key/ {print $3}' .aws/credentials | base64`
EOF
```

---------------------------------------------------------------------------------
### Step 12. Backup your ignition config directory
```
cp -rf /root/${CLUSTER_DOMAIN} /root/bak/${CLUSTER_DOMAIN}
```

---------------------------------------------------------------------------------
### Step 13. Create Ignition Configurations
```
cd /root/ && ./openshift-install create ignition-configs --dir=${CLUSTER_DOMAIN}
```

