# Step-by-Step Guide to Use AWS CLI for Copying S3 Data Between Accounts
## Step 1: Configure IAM Permissions
### Ensure you have an IAM user or role in each account with the appropriate permissions.
  - Source Account (Account A):
    -->Ensure the IAM user/role has s3:GetObject and s3:ListBucket permissions on the source bucket.
  - Destination Account (Account B):
    -->Ensure the IAM user/role has s3:PutObject permission on the destination bucket.

Example Policy for Account A (Source):
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::<source-bucket-name>",
        "arn:aws:s3:::<source-bucket-name>/*"
      ]
    }
  ]
}

```

Example Policy for Account B (Destination):
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::<destination-bucket-name>/*"
    }
  ]
}

```
## Step 2: Set Up Cross-Account Bucket Policies
Configure the bucket policies for cross-account access.

Bucket Policy for the Source Bucket (Account A):
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<Account-B-ID>:root"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::<source-bucket-name>/*"
    }
  ]
}

```
Bucket Policy for the Destination Bucket (Account B):
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<Account-A-ID>:root"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::<destination-bucket-name>/*"
    }
  ]
}

```
## Step 3: Configure AWS CLI Profiles
Make sure the AWS CLI is configured for both accounts.

- Configure Account A (Source Account):
```
aws configure --profile account-a
```
Enter the `AWS Access Key`, `Secret Access Key`, `region`, and `output format` for Account A.
- Configure Account B (Destination Account):
```
aws configure --profile account-b
```
Enter the credentials for Account B.

## Step 4: Use AWS CLI to Copy Data
Choose which account you want to use to initiate the copy. You can run the command from Account B (using its credentials) if Account B has the necessary permissions on Account A’s source bucket.

- Method 1: Using Account A's CLI to Copy Data:
```
aws s3 cp s3://<source-bucket-name>/ s3://<destination-bucket-name>/ --recursive --profile account-a
```
- Method 2: Using Account B's CLI to Copy Data: Ensure Account B has s3:GetObject permission on Account A's bucket.
```
aws s3 cp s3://<source-bucket-name>/ s3://<destination-bucket-name>/ --recursive --profile account-b
```
Note: The --recursive flag ensures that all objects, including subdirectories, are copied.

## Step 5: Verify Data Transfer
Check that the data has been copied successfully:

List Objects in the Destination Bucket:
```
aws s3 ls s3://<destination-bucket-name>/ --profile account-b
```
Count the Number of Files:
```
aws s3 ls s3://<destination-bucket-name>/ --recursive --profile account-b | wc -l

```

## Step 6: Remove Temporary Permissions (Optional)
Once the transfer is complete, consider removing the cross-account permissions from the bucket policies to maintain security.

## Additional Tips
Large Data Transfers: For large data transfers, consider using the aws s3 sync command, which only copies new or updated files.
```
aws s3 sync s3://<source-bucket-name>/ s3://<destination-bucket-name>/ --profile account-b
```

