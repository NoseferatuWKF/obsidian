s3
```bash
# copy all files in folder
aws s3 cp s3://Bucket/Folder LocalFolder --recursive
```

ecr
```bash
# pipe ecr credentials to docker
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```