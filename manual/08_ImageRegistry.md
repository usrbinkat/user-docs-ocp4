# [Task 08](../tasks/registry) - Setup Image Registry
### Prerequisite:
  + [07 Setup IAM Roles]
--------------------------------------------------------------------------------
### Step 01\. Create IAM Policies `master`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Policies]
  1. Click: `Create Policies`
  2. Select "json" Tab 
  3. Erase default content
  4. Fill in with the following content
```
```
  5. Provide `Name` value: `{vpc_name}-master-profile`
  6. Click: `Create Policy`

---------------------------------------------------------------------------------
### Step 02\. Create IAM Policy `worker`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Policies]
  1. Click: `Create Policy`
  2. Select "json" Tab 
  3. Erase default content
  4. Fill in with the following content
```
```
  5. Provide `Name` value: `{vpc_name}-worker-profile`
  6. Click: `Create Policy`

---------------------------------------------------------------------------------
### Step 03\. Create IAM Roles `worker` & `master`
###### Navigate: [AWS Console] > [EC2] > Security, Identity & Compliance > [IAM] > [Roles]
###### - For both profiles `{vpc_name}-master-profile` & `{vpc_name}-worker-profile` tag per the following
  1. Click: `Create Role`
  2. Select "EC2" under heading "Choose a use case" 
  3. Click: `Next: Permissions`
  4. Filter for term `{vpc_name}-master-profile` & Check left selection box
  5. Click: `Next: Tags`
  6. Fill in with the following Key:Value format
>   Role Tags Table:
>
>   | Role                      | Key  | Value                       |
>   |---------------------------|-----:|:----------------------------|
>   | {vpc_name}-master-profile | Name | `{vpc_name}-master-profile` |
>   | {vpc_name}-worker-profile | Name | `{vpc_name}-worker-profile` |

  7. Click: `Next: Review`
  8. Fill in Role Name: eg `{vpc_name}-master-profile`
  9. Click: `Create Role`
    
###### Verify: 'Lower Panel > Permissions > Policy Name > Expand all' should list all 3 new policies
        

--------------------------------------------------------------------------------
### Step 04\. Create IAM User Role
###### Navigate: [AWS Console] > [EC2] > [IAM] > [Users] > Lower Pannel > Permissions
###### - For each of the 3 IAM User Roles in the `IAM User Role Index` below, do the following:

  1. Filter for your aws administrator user name & click on user
  2. Click: `Add inline policy`
  3. Click: `JSON` Tab
  4. Erase default content
  5. Fill in with associated JSON content
  6. Click: `Create Policy`
  7. Provide `Name` value
  8. Click: `Create Policy`


---------------------------------------------------------------------------------
### Next Steps:
  + [09 Create Bootstrap Node]
--------------------------------------------------------------------------------
[07 Setup IAM Roles]:../manual/07_IAMRoles.md
[09 Create Bootstrap Node]:../manual/09_Bootstrap.md
[EC2]:https://console.amazonaws-us-gov.com/ec2/home
[IAM]:https://console.amazonaws-us-gov.com/iam/home
[Users]:https://console.amazonaws-us-gov.com/iam/home#/users
[Roles]:https://console.amazonaws-us-gov.com/iam/home#/roles
[Policies]:https://console.amazonaws-us-gov.com/iam/home#/policies
[AWS Console]:https://console.amazonaws-us-gov.com/console/home
