# DevOps Capstone Project - Simple CI/CD

By: Kaan Keskin

Date: October, 2021

Available at: https://github.com/kaan-keskin/devops-capstone-project-simple-ci-cd

DevOps Capstone Project for learning DevOps methodologies and technologies in the cloud and on-premise environment.

## Project Description

**Problem to Solve:** Create a CI/CD Pipeline to convert the legacy development process to a DevOps process.

### Background of the problem statement:

A leading healthcare company, with a large IT structure had a 12-week release cycle and their business was impacted due to the legacy process. To gain true business value through faster feature releases, better service quality, and cost optimization, they wanted to adopt agility in their build and release process.

The objective is to implement iterative deployments, continuous innovation, and automated testing through the assistance of the strategy.

**Implementation requirements:**

- Install and configure the Jenkins architecture on AWS instance.
- Use the required plugins to run the build creation on a containerized platform.
- Create and run the Docker image which will have the application artifacts.
- Execute the automated tests on the created build.
- Create your private repository and push the Docker image into the repository.
- Expose the application on the respective ports so that the user can access the deployed application.
- Remove container stack after completing the job.

**The following tools must be used:**

- Ansible
- Docker
- AWS EC2
- Git
- Jenkins
- Terraform

**The following things to be kept in check:**

You need to document the steps and write the algorithms in them, which includes:
- Project and tester details.
- Concepts used in the project.
- Your conclusion on enhancing the application and defining the USPs (Unique Selling Points).

## Amazon AWS: Authentication with AWS

In order to connect to AWS, Terraform or Ansible has to successfully authenticate. It is done with the help of Programmatic API Keys (Access Key and Secret). We should go and create these access and secret keys for our AWS account.

Login to AWS Console, in the services, go to IAM and perform the following steps.

<img src=".\images\amazon-aws-iam-1.png" style="width:75%; height: 75%;"/>

1. Add new user and key in the User Name:

<img src=".\images\amazon-aws-add-user-1.png" style="width:75%; height: 75%;"/>

2. Attach Existing Policies and Select Admin:

<img src=".\images\amazon-aws-add-user-2.png" style="width:75%; height: 75%;"/>

<img src=".\images\amazon-aws-add-user-3.png" style="width:75%; height: 75%;"/>

Let the Values be Default Click Next till you see the following Screen.

3. Completion and Download

<img src=".\images\amazon-aws-add-user-4.png" style="width:75%; height: 75%;"/>

**Note this information:**

Users with AWS Management Console access can sign-in at:
https://422------610.signin.aws.amazon.com/console

Access key ID: AKIA----------U3YZNC

Secret access key: W2YY----KtHRj------------QvHgGUa----tqDU

**API key creation Successful Message Banner**

> **Note**: Once the Access Key ID and Secret Access Key is created you can download and save them somewhere safe and if you lost it you cannot recover (or) re-download it. You would have to create a new API key.

> The best practice is to keep changing the API Access Key and recreating it. The older your API keys are the prone they are to Malicious attacks. So you should keep updating the API key and should not use the Same API key for a long period of time.

## Terraform AWS : Using Terraform to Create EC2 Instances

There are so many tools in the market helps you to achieve the **Infrastructure as a Code (IaC)**. It is always a tough choice to choose the right product from this. While everything has its pros and cons. **Terraform outruns them for the right reasons.**

**Terraform is an open-source infrastructure as code software tool created by HashiCorp.** It enables users to define and provision a data center infrastructure using a high-level configuration language known as **Hashicorp Configuration Language (HCL)**, or optionally **JSON**.

Terraform supports a number of cloud infrastructure providers such as Amazon Web Services, IBM Cloud (formerly Bluemix), Google Cloud Platform, Linode, Microsoft Azure, Oracle Cloud Infrastructure, or VMware vSphere as well as OpenStack

Since this is going to be the process of Infrastructure as a Code paradigm. We need a API programmatic access for AWS.

So we are going to programmatically create terraform ec2 instance.

If you want to compare Terraform with other IaC products like Chef, Puppet, Cloudformation, etc. you can check this page:

https://www.terraform.io/intro/vs/index.html

Terraform has a lot of resources and configurations that support the entire AWS Infrastructure management tasks like AWS EC2 instance creation, Security Group creation, Virtual Private Cloud (VPC) Setup,  Serverless set up, etc.

### Download and Install Terraform CLI

Terraform is a single file binary which you can download and run it without any additional installation.

You can find the instructions here:

https://learn.hashicorp.com/tutorials/terraform/install-cli

Now Let me proceed further with an assumption that you have installed the Terraform CLI.

<img src=".\images\terraform-version.png" style="width:75%; height: 75%;"/>

### Terraform Configuration File

The input file for terraform is known as Terraform Configuration. Terraform configuration is written in a specific language named **Hashicorp Configuration Language (HCL)** and it can optionally be written in **JSON** as well.

Here is the sample Terraform Configuration file saved with **\*.tf** extension

In order to connect to AWS, Terraform has to successfully authenticate. It is done with the help of Programmatic API Keys (Access Key and Secret).

Sample usage of these API Keys in a terraform configuration.

```hcl
provider "aws" {
  region     = "us-east-1" # US East - N. Virginia
  access_key = "my-access-key"
  secret_key = "my-secret-key"
}
```

You need to save the **API Access Key ID** and **Secret Access Key** so that you can use it in Terraform.

Though terraform accepts the Access Key and Secret Key hardcoded with in the configuration file. It is not recommended!

Either you should save these keys as Environment variables (or) save it as a AWS Config profile.

**As Environment Variable:**

In your terminal, you just have run these commands with your Access and Secret key.

```shell
$ export AWS_ACCESS_KEY_ID=AKIA----------U3YZNC
$ export AWS_SECRET_ACCESS_KEY=W2YY----KtHRj------------QvHgGUa----tqDU
```

**As an AWS config Profile:**

In order to do this, the simplest way is to download and setup AWS CLI.

The following file presumes that you are using the AWS Config profile. So it refers to the **profile: default** for the authentication.

```hcl
provider "aws" {
  profile    = "default"
  region     = "us-east-1"
}

resource "aws_instance" "example" {
  # Ubuntu Server 20.04 LTS (HVM), SSD Volume Type
  ami           = "ami-09e67e426f25ce0d7"
  instance_type = "t2.micro"
}
```

In case if you are using the Environment Variables method. You can remove the profile line alone and that should be it.

**Terraform Configuration File:**

Terraform configuration file would ideally have lot of elements known as blocks such as provider, resource etcetera.

This is a Syntax of how Terraform Configuration file block is formatted:

```hcl
<BLOCK TYPE> "<BLOCK NAME>" "<BLOCK LABEL>" {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}
```

There are a lot of BLOCK_TYPE's available in Terraform and the resource is primary and all others are to support building that specified resource.

Some of the Terraform blocks (elements) and their purpose is given below:

- **providers** – the provider name aws, google, azure etc.
- **resources** – a specific resource with in the provide such as aws_instance for aws.
- **variable** – to declare input variables.
- **output** – to declare output variables which would be retained the Terraform state file.
- **local** – to assign value to an expression, these are local temporary variables work with in a module.
- **module** – A module is a container for multiple resources that are used together.
- **data** – To Collect data from the remote provider and save it as a data source.

### Terraform EC2: Create AWS EC2 instance with Terraform

As we have crossed all the sections of basic and prerequisites. We are now ready to move forward to the practical application of Terraform and we are going to create an EC2 instance with terraform.

**Step1: Creating a Configuration file for Terraform AWS**

The Terraform configuration file to create EC2 instance is located in: '**./terraform/main.tf**' in this repository.

**Step2: Initialize Terraform**

Once we have saved the File in the newly created directory, we need to initialize terraform with **'terraform init'** command.

```shell
$ terraform init
```

<img src=".\images\terraform-init.png" style="width:75%; height: 75%;"/>

**Step3: Pre-Validate the change – A pilot run**

Once the Initialization completed, you can execute the '**terraform plan**' command to see what changes are going to be made.

Execute the '**terraform plan**' command and it would present some detailed info on what changes are going to be made into your AWS infra.

<img src=".\images\terraform-plan.png" style="width:75%; height: 75%;"/>

the **-out tfplan** is to save the result given by plan so that we can refer it later and apply it as it is without any modification.

```shell
$ terraform plan -out tfplan
```

It also guarantees that what we see in the planning phase would be applied when we go for committing it.

You can verify the outputs shown and what resources are going to be created or destroyed. Sometimes while doing a modification to the existing resources, Terraform would have to destroy the resource first and recreate it. In such cases, It would mention that it is going to destroy.

You should always look for the + and - signs on the terraform plan output.

Besides that, you should also monitor this line every time you run this command to make sure that no unintended result happen.

**Step4: Go ahead and Apply it with Terraform apply**

When you execute the '**terraform apply**' command the changes would be applied to the AWS Infra.

If '**terraform plan**' is a trial run and test,  '**terraform apply**' is real-time and production!

Since we have saved the plan output to a file named **tfplan** to guarantee the changes. We need to use this file as an input while running the apply command.

```shell
$ terraform apply "tfplan"
```

<img src=".\images\terraform-apply.png" style="width:75%; height: 75%;"/>

So we have successfully created an EC2 instance and a Security Group and logged into the server.

You can see newly created EC2 instance on AWS web console with all properties defined in '**main.tf**' file.

<img src=".\images\amazon-aws-ec2-terraform-apply.png" style="width:75%; height: 75%;"/>

<img src=".\images\amazon-aws-volumes-terraform-apply.png" style="width:75%; height: 75%;"/>

<img src=".\images\amazon-aws-security-groups-terraform-apply.png" style="width:75%; height: 75%;"/>

You can destroy the created resources by executing '**terraform destroy**' command.

```shell
$ terraform destroy
```

## Ansible AWS : Using Ansible to Create EC2 Instances

Ansible is an open-source automation tool that uses playbooks to enable you to make deployments faster and scale to various environments. Think of playbooks as recipes that lay out the steps needed to deploy policies, applications, configurations, and IT infrastructure. You can use playbooks repeatedly across multiple environments. 

Customers who use Ansible playbooks typically deploy periodic changes manually. As complex workloads increase, you might be looking for ways to automate them. In this chapter, we will show you how to automate an Ansible playbook deployment using Amazon Elastic Compute Cloud (Amazon EC2) and GitHub.

 I am assuming that you are using a modern version of Linux like Ubuntu or Centos. You need to have the latest version of Ansible installed.

<img src=".\images\ansible-version.png" style="width:75%; height: 75%;"/>

You can build almost any sort of environment of AWS no matter how simple or complex it can get. So, in order not to overwhelm you with so much information, we’ll create one EC2 instance from scratch. 

We’re going to do the following:

- Make an AWS account
- Create an IAM role and obtain your access and secret keys
- Generate a public/private key pair.

Then, using Ansible, we’ll create a playbook that will:

- Create a security group for the environment and add the appropriate rules
- Launch an EC2 instance based on the type and region


