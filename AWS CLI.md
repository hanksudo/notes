# AWS CLI Note

[aws-cli - examples](https://github.com/aws/aws-cli/tree/develop/awscli/examples)

## Getting started

```bash
brew install awscli
aws configure
aws configure get region
```

## STS - AWS Security Token Service

```bash
# whoami
aws sts get-caller-identity
```

## IAM

```bash
aws iam list-users
aws iam list-users --output table

# List keys
aws iam list-access-keys

# List users by ARN
aws iam list-users --output json | jq -r '.Users[].Arn'
```

## EC2

```bash
# Elastic IPs
aws ec2 describe-addresses | jq '.'
aws ec2 describe-addresses | jq '.Addresses[] | {PublicIp,PrivateIpAddress,Tags}'

aws ec2 describe-instances | jq '.Reservations[].Instances[] | {InstanceId,InstanceType}'

# List all of your  instances that are currently stopped, and the reason for the stop
aws ec2 describe-instances --filters Name=instance-state-name,Values=stopped --output json | jq -r '.Reservations[].Instances[].StateReason.Message'
```

## S3

```bash
aws s3 ls
aws s3 ls $BUCKET_NAME

# Create bucket
aws s3api create-bucket --bucket $BUCKET --region ap-southeast-1

# Get bucket's region
aws s3api get-bucket-location --bucket $BUCKET

# Count objects
aws s3api list-objects --bucket $BUCKET --output json --query "[length(Contents[])]"

# Force delete bucket (even not empty)
aws s3 rb s3://$BUCKET --force --region `aws s3api get-bucket-location --bucket BUCKETNAME | jq '.LocationConstraint' -r`

# Recursively copy a directory and its subfolders from your PC to Amazon S3
aws s3 cp my_folder s3://$BUCKET -- recursive [--region us-west-2]

# List the sizes of an S3 bucket and its contents
aws s3api list-objects --bucket $BUCKET --output json --query "[sum(Contents[].Size), length(Contents[])]"

# Move S3 bucket to different location
aws s3 sync s3://oldbucket s3://newbucket --source-region us-west-1 --region us-west-2

# Remove file with specify extension name
s3cmd --recursive ls s3://$BUCKET | awk '{ print $4 }' | grep ".DS_Store" | xargs s3cmd del
s3cmd --recursive ls s3://$BUCKET | awk '{ print $4 }' | grep ".keep" | xargs s3cmd del

aws s3 rm s3://$BUCKET --recursive --exclude "*.jpg" --exclude "*.png"  --exclude "*.mp4" --exclude "*.m4a" --exclude "*.txt" --exclude "*.mp3"

# Update static files
aws s3 sync static/data s3://$BUCKET/data --delete --region ap-northeast-1 --acl public-read --cache-control max-age=604800
```

## ECS

```zsh
# Force new deployment
aws ecs update-service --cluster <CLUSTER NAME> --service <SERVICE NAME> --force-new-deployment
```

## ECR

```zsh
# Add tag
MANIFEST=$(aws ecr batch-get-image --repository-name <REPO NAME> --image-ids imageTag=latest --output json | jq --raw-output --join-output '.images[0].imageManifest')
aws ecr put-image --repository-name <REPO NAME> --image-tag new-tag --image-manifest "$MANIFEST"

# Remove tag
aws ecr batch-delete-image --repository-name <REPO NAME> --image-ids imageTag=new-tag
```

## References

- [Best Practices for using Elastic IPs (EIP) and Availability Zones - RightScale Technical Support](http://support.rightscale.com/09-Clouds/AWS/02-Amazon_EC2/Designing_Failover_Architectures_on_EC2/00-Best_Practices_for_using_Elastic_IPs_(EIP)_and_Availability_Zones/)
