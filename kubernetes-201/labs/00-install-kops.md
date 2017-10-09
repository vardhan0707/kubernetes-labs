# Lab 1

## Exercise

### Installing KOPS

KOPS has a dependency on you having the AWSCLI installed and configured and kubectl.

### Install awscli

### Setup a S3 bucket

1: Setup an S3 Bucket

I used the aws cli to create an S3 Bucket:

aws s3 mb s3://my-kops-configs

**Note:** you can use the bucket to store more than one cluster state
