# Migration Between AWS Accounts Using DataSync

This guide outlines a detailed, step-by-step approach for transferring data between S3 buckets across two separate AWS accounts using AWS DataSync. It covers the creation of IAM roles, configuration of S3 bucket policies, setup of DataSync locations, and execution of the data transfer task.


## Documentation



**Prerequisites**

* Two separate AWS accounts are involved: one as the **Source** and the other as the **Destination**.
* The **source S3 bucket** resides in **Account A**.
* The **destination S3 bucket** is located in **Account B**.
* The **AWS DataSync service** must be enabled in **both accounts**.
* Appropriate **IAM permissions** are required in each account to allow the creation of **roles and policies** necessary for DataSync to function.

### 1. Create IAM Role in Source Account (Account A)

#### a. Open IAM Console:

- Go to IAM console in the source account.

- Click Roles > Create role.

#### b. Configure Trusted Entity:

- Select AWS Service.

- Choose DataSync.

- Click Next.

#### c. After role creation,
- Go to permission > add permission.
- Then create inline policy.

#### d. Attach following Permissions Policy
- AWSDataSyncFullAccess
- AWSDataSyncReadOnlyAccess

#### e. Use the Destination role inline policy

#### d. Save the role ARN for later use.

### Create Bucket and add Bucket Policy in Source Account
- Open the S3 console in Account A.
- Navigate to Create Source Bucket. (give globally unique name).
- Add Bucket policy to the source bucket.



### Create Bucket and add BUcket Policy in Destination Account
- similar steps as used in source account. 

### Create DataSync Destination Location in Account B through CLI
The CLI command for creating the DataSync destination location in Account B are:

```
 aws sts get-caller-identity
```

 ```
 aws datasync create-location-s3 --s3-bucket-arn arn:aws:s3:::destination-bucket-name--subdirectory "/"--s3-storage-class STANDARD--s3-config BucketAccessRoleArn="arn:aws:iam: <DESTINATION_ACCOUNT_ID>:role/<YOUR_ROLE_NAME>" --region <YOUR_REGION>
```
### Create DataSync Task in Destination Account 
- Open DataSync console in Account A.

- Click Create task.

- Select the source and destination locations.

- In source location choose an existing location.

- In Destination location choose  create a new location.

- Then select **Amazon S3** as location type.

- Configure task settings (e.g., task mode, include/exclude filters, data verification).

- Click Create task.

### Start DataSync Task
- Go to DataSync console in Destination Account.

- Select the task and click Start.

- Monitor the task progress and verify the data transfer.

### Post-Migration Verification
- Verify data integrity in the destination S3 bucket. Confirm access and permissions for the migrated data.


