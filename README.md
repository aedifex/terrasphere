# terrasphere

A simple exercise in IaC &amp;&amp; CI/CD leveraging Terraform and CircleCI

“There are only two hard problems in computer science: cache invalidation, off-by-1 errors, and naming things” 
– random software professional

Enter Terracube, a simple and elegant sample application demonstrating continuously deployment of infrastructure using CircleCI and Terraform.

[![<ORG_NAME>](https://circleci.com/gh/aedifex/terrasphere.svg?style=svg)](https://app.circleci.com/pipelines/github/aedifex/terrasphere)

## Prerequisites 

1.	A CircleCI account
2.	A Github account
3.	An AWS account (with corresponding S3 bucket)

## With your 3 accounts setup:
- Fork the repository to your Github account and clone the source code onto your local machine.

- Copy the vars template file (`terraform.tfvars.template`) into a file called `terraform.tfvars` 

- Fill in the appropriate values. All of the variables used have default values, this includes AWS region and AMI.

Now, we are ready to create a remote backend for state storage. Externalizing the statefile is absolutely critical when using terraform in a CI environment.

Do this:

`cd s3_backend`

`terraform init && terraform apply`

The desired output should look like this:

```
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:

s3_bucket_name = 5092f11c-xxxx-xxxx-xxxx-xxxxxxx-backend
```

Do this:
Copy the value of the s3_bucket_name.

``cd ..``

Add the value of the bucket name from the previous step to `bucket = "YOUR-UNIQUE-BUCKET-ID"` in `main.tf` under the `backend` module.

You can now commit these changes upstream. We're now ready to start running builds on CircleCI.

You'll need to add this project to CircleCI in order to trigger builds on the platform.

CircleCI offers a very generous free tier, so worry not if you don't already have an account (hopefully you do!).

On the project dashboard, e.g.
https://app.circleci.com/projects/project-dashboard/github/aedifex/

Simply click setup project to configure CircleCI to start building on every commit. CircleCI's robust API handles all of the magic under the hood, much like terraform handles the magic of deploying infrastructure.

Once you've added the project, CircleCI is going to trigger an initial pipeline by default and then during every subsequent push event. 

The `config.yml` file will be used for CircleCI configuration, i.e. steps to execute when running a pipeline.

Your first pipeline will fail, but don't worry, this is expected. You'll need to add your AWS credentials as project specific environment variables.

More on how to setup project specific environment variables can be found here: https://circleci.com/docs/2.0/env-vars/#setting-an-environment-variable-in-a-project

Once you've added the project to circle and setup your environment variables, you're ready to deploy some infrastructure! 

You can now tell your friends and family you've deployed infrastructure on AWS. Exciting, I know.

The shape of our workflow looks something like:

![Image of jobs after staging merge](https://github.com/aedifex/terrasphere/blob/master/.images/TerraformWorkflow.png)

