# Boto

Make sure you ahve  python 2.7 installed  ( mac has it default)

```
pip install boto3 ( if pip is not installed then follow :)

xcode-select --install
sudo easy_install pip
sudo pip install --upgrade pip
```

Assume role

```
import boto3
#cteate security token service object
sts_client = boto3.client('sts')
assumedRoleObject = sts_client.assume_role(
    RoleArn="arn:aws:iam::157065524616:role/awstraining-role-tbd",
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



