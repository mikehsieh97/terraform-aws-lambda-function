# AWS Lambda module

## Description

This module is a simple wrapper around the `aws_lambda_function` aws resource. It creates:
 * a lambda function whose name is namespaced with the terraform workspace and the project name.
 * a cloudwatch log group the lambda function will ship its logs to.

## Usage

```hcl
module "project" {
  source  = "PGBI/project/aws"
  version = "~>0.1.0"

  name     = "myproject"
  vcs_repo = "github.com/account/project"
}

/**
 * Create a lambda function whose name will be "prod-myproject-hello" if the terraform workspace is "prod".
 */
module "lambda" {
  source  = "PGBI/lambda-function/aws"
  version = "~>0.1.0"

  description = "Python hello world lambda."
  name        = "hello"
  runtime     = "python3.7"
  project     = module.project
  src_path    = "${path.module}/dist"
  env_vars    = {
    ENV = terraform.workspace
  }
}
```
