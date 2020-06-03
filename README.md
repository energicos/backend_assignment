# Serverless API Overview

## 1. Background Info

Energicos is a utility company headquartered in Berlin/ Germany. We supply Gas, Electricity and Heat to mainly residential customers in Germany. Energicos energy supply is based on real-time data analysis to minimise the carbon footprint. The software representing the backbone of our real-time data analysis is being developed by a 100% distributed team using cutting-edge technologies. Our Applications are using AWS Serverless as main architecture layer. We use multiple AWS services supporting scalability for both the team and the resulting features to ensure a rapid feature development and deploy process. To accommodate our growing business we are looking for an independent and experienced engineer with great analytical skills and advanced knowledge regarding AWS services.

- We use AWS Serverless for our functionalities with Dynamodb as Datastore and API Gateway to access our APIS.
- The implementation of pure Reads and Writes to/from DynamoDB with API Gateway are done using VLT (Velocity Template Language) and custom integrations
- Our intention is to avoid creating lambdas when not needed
- For any other action we use Lambdas with nodejs as runtime (machine learning aside) and typescript as coding language
- All our deployments are Cloudformation based, we dont deploy Resources via UI Console
- We dont allow the usage of Serverless Framework or Terraform
- We use Makefiles to automate the stacks deployment

## 2. Goal

- This assignment is intended to test general development skills using the core technologies we employ supporting serverless development:
  - DynamoDb
  - DynamoDB Streams
  - API Gateway
  - Swagger
  - VTL
  - Lambda Functions
  - S3
  - SQS
  - SNS
  - Cognito
  - etc

## 3. Task Description

### 3.1 DynamoDB Setup

- We provide the data, split into 3 tables:
  - Clients,
  - Products
  - Orders.
- The tables are represented by a file in xlsx format. The data can be found in the [`./data` folder](./data).

Implement a lambda function capable to import the data from the three tables above into a single DynamoDB table (sticking to the [Single Table Pattern](https://www.alexdebrie.com/posts/dynamodb-single-table/)). Expose this lambda function through an API Gateway endpoint.
You can assume that the excel files have been uploaded into a public S3 bucket.

### 3.2 API Gateway VTL Endpoints

Create VTL endpoints for the following access patterns:

- Select all clients
- Select all orders for the ClientID=XYZ within a date period (Date1 - Date2)
- Select the orders with the CategoryID=XXX and ProductID from YYY to ZZZ that were made by the ClientID=XYZ

### 3.3 DynamoDB Streams: Monthly Report

Implement a lambda function responsible for generating a monthly report for a client. It should receive the ClientID, Year and Month. It should get the list of orders fulfilled by the given client's account within the specified year and month. Then it should prepare a table containing the following attributes: CategoryID, ProductID, Product, Price, Amount, Cost.
The `Amount` and `Cost` columns should contain the aggregated sums over the orders of each product type. Use DynamoDB Streams to aggregate the data. The report should be sent to the client's email address. You may use any format to present the data. Integrate the function call into the existing API Gateway.

## 4. Details of the assignment

- The assignment contains the above mentioned tasks 2./3.

### 4.1 Constraints

- All Cloudformation stacks must use YAML format

- Split the stacks to make the setup easy to review for QA. For example, if an API Gateway resource needs a role or s3 buckets before, be sure that these are implemented in separate stacks. Use the import/export functionalities from Cloudformation to achieve this goal.

- The API Gateway shall use the swagger yaml templates, describing all the resources (end-points, models, etc)

- Document all the steps needed to deploy the stacks on our environment.

- The test will only be approved once working in our environment. That's why we strongly suggest to focus on automation, **avoiding hardcoded values** from AWS-Account and AWS-Region inside your stacks. The setup must be ready to be deployed in several environments.

- Nodejs 10.x and Typescript 3.8 must be used for all the lambdas

## 5. Scope of delivery

| __Results__              | __Description__                                              |
| ------------------------ | ------------------------------------------------------------ |
| Documentation            | A documentation describing the resources and the approach as well as the elaborations on the lambda/ nodejs code. Follow the example described at [our repo.](./docu_template.md) |
| Cloudformation templates | Cloudformation templates with the resources working as expected in an automated way |
| Lambda Functions | A folder with all the lambda functions |
| API | the APIs must be ready to be tested using the AWS Console |
| Global Makefile | A Makefile that automates the creation of the Cloudformation Stacks. It should execute step by step every stack, waiting till its complete before moving to the next stack. It also needs to cover all the S3 uploads, for the case of lamba resources. |

### 5.1 The list of expected delivered resources

- Cloudformation template for creating the dynamoDB database.
- Cloudformation template + swagger integration file for the API (including the VTL and lambda integrations). Using the API Key authorization.
- Cloudformation template for creating the s3 bucket from where the lambda takes the xlsx files
- The folder with every created lambda's code (written in Nodejs + Typescript)
- Cloudformation templates for each lambda function deployment. Each lambda function should be in a separate Cloudformation stack
- Cloudformation templates for SQS, SNS or other
- Makefile script that deploys all the resources to AWS
- Tests via POSTMAN (or other similar programs) or the API tests, written in  Nodejs + Typescript + Mocha/Jest (or any other testing framework)

Please create a private repository in your own github's account and give access to the following github users:

- https://github.com/aerioeus
- https://github.com/jprivillaso

## 6. Compensation

- The test is being compensated if you deliver the entire assignment meeting all the requirements described above in time and its approved by QA as of your first submission. If the aforesaid requirements are met you will receive **500 USD**.

## 7. Timeline

- Project Term: 12 days upon receiving the assignment.
