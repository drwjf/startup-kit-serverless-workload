# startup-kit-serverless-workload

An example serverless application project:  a RESTful API for a TODO app.

Components include multiple AWS Lambda functions, Amazon API Gateway, and an 
Amazon DynamoDB table.  The project uses the Lambda Node.js 4.3 runtime, and 
generally follows the ECMAScript 6 standard.  The AWS Serverless Application 
Model (SAM) is used to deploy the project. 

LAUNCHING THE APP ON AWS:

To begin your deployment, either download a zip file of the code from GitHub 
or clone the GitHub repository with the command:  

  git clone git@github.com:awslabs/startup-kit-serverless-workload.git

In the AWS Region where you plan to do your deployment, be sure you have an 
existing Amazon S3 bucket in which SAM can put the deployment artifacts, or 
create a new bucket using the following AWS CLI command:  

  aws s3 mb s3://<your-bucket-name>.  


Next, to deploy the project for the first time with SAM, and for each subsequent 
code update, run both of the following AWS CLI commands in order.  For the 
first command, package, replace the s3-bucket argument with the name of your 
S3 bucket.  For the second command, deploy, replace the template-file argument 
with the full path to your output template file.

aws cloudformation package \
--template-file serverless.cfn.yml \
--output-template-file serverless-xfm.cfn.yml \
--s3-bucket startup-kit

aws cloudformation deploy \
--template-file <path-to-file/serverless-xfm.cfn.yml> \
--stack-name StartupKitServerless \
--capabilities CAPABILITY_IAM

TESTING THE APP:

First get the invoke URL of your API.  Do this by going to the API Gateway console, 
selecting StartupKitServerless, then Stages in the left navigation panel, and finally 
Stage in the list of stages.  The invoke URL should now appear at the top of the right 
hand panel.  Begin testing by adding some TODO items using the create API.  This may 
be accomplished using the following command: 

   curl -X POST -H 'Content-Type: application/json' -d '{"todo_id": "1001", "active": true, "description": "What TODO next?"}' https://<invoke-URL-for-your-API>/todo/new

To fetch the active TODO items you created, execute the following command:

   curl https://<invoke-URL-for-your-API>/todo/active

Similar commands can be used to test all of the other API calls.


