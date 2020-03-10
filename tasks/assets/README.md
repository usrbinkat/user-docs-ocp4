# [Task 02](../tasks/artifacts/) - Create/Stage Secret & Configuration Assets
### Prerequisite:
  + [Task 01 Prerequisites]    
--------------------------------------------------------------------------------
### Step 00\. Prepare Working Environment
  1. Export vpc name
```
export VPC_NAME='myVpcName'; echo ${VPC_NAME} 
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
  5. Export certificate owner email address
```
export CERT_EMAIL="admin@${DOMAIN_NAME}"; echo ${CERT_EMAIL}
```
  6. Prep directories - CMD: 
```
mkdir -p ${HOME}/${CLUSTER_DOMAIN}/{bak,auth,ssl,data,images,.ssh,.aws,.docker} 
cd ${HOME}/${CLUSTER_DOMAIN}
```
  7. Define target AWS Region
```
export AWS_REGION='us-gov-west-1'; echo ${AWS_REGION} 
```
TODO: @jhultz need to solution path to resolve region discrepencies, raise issue w/ BU & track on this repo or fix
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

  8. Write authorized\_key to file
```
ssh-keygen -C "core@${CLUSTER_DOMAIN}" -f ${HOME}/${CLUSTER_DOMAIN}/.ssh/id_rsa_${CLUSTER_DOMAIN}
```
```
cat ${HOME}/${CLUSTER_DOMAIN}/.ssh/id_rsa_${CLUSTER_DOMAIN}.pub | tee -a ${HOME}/${CLUSTER_DOMAIN}/.ssh/authorized_keys
```
```
chmod 600 ${HOME}/${CLUSTER_DOMAIN}/.ssh/authorized_keys
```
```
ln -f .ssh/id_rsa_${CLUSTER_DOMAIN}* ${HOME}/.ssh/
```
---------------------------------------------------------------------------------
### Step 09\. Write AWS Credentials File
>   An AWS Commercial Account is required for deployment   

  1. Write aws commercial credential template
```
cat <<EOF >${HOME}/${CLUSTER_DOMAIN}/.aws/credentials
; https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html
[default]
aws_access_key_id = XXXXXXXXXXXXXXXXXXXX
aws_secret_access_key = zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz
EOF
```
  2. Replace values for `aws_access_key_id` & `aws_secret_access_key` && save/close
```
vi ${HOME}/${CLUSTER_DOMAIN}/.aws/credentials
```
  3. Link to $HOME
```
ln -s ${HOME}/${CLUSTER_DOMAIN}/.aws ${HOME}/
```
---------------------------------------------------------------------------------
### Step 09\. Write httpasswd auth file
  1. Create user/pass - CMD:
  - substitute {yourUserName} for your username (eg: regadmin)
```
htpasswd -Bc ${HOME}/${CLUSTER_DOMAIN}/auth/htpasswd {yourUserName}
```

---------------------------------------------------------------------------------
### Step 09\. Acquire Binaries {openshift-installer,kubectl,oc}
  1. Pull oc + kubectl CMD:    
```
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux-4.3.1.tar.gz | sudo tar xzvf - --directory /usr/local/bin/ kubectl oc
```
  2. Pull ocp-release container image
```
sudo podman pull quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64
```
  3. Pull openshift-install - CMD:    
```
oc adm release extract --command=openshift-install quay.io/openshift-release-dev/ocp-release:4.3.0-rc.3-x86_64 --to=${HOME}/${CLUSTER_DOMAIN}/
```

---------------------------------------------------------------------------------
### Step 09\. Acquire Binaries {openshift-installer,kubectl,oc}
  1. Pull & Export registry image
```
 sudo podman pull docker.io/library/registry:2
```
```
 sudo podman save -o ${HOME}/${CLUSTER_DOMAIN}/images/docker-registry2-image.tar registry:2 
```
  2. Pull & Export registry image
```
 sudo podman pull docker.io/library/nginx:latest
```
```
 sudo podman save -o ${HOME}/${CLUSTER_DOMAIN}/images/docker-nginxlatest-image.tar nginx:latest
```

---------------------------------------------------------------------------------
### Next Step: Create/Acquire SSL Certificates for temporary image mirror
###### Options:
  A. [Generate Self Signed Certificate]    
  B. [Generate Let's Encrypt Certificates]    
  C. [Acquire Organization Issued Certificates]    

--------------------------------------------------------------------------------
[Task 01 Prerequisites]:manual/01_Prerequisites.md
[Task 02 Stage Assets]:manual/02_StageAssets.md
[Task 03 Certificates]:manual/03_Certificates.md
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
