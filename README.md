# serverless-api-using-lambda
Sample how to deploy serverless api using lambda, api gateway and dynamodb and use custom authorizer to control access

# Create Dynamo db table 
Table name: http-crud-tutorial-items
Primary key: id

# Create IAM role "serverless-crud-api-role"
Grant access to managed policy AWSLambdaBasicExecutionRole and create custom role to grant access to dynamodb 

### Create node js lambda function "serverless-crud-function" in lambda console and copy code from serverless-crud-function.js

### Create node js lambda function "serverless-crud-function" in lambda console and copy code from serverless-crud-function.js

### Create node js lambda function "lambda-auth-basic" in lambda console and copy code from lambda-auth-basic.js

### Create node js lambda function "lambda-auth-iam" in lambda console and copy code from lambda-auth-iam.js

# Create API Gateway

Create http api gateway: http-serverless-api
create routes: GET{items}, GET/{items}/{id}, POST{items}, DELETE/{items}

### Use CURL to add the api-gateway url and invoke POST end point
#### Test without headers

export INVOKE_URL="<URL OF APIGATEWAY>"

curl -X "PUT" -H "Content-Type: application/json" -H -d "{
    \"id\": \"111\",
    \"price\": 12345,
    \"name\": \"myitem\"
}" $INVOKE_URL/items
  
### Invoke get end point
curl -s GET $INVOKE_URL/items | js-beautify 
  
### Notice it is publicly available
  
  
## Adding Custom authorizer  
  Add Authorizer in api-gateway
  
### Test with headers
  
curl -X "PUT" -H "Content-Type: application/json" -H 'authorization: secretToken' -d "{
    \"id\": \"111\",
    \"price\": 12345,
    \"name\": \"myitem\"
}" $INVOKE_URL/items
  
  
curl -s GET $INVOKE_URL/items -H 'authorization: secretToken' | js-beautify

curl -s GET $INVOKE_URL/items/123 -H 'authorization: secretToken' | js-beautify
  
