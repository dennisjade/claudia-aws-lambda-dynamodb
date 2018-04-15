# claudia-aws-lambda-dynamodb

setup
1. setup aws credentials on mac (https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
~/.aws
2. install claudia globally
npm install -g claudia
3. inside your project folder, install as dependencies
npm install aws-sdk claudia-api-builder -S


create db
1. run this in the terminal. (make sure aws command tool is installed)
aws dynamodb create-table --table-name icecreams \
  --attribute-definitions AttributeName=icecreamid,AttributeType=S \
  --key-schema AttributeName=icecreamid,KeyType=HASH \
  --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1 \
  --region us-east-1 \
  --query TableDescription.TableArn --output text
  
deployment
1. initially, you need to create the aws lambda instance
claudia create --region us-east-1 --api-module index --policies policy
Note: succeeding update to the setup just use the command below
claudia update

Sample output for successful created of lambda service
{
  "lambda": {
    "role": "ice-cream-shop-executor",
    "name": "ice-cream-shop",
    "region": "us-east-1"
  },
  "api": {
    "id": "your-service-id",
    "module": "index",
    "url": "https://your-service-url.execute-api.us-east-1.amazonaws.com/latest"
  }
}

sample commands
1. get all icecreams
curl https://your-service-url.execute-api.us-east-1.amazonaws.com/latest/icecreams
2. add icecream flavors
curl -H "Content-Type: application/json" -X POST \
-d '{"icecreamId":"123", "name":"chocolate"}' \
https://your-service-url.execute-api.us-east-1.amazonaws.com/latest/icecreams

