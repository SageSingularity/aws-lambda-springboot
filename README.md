# SpringBoot on Lambda

Example repository for running a SpringBoot API on AWS Lambda. This GitHub repo coincides with a [series on Youtube](https://www.youtube.com/watch?v=A1rYiHTy9Lg&list=PLCOG9xkUD90IDm9tcY-5nMK6X6g8SD-Sz) walking through how to build and deploy SpringBoot API's on Lambda.

## Deployment

To deploy this sample into your own AWS account two separate steps are required. The first is to deploy the infrastructure, the second to deploy the application code.

### Infrastructure

The infrastructure is built using the AWS CDK. The deployed resources contain:

- A VPC
- 2 private, 2 public subnets
- 2 NAT Gateways
- Postgres RDS Instance
- Secrets Manager secret for credentials
- Lambda function for applying database changes

To deploy the infrastructure, run the following commands in order:

```
cd infrastructure/db-setup
mvn clean install
cd ../cdk
cdk deploy
```

This will compile the db-update Lambda function and then deploy the CDK infrastructure to your account. 3 values will output to the console that are required to deploy the application.

### Application

The application is deployed using AWS SAM. You can [install AWS SAM here](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html). To deploy the application run the below commands:

```
sam build
sam deploy --guided
```

# Dependencies
* AWS-CDK
* AWS CLI
* AWS SAM
* Java 17
* SpringBoot 3

# Project Directory
* `~./aws`: Stores AWS Credentials/Configuration. Not part of this project, but may be necessary to work with AWS CLI.
* `.github`: Stores GitHub actions and workflows.
* `infrastructure/`:  Contains the code and configuration files that define and provision the cloud infrastructure resources, such as AWS services, required to support the application.
* `src/`: Root directory containing all the source code, including Java classes, resources, and test files, organized into subdirectories like `main` for production code and `test` for unit tests.
* `target/`: Where Maven stores the compiled bytecode, packaged artifacts (like JARs or WARs), and other build output generated during the project's build process.
* `pom.xml`: Defines the project’s dependencies, build plugins, versioning, and other project management details necessary for building and managing the project lifecycle.
* `template.yaml`: Used with AWS SAM CLI to define the infrastructure and configuration for serverless applications, including AWS Lambda functions, APIs, and associated resources, using a simplified syntax that extends AWS CloudFormation.

# Common Commands
## Maven
* `maven clean install`: First removes any previously compiled files (clean), then compiles the project, runs tests, packages the code into its distributable format (like a JAR), and finally installs the package into the local Maven repository (install) for use as a dependency in other projects.
## AWS
* `aws configure`: Set up and store your AWS credentials, default region, and output format on your local machine, enabling authenticated access to AWS services.
## AWS SAM (Serverless Application Model)
* `sam build`: Compiles and prepares AWS Lambda functions and serverless application code, including dependencies, into ready-to-deploy artifacts.
* `same deploy --guided`: Deploys the serverless application, including Lambda functions and other resources, to AWS by creating or updating the necessary infrastructure based on the `template.yaml` configuration.
## CDK (Cloud Development Kit)
* `cdk bootstrap`: First time setup for CDK, checks that you (or AWS User) has valid permissions to execute deployment.
* `cdk deploy`: Synthesizes your infrastructure code into an AWS CloudFormation template and then deploys the defined resources to your AWS environment.
