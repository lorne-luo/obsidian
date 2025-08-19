## Current implementation

Due to the py312 container can’t get the IAM role of the host ec2, currently we split two jobs:

* The first job run without container to get DB credential, and encoded the credentials as base64 encoding and pass to next job(Github disallow pass secret directly)
* The second job run with py312 contianer, run the alembic command with the credential received.

## The drawback of current implementation:

* Not safe to pass credential in unencrypted encoding
* When review require enabled in Github env settings, we have to manually approve twice to process each db migration

## Potential nice solution

We can a try get AWS accesskey via [EC2 metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) and pass into the py312 container to let container get same IAM role with the host EC2, then the two job can be merged into one

```
# get access key id from metadata server
curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/ | jq -r .AccessKeyId
```
