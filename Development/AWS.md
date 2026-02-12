# AWS

## Install AWS CLI

```bash
sudo snap install aws-cli --classic
```

## Configure

[Configuration and credential file settings in the AWS CLI](https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-files.html)

Create a `config` and `credentials` file in `~/.aws`.

`~/.aws/config`
```
[profile user1]
region=us-east-1

```

`~/.aws/credentials`
```
[default]
aws_access_key_id=ACCESSKEYID
aws_secret_access_key=SECRETACCESSKEY

```

## Test

```bash
aws sts get-caller-identity
```
## Organization Units and accounts

create member accounts, add the following policy

ManagementAccountTrustPolicy

Trusted entities
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::733979974522:root"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

AdministratorAccess

then in `~/.aws/config` add the profile

```
[default]
region=us-east-1
output=json


[profile infra]
aws_account_id=140873264336
role_arn=arn:aws:iam::140873264336:role/ManagementAccountTrustPolicy
source_profile=default
role_session_name=infra-admin

```
