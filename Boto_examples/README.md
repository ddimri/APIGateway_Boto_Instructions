# Boto

Make sure you ahve  python 2.7 installed  ( mac has it by default)

```
pip install boto3 - if pip is not installed then follow these commands:

xcode-select --install
sudo easy_install pip
sudo pip install --upgrade pip
```

Assume role from lets say AWS Account 123456789012 to 0987654321012

Create a service account lets say "terraform" in AWS 123456789012
Attach below policy to the service account terraform that you have created in 123456789012

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1424898647000",
            "Effect": "Allow",
            "Action": [
                "sts:AssumeRole"
            ],
            "Resource": [
                "arn:aws:iam::0987654321012:role/terraform-role"
            ]
        }
    ]
}
```
Create a role name "terraform-role" in 0987654321012
Attach S3 Read only access policy to terraform-role
Trust 123456789012 account

Run the 

```
import boto3
#cteate security token service object
sts_client = boto3.client('sts')
assumedRoleObject = sts_client.assume_role(
    RoleArn="arn:aws:iam::0987654321012:role/terraform-role",
    RoleSessionName="AssumeRoleSession1"
)

#temp creds are issues by the role
credentials = assumedRoleObject['Credentials']

# connection to Amazon S3
s3_resource = boto3.resource(
    's3',
    aws_access_key_id = credentials['AccessKeyId'],
    aws_secret_access_key = credentials['SecretAccessKey'],
    aws_session_token = credentials['SessionToken'],
)
# credentials to access your S3 buckets.
for bucket in s3_resource.buckets.all():
    print(bucket.name)
```


s3 --> Create s3 Bucket

```
session = boto3.session.Session(profile_name='deepakprasad')
s3 = session.resource('s3')
s3.create_bucket(Bucket='ddimri-test-buekt')


s3 = boto3.client('s3')
s3.create_bucket(Bucket='ddimri-test-bucket')

```

S3--> store an object in s3

```
bucket_name='ddimri-test-buekt'
file_name='whatever'
file_data='somemetadata'
s3.Object(bucket_name,file_name).put(Body=file_data)

```
s3--> list buckets

```
bucket = s3.Bucket(bucket_name)
for key in bucket.objects.all():
	print str(key.key)

```
s3--> delete object from s3 bucket

```
s3 = boto3.client('s3')
s3.delete_object(Bucket='ddimri-test-buekt', Key='Declaration.docx')
```

s3--> delete bucket itself
```
  
s3 = boto3.resource('s3')
bucket = s3.Bucket('ddimri-test-buekt')
bucket.delete()
```

ec2 --> stop instance

```
ec2 = boto3.client('ec2', region_name='us-west-2')
ec2.stop_instances(InstanceIds=['i-076875e337853a513'])
ec2.terminate_instances(InstanceIds=['i-076875e337853a513'])
```



