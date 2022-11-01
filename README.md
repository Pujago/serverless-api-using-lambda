# serverless-api-using-lambda
Sample how to deploy serverless api using lambda, api gateway and dynamodb and use custom authorizer to control access

# Create Dynamo db table 
Table name: http-crud-tutorial-items
Primary key: id

# Create IAM role "serverless-crud-api-role"
Grant access to managed policy AWSLambdaBasicExecutionRole and create custom role to grant access to dynamodb 

# Create node js lambda function "serverless-crud-function" in lambda console and copy code from serverless-crud-function.js

# Create node js lambda function "serverless-crud-function" in lambda console and copy code from serverless-crud-function.js
