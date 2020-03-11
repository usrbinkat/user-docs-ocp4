# [Task 03](../tasks/manifests/) - Write Manifests & Create Ignition Config
### Prerequisite:
  + [Task 02 Stage Assets]    
---------------------------------------------------------------------------------
### Step 01\. Write `install-config.yaml`
  2. Confirm Directory Location - CMD: 
```
cd ${HOME}/${CLUSTER_DOMAIN}
```
  2. Write yaml - CMD: 
```
cat <<EOF >/${HOME}/${CLUSTER_DOMAIN}/bak/install-config.yaml
apiVersion: v1
additionalTrustBundle: |
`awk '{printf "  %s\n", $0}' < ${HOME}/${CLUSTER_DOMAIN}/ssl/${CLUSTER_DOMAIN}.crt`
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
sshKey: "`cat .ssh/authorized_keys`"
publish: Internal
EOF
```
  3. Stage install-config.yaml file - CMD: 
```
cp -f ${HOME}/${CLUSTER_DOMAIN}/bak/install-config.yaml ${HOME}/${CLUSTER_DOMAIN}/data/install-config.yaml
```
  4. Generate Manifests
```
 cd ${HOME}/${CLUSTER_DOMAIN} && ./openshift-install create manifests --dir=${HOME}/${CLUSTER_DOMAIN}/data
```
  5. Reassign cluster random name to VPC\_NAME
```
export idRand=$(awk -F'[-]' '/infrastructureName/{print $2}' ${HOME}/${CLUSTER_DOMAIN}/data/manifests/cluster-infrastructure-02-config.yml)
```
```
sed -i "s/${CLUSTER_NAME}-${idRand}/${VPC_NAME}/g" ${HOME}/${CLUSTER_DOMAIN}/data/manifests/cluster-infrastructure-02-config.yml
```
```
find . -type f | xargs sed -i  "s/${CLUSTER_NAME}-${idRand}/${VPC_NAME}/g"
```
  6. Rewrite cluster-infrastructure-02-config.yml ` infrastructureName: ` line
```
sed -i "s/\(^  infrastructureName:\)\(.*\)/\1 ${VPC_NAME}/g" ${HOME}/${CLUSTER_DOMAIN}/data/manifests/cluster-infrastructure-02-config.yml
```
  7. Rewrite manifest entries for us-east-1 to AWS Gov region
```
find  ${HOME}/${CLUSTER_DOMAIN}/data/ -type f | xargs sed -i  "s/us-east-1/${AWS_REGION}/g"
```
  8. Rewrite manifest value 'infrastructureNAME' with unique tag
```
find  ${HOME}/${CLUSTER_DOMAIN}/data/ -type f | xargs sed -i  "s/infrastructureName/${VPC_NAME}/g"
```
TODO: Break off `disconnected` vs `air gapped` steps .. the following .9 .10
only intended for disconnected, not air gapped env
  9. Purge `publish: Internal` configuration
```
sed -i '/publish: Internal/d' ${HOME}/${CLUSTER_DOMAIN}/data/manifests/*
```
 10. Remove default ingress configuration & non-applicable cloud credential configs
```
rm \
  ${HOME}/${CLUSTER_DOMAIN}/data/openshift/99_cloud-creds-secret.yaml \
  ${HOME}/${CLUSTER_DOMAIN}/data/openshift/99_role-cloud-creds-secret-reader.yaml \
  ${HOME}/${CLUSTER_DOMAIN}/data/manifests/cluster-ingress-default-ingresscontroller.yaml
  
```

---------------------------------------------------------------------------------
### Step 11. Write Secrets Configurations
  1. Write `openshift/99_openshift-ingress-operator_cloud-credentials-secret.yaml`
```
cat <<EOF > ${HOME}/${CLUSTER_DOMAIN}/data/openshift/99_openshift-ingress-operator_cloud-credentials-secret.yaml
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
  2. Write `openshift/data/openshift/99_openshift-machine-api_aws-cloud-credentials-secret.yaml`
```
cat <<EOF > ${HOME}/${CLUSTER_DOMAIN}/data/openshift/99_openshift-machine-api_aws-cloud-credentials-secret.yaml
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
  3. Write `data/openshift/99_openshift-image-registry_installer-cloud-credentials-secret.yaml`
```
cat <<EOF > ${HOME}/${CLUSTER_DOMAIN}/data/openshift/99_openshift-image-registry_installer-cloud-credentials-secret.yaml
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
cp -rf ${HOME}/${CLUSTER_DOMAIN}/data ${HOME}/${CLUSTER_DOMAIN}/bak/$(date '+%Y%m%d%H%M')-data
```

---------------------------------------------------------------------------------
### Step 13. Create Ignition Configurations
```
 ./openshift-install create ignition-configs --dir=${HOME}/${CLUSTER_DOMAIN}/data
```

--------------------------------------------------------------------------------
### Next Steps:
  + [Task 04 Setup AWS VPC]
--------------------------------------------------------------------------------
[Task 01 Prerequisites]:manual/01_Prerequisites.md
[Task 02 Stage Assets]:manual/02_StageAssets.md
[Task 03 Manifests & Ignition Config]:manual/03_ManifestIgnition.md
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
[Generate Self Signed Certificate]:../tasks/certificates/SelfSigned.md
[Generate Let's Encrypt Certificates]:../tasks/certificates/LetsEncrypt.md
[Acquire Organization Issued Certificates]:../tasks/certificates/OrganizationIssued.md

