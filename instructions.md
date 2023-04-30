# How to use

## Pre-requisite
* Install aws cli to issue aws commands
* Configure aws profile, run aws configure. I have used a profile called 'fiverr', you may want to use a different profile name. 


All resources are region based. If you want to deploy to another region. The common stack and frontend/backend stack needs to be in the same region.

## Common resources are not environment specific. If you want to change the CIDR block you may do so in the mappings. You choose to give the vpc a different name by supplying a VPCName parameter. SSM Parameters are used to store resource ids or arns for the common stack

1. Deploy common resources
* aws cloudformation deploy --profile fiverr --region us-east-1 --template common.template.yaml --stack-name common-network-resources

## Replace the env variable with staging, test or prod. If you are redeploying to another region, either change the env name or delete the s3 bucket for static object.

2. Deploy frontend resources

* aws cloudformation deploy --profile fiverr --region us-east-1 --template frontend.template.yaml --stack-name dev-frontend-resources-stack --parameter-overrides env=dev

* aws cloudformation deploy --profile fiverr --region us-east-1 --template frontend.template.yaml --stack-name test-frontend-resources-stack --parameter-overrides env=test

## Almost all services in the backend are deployed within a VPC. You may want to supply another parameter 'dbPassword' this will keep the password out of any github branch. When specifying a new environment, ensure the config mapping exists in the backend.template.yaml.

3. Deploy backend resources

* aws cloudformation deploy --profile fiverr --region us-east-1 --template backend.template.yaml --stack-name dev-backend-resources-stack --capabilities CAPABILITY_NAMED_IAM --parameter-overrides env=dev

* aws cloudformation deploy --profile fiverr --region us-east-1 --template backend.template.yaml --stack-name test-backend-resources-stack --capabilities CAPABILITY_NAMED_IAM --parameter-overrides env=test dbPassword=AbraCadabra123