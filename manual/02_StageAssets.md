# [Task 02](../tasks/artifacts/) - Create/Stage Secret & Configuration Assets
### Prerequisite:
  + [Task 01 Prerequisites]    
--------------------------------------------------------------------------------
### Step 01\. Prepare Working Environment
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
  6. Define target AWS Region    
TODO: @jhultz need to solution path to resolve region discrepencies, raise issue w/ BU & track on this repo or fix
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

  7. Prep directories - CMD: 
```
mkdir -p ${HOME}/${CLUSTER_DOMAIN}/{auth,ssl,data,mirror,images,registry,bak/.docker,.ssh,.aws,.docker} && cd ${HOME}/${CLUSTER_DOMAIN}
```
  8. Write Configuration File
```
cat <<EOF >${HOME}/${CLUSTER_DOMAIN}/environment
export DOMAIN_NAME="${DOMAIN_NAME}"
export CLUSTER_NAME="${CLUSTER_NAME}"
export VPC_NAME="${VPC_NAME}"
export AWS_REGION='us-gov-west-1'
export CERT_EMAIL="admin@${DOMAIN_NAME}"
export CLUSTER_DOMAIN="${CLUSTER_NAME}.${DOMAIN_NAME}"
EOF
```

---------------------------------------------------------------------------------
### Step 02\. Write httpasswd auth file
  1. Create temp image registry user/pass - CMD:
```
htpasswd -Bcb ${HOME}/${CLUSTER_DOMAIN}/auth/htpasswd ${VPC_NAME} ${VPC_NAME}
```
  2. Enter & Confirm user password

TODO: Integrate htpasswd values into prereqs/environment vars
TODO: Evaluate use of `openssl passwd` command to eliminate htpasswd dependency

---------------------------------------------------------------------------------
### Step 03\. Acquire & Stage Pull Secret & AWS Secrets & SSH Public Key
###### Navigate: [Red Hat OpenShift Cluster Manager] > Install > AWS > [User-provisioned Infrastructure]
  1. Click: `Copy pull secret`
  2. CMD: 
```
vi ${HOME}/${CLUSTER_DOMAIN}/bak/.docker/config.json
```
  3. Paste Pull Secret from clipboard && save/close
```
jq -e ".auths += {\"registry.${CLUSTER_DOMAIN}:5000\": {\"auth\": \"$(cat ${HOME}/${CLUSTER_DOMAIN}/auth/htpasswd)\", \"email\": "env.CERT_EMAIL"}}" ${HOME}/${CLUSTER_DOMAIN}/bak/.docker/config.json | jq -c | tee ${HOME}/${CLUSTER_DOMAIN}/.docker/config.json
```
  4. Link for local use:
```
ln -s ${HOME}/${CLUSTER_DOMAIN}/.docker ${HOME}/.docker
```

  5. Write authorized\_key to file
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
### Step 04\. Write AWS Credentials File
>   NOTE:
>   - An AWS Commercial Account is required for deployment   
>   - The AWS User requires the [AdministratorAccess policy] or the [Required AWS Permissions]    
>   - [Permissions Check Code Base]

###### Navigate: [AWS Console] > Top Username Dropdown > My Security Credentials >  [AWS IAM Credentials] > Access Keys

  1. Write aws commercial credential template
```
cat <<EOF >>${HOME}/${CLUSTER_DOMAIN}/.aws/credentials && vi ${HOME}/${CLUSTER_DOMAIN}/.aws/credentials
; https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html
[default]
aws_access_key_id = XXXXXXXXXXXXXXXXXXXX
aws_secret_access_key = zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz
EOF
```
  2. Replace values for `aws_access_key_id` & `aws_secret_access_key` && save/close

---------------------------------------------------------------------------------
### Step 05\. Acquire Binaries {openshift-installer,kubectl,oc}
  1. Pull oc + kubectl CMD:    
```
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux-4.3.5.tar.gz | sudo tar xzvf - --directory /usr/local/bin/ kubectl oc
```
  2. Pull openshift-install - CMD:    
>   On RHCOS
```
sudo podman pull quay.io/openshift-release-dev/ocp-release:4.3.5-x86_64
oc adm release extract --command=openshift-install quay.io/openshift-release-dev/ocp-release:4.3.5-x86_64 --to=${HOME}/${CLUSTER_DOMAIN}/
```
>   On Linux
```
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-install-linux.tar.gz | sudo tar xzvf - --directory ${HOME}/${CLUSTER_DOMAIN}/ openshift-install
```
    
TODO: verify mirror vs quay binary release cadence
TODO: solve for hard coded version numbering

---------------------------------------------------------------------------------
### Step 06\. Acquire Nginx:latest & Registry:2 Image Files
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
[User-provisioned Infrastructure]:https://cloud.redhat.com/openshift/install/aws/user-provisioned
[Red Hat OpenShift Cluster Manager]:https://cloud.redhat.com/openshift/
[Example Success Message]:../tasks/registry/lib/install-config/oc_adm_success_example.txt
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
[AdministratorAccess policy]:https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AdministratorAccess$serviceLevelSummary
[Required AWS Permissions]:https://docs.openshift.com/container-platform/4.3/installing/installing_aws/installing-aws-account.html#installation-aws-permissions_installing-aws-account
[Permissions Check Code Base]:https://github.com/usrbinkat/installer/blob/master/pkg/asset/installconfig/aws/permissions.go
[AWS IAM Credentials]:https://console.amazonaws-us-gov.com/iam/home#/security_credentials
[AWS Console]:https://console.amazonaws-us-gov.com/console/home

