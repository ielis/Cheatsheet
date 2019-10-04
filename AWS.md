# AWS

 Commands used to work with AWS S3 storage.

## Install AWS CLI

Following [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html).

**Mac OS**

```bash
pip3 install awscli --upgrade --user

## ---- Adjust & paste following to your $HOME/.bashrc file ---- ##
# Set path to awscli binaries, they were installed to '/Users/${USER}/Library/Python/3.7/bin' last time
AWSCLI_HOME=/Users/${USER}/Library/Python/3.7/bin

#Add AWS CLI to PATH
AWSCLI_HOME=${AWSCLI_HOME}
export PATH=\$PATH:\$AWSCLI_HOME
# enable CLI autocompletion
complete -C ${AWSCLI_HOME}/aws_completer aws
## ------------------------------------------------------------- ##

# Test successful installation
aws --version 
# should show something like 'aws-cli/1.16.144 Python/3.7.2 Darwin/18.5.0 botocore/1.12.134'
```

**Linux**

```bash
pip3 install --user --upgrade awscli
```



## Configure credentials

Sign into AWS console and select **IAM** service. First you need to create a policy, here for reading/writing/listing objects in a specified bucket.

### Create Policy

This policy allows read/write/list of objects within a specified bucket (`supercool` here). Paste this

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListObjectsInBucket",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::supercool"
            ]
        },
        {
            "Sid": "AllObjectActions",
            "Effect": "Allow",
            "Action": "s3:*Object",
            "Resource": [
                "arn:aws:s3:::supercool/*"
            ]
        }
    ]
}
```

### Create Group

Click on *Groups* link on the left part and create a new group in these steps:

- Set Group Name - e.g. `TheGroup`
- Attach Policy - e.g. the one created in previous step
- Confirm

### Create User

Click on *Users* and then on *Add user* button

- Set name - e.g. `johndoe`
- Check programmatic access, or console
- Add user to group (e.g one from above)
- Add tags (optional)
- Review and confirm by `Create user`
- **IMPORTANT** - now you have to record *AWS Access Key ID* and *AWS Secret Access Key*. In particular, the secret access key **cannot** be retrieved later!

## Configure CLI access

Use:

- AWS Access Key ID
- AWS Secret Access Key
- Default region name - `us-east-2`
- Default output format - json

```bash
# Run and provide AWS Access Key ID, 
aws configure
```

## Create bucket

Create bucket `supercool` if you have privileges

```bash
aws s3api create-bucket --bucket supercool --region us-east-2 --create-bucket-configuration LocationConstraint=us-east-2
```

