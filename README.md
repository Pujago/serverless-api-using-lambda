### Serverless-api-using-lambda
### Sample how to deploy serverless api using lambda, api gateway and dynamodb and use custom authorizer to control access

# Create Dynamo db table 
Table name: product-items
Primary key: id

# Create IAM role "crud-server-api-full-privilege-role"
Grant access to managed policy AWSLambdaBasicExecutionRole and create custom role to grant access to dynamodb 

crud-server-api-full-privilege-role


# Create Lambda functions

1. Create node js lambda function "serverless-crud-function" in lambda console and copy code from serverless-crud-function.js
2. Create node js lambda function "lambda-auth-basic-function" in lambda console and copy code from lambda-auth-basic.js
3. Create node js lambda function "lambda-auth-iam-function" in lambda console and copy code from lambda-auth-iam.js

# Create API Gateway

1. Create http api gateway: http-serverless-api
2. create routes: GET{items}, GET/{items}/{id}, POST{items}, DELETE/{items}

### Use CURL to add the api-gateway url and invoke POST end point
#### Test without headers

export INVOKE_URL="<URL OF APIGATEWAY>"
  
### Invoke PUT end point    
curl -v -X "PUT" -H "Content-Type: application/json" -d "{\"id\": \"123\", \"price\": 12345, \"name\": \"myitem\"}" $INVOKE_URL/items    
  
### Invoke get end point
curl -s GET $INVOKE_URL/items | js-beautify 
  
### Notice it is publicly available
  
  
## Adding Custom authorizer  
  Add Authorizer in api-gateway
  
### Test with headers
  
#### Using lambda-auth-basic-function Authorizer 
    
##### PUT       
curl -v -X "PUT" -H "Content-Type: application/json" -H 'authorization: secretTokenForLambdaBasicAuth' -d "{\"id\": \"323\", \"price\": 12345, \"name\": \"myitem\"}" $INVOKE_URL/items 

curl -v -X "PUT" -H "Content-Type: application/json" -H 'authorization: secretTokenForLambdaBasicAuth' -d "{\"id\": \"111\", \"price\": 12345, \"name\": \"myitem\"}" $INVOKE_URL/items

curl -v -X "PUT" -H "Content-Type: application/json" -H 'authorization: secretTokenForLambdaBasicAuth' -d "{\"id\": \"222\", \"price\": 12345, \"name\": \"myitem\"}" $INVOKE_URL/items  
  
##### GET ALL  
curl -s GET $INVOKE_URL/items -H 'authorization: secretTokenForLambdaBasicAuth' | js-beautify
  
##### GET By ID
curl -s GET $INVOKE_URL/items/123 -H 'authorization: secretTokenForLambdaBasicAuth' | js-beautify
    
#### Using lambda-auth-iam-function Authorizer     

##### GET ALL    
curl -s GET $INVOKE_URL/items -H 'authorization: secretTokenForLambdaIAMAuth' | js-beautify
  
##### GET By ID    
curl -s GET $INVOKE_URL/items/323 -H 'authorization: secretTokenForLambdaIAMAuth' | js-beautify
  
##### PUT       
curl -v -X "PUT" -H "Content-Type: application/json" -H 'authorization: secretTokenForLambdaIAMAuth' -d "{\"id\": \"323\", \"price\": 12345, \"name\": \"myitem\"}" $INVOKE_URL/items
  
##### DELETE
curl -v -X "DELETE" -H 'authorization: secretTokenForLambdaIAMAuth' $INVOKE_URL/items/123  
  
## Go to cloudwatch logs, verify logs
 Go to the log group of lambda function
  
## Update the lambda execution role with "crud-server-api-least-privilege-role"
Execute queries in cloud 9
Check logs again
  
## Deploy API Gateway and create lambda version and alias  
