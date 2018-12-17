# aws-mfa

# AWS MFA

The following describes how to automate the MFA process.

## Required installed dependencies:  

Versions in use at this time:  aws-cli/1.15.50 Python/3.7.0 botocore/1.10.49

More recent versions are fine.

1. Requires OATHTOOL to be installed
1. Requires the AWS-CLI
 * Python: Boto3 is required

## Installation

### ARN

Get your MFA ARN from the AWS Console.  This ARN identifies you as an MFA User to AWS.

Find your MFA ARN IN the UI via Services -> IAM and searching for your user name.

#### Create file to store your ARN locally

1. Create the file '~/.aws/dev.arn' and save the ARN in the file
 * Example: `arn:aws:iam::027277054295:mfa/<user-name>`

### MFA Secret Key

Your MFA Secret Key is only accessible from the UI, and only once when it is initially created.  If you have already registered a device and you didn't save your secret key then you will need to recreate it and store the 60+ character key.

Create the MFA Secret Key via the your User Summary / Tab: Security Credentials, editing the "Assigned MFA Device".  On the popup windows with the QR code, be sure to click on 'Show secret key for manual configuration' and store the secret key in a secure location.

1. Create the file '~/.aws/dev.mfa' and save the Secret in the file
 * Example: UZSH2RMWJMF4TGRHGCTTMGVFJT4W6WUYWNIWT7QXBEFZ6V3QA3WKPV63W37J54PP

### Setup AWS Credentials

The aws-mfa script will set & overwrite the [default] properties that is located in your ~/.aws/credentials file.  The script requires your personal 'Access Key', to bootstrap the MFA process.

If you have Access Key credentials already in the [default] profile, move them to another profile like '[dev]'.  Your AWS credentials file should look like the following.

```
[default]
aws_access_key_id =
aws_secret_access_key =
aws_session_token =

[dev]
aws_access_key_id = < Your Personal Dev Key>
aws_secret_access_key =  < Your Personal Dev Key>
```

### Authenticating

To create credentials, after running an init.sh you can run `aws-mfa.sh ${profile}`

For example: `aws-mfa.sh dev`

NOTE: Be sure that any AWS environment variables have not been set locally in the shell.  The script might fail if the other credentials are set and override the AWS credentials file.  Places to look for automatically set environment variables include, .bashrc, .bash_profile, .AWS / Config file
