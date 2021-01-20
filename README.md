# Provisioning the infra using AWS Cloud Formation Templates

---
Introduction
---
These CFT's are used to provision the Infra automatically on AWS where the infastructure is split into modules with the nested scripts and the 
master script pulls up all the nested scripts.On a high level these CFT's basically creates a VPC with a private and public subnets and two EC2 instances (public and private each)

---
Scripts:
---
* master-script: invokes all nested scripts that creates and deploys the infrastructure components
* vpc: creates 1VPC and 2 subnets –1 public and 1 private
* ec2_private: creates 1 private ec2 instance which you can consider this as the app insatnce.
* bastion_ec2_public: Creates 1 public ec2 instance which you can consider this as the bastion host
 
---
Prerequisites
---
To create the CloudFormation stack in your AWS target account , you may need the following:

*	An account with sufficient privilege to run/launch CloudFormation stacks
*	The yaml scripts provided with this repo (you may store these scripts in a git repo or into AWS S3 bucket)
*	AWS CLI needs to be configured and installed in your machine. Refer below for AWS CLI installtion
  https://docs.aws.amazon.com/polly/latest/dg/setup-aws-cli.html
      
---      
How it Works:
---
*	The CLI command references a master script which is stored in an git repo/S3 bucket.  The master then calls the nested scripts from the specified git-repo/
  S3 bucket
*	from your AWS CLI run the below command to launch this stack
    
$ aws cloudformation create-stack --stack-name <name-of-your-stack> --template-url https://s3.us-east-1.amazonaws.com/name-of-your-bucket/master-script.yml --capabilities CAPABILITY_NAMED_IAM --parameters ParameterKey=EnvironmentName,ParameterValue=$env ParameterKey=AvailabilityZone1,ParameterValue=$your-AZ-ex-us-east-1a  ParameterKey=KeyName,ParameterValue=$your-key  ParameterKey=TemplateBucket,ParameterValue=$your-S3-bucket-name

    
    
 The master script requires the following parameters:
*	Environment Name: (dev, test, or prod, for example)
*	Availability Zone: AZ for your VPC and subnets
*	Stack Name: To follow the progress of the stack build through the console or the CLI statements
*	Template URL: S3 Bucket URL where the master is being stored (or) git repo URL where the master script/nested scripts are being stored
*	Key Name: EC2 Key Pair used to create new instances
*	Template Bucket: S3 Bucket where nested scripts are stored (you would only need this parameter if you stroing scripts in S3 bucket. (Note:if you are using git you 	would not need this parameter)
  
  



        
