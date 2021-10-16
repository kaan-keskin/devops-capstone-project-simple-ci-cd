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

**The following tools will be used:**

- Ansible
- AWS EC2
- Docker
- Git
- Jenkins
- Maven
- Terraform

**The following things to be kept in check:**

You need to document the steps and write the algorithms in them, which includes:
- Project and tester details.
- Concepts used in the project.
- Your conclusion on enhancing the application and defining the USPs (Unique Selling Points).

## Amazon AWS: Authentication with AWS

In order to connect to AWS, Terraform or Ansible has to successfully authenticate. It is done with the help of Programmatic API Keys (Access Key and Secret). We should go and create these access and secret keys for our AWS account.

Login to AWS Console, in the services, go to IAM and perform the following steps.

<img src=".\images\amazon-aws-iam-1.png" style="width:100%; height: 100%;"/>

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

### Create Key pairs

A key pair, consisting of a public key and a private key, is a set of security credentials that you use to prove your identity when connecting to an Amazon EC2 instance. Amazon EC2 stores the public key on your instance, and you store the private key. For Linux instances, the private key allows you to securely SSH into your instance. Anyone who possesses your private key can connect to your instances, so it's important that you store your private key in a secure place. 

Create Key pairs named "myseckey" on the AWS Web Console:

<img src=".\images\amazon-aws-key-pair-create.png" style="width:75%; height: 75%;"/>

The private key file is automatically downloaded by your browser. The base file name is the name that you specified as the name of your key pair, and the file name extension is determined by the file format that you chose. Save the private key file in a safe place.

<img src=".\images\amazon-aws-key-pairs.png" style="width:100%; height: 100%;"/>

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

<img src=".\images\amazon-aws-ec2-terraform-apply.png" style="width:100%; height: 100%;"/>

<img src=".\images\amazon-aws-volumes-terraform-apply.png" style="width:100%; height: 100%;"/>

<img src=".\images\amazon-aws-security-groups-terraform-apply.png" style="width:100%; height: 100%;"/>

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

We’re going to do generate a public/private key pair.

Then, using Ansible, we’ll create a playbook that will:

- Create a security group for the environment and add the appropriate rules
- Launch an EC2 instance based on the type and region

Another playbook will be used to shut down or destroy the environment.

### Storing your keys in Ansible vault

We’ll need to store the AWS keys. Since they are sensitive data, we should use Ansible vault for this:

```shell
$ ansible-vault create aws_keys.yml
```

Once open, add the following to it:

```shell
aws_access_key: AKIA----------U3YZNC
aws_secret_key: W2YY----KtHRj------------QvHgGUa----tqDU
```

Once you save the file, all the content will be encrypted. Let’s check that:

<img src=".\images\ansible-vault-create.png" style="width:75%; height: 75%;"/>

### Setting up the hosts file

Next, we need to create/update the hosts file to handle our new EC2 instance that yet to be created. Adding the following to ./hosts file:

```shell
[local]
localhost
```

### Building the EC2 instance

OK, now let’s edit our playbook file. Create a new file called aws_provisioning.yml and the following:

```yaml
- name: Create a Sandbox Instance.
  hosts: local
  connection: local
  gather_facts: False
  vars:
    ansible_host_group: webservers
    count: 1
    image: ami-09e67e426f25ce0d7
    instance_type: t2.micro
    keypair: myseckey 
    region: us-east-1
    security_group: webservers_sg
  vars_files:
    - aws_keys.yml
```

Let’s have a quick look at what each line of the file does:

First, you’re limiting the scope of the playbook to the local hosts group. It contains localhost and this is the way Ansible will work with EC2 instances. Behind the scenes, Ansible connects to Python boto on the local machine and use to establish connection with the AWS API and issue the necessary commands.

We need to set the connection to local so that Ansible won’t attempt to establish an SSH connection session with localhost unnecessarily. 

The variables section contains the optinos we intend to use with our instance:

- The instace type is t2.micro, this is suitable for our test environment. It’s also eligible for the free tier, in which Amazon will not charge you for some services (including selected EC2 instance types) for a period of 1 year.

- Then we specify the name of the security group that Ansible will create for us. A security group is like a virtual firewall that must be created for your EC2 instances. If you already have one created, you can associate it with the new EC2 instance. In our case, we’ll be creating a new one from scratch.

- The image specifies the AMI (Amazon Machine Image). AMI’s are like templates that are used to spawn machine instances. If you’ve used Vagrant before, they serve the same purpose as the box. You can even create and use your own AMI images. Amazon provides a list of its own AMI’s that can be found here: https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard. Notice that it depends on your region of choice.

- They keypair refers to the name of the public/private key pair that you created earlier.

- The region is the region of your choice. If you receive huge volumes of traffic, it’s advised that you choose a region that it geographically closest to where most of your customers are located. This is mainly to reduce network latency and enhance performance. However, if you are creating a lab, or if you are not expecting extremely high traffic volume, then you can choose the region based on the best pricing rates. Yes, each region may have different rates than the other for the AWS services you consume.

- The count variable is the number of instances you need to launch. All of them will share the same settings. In our case, we’ll only going to create one instance.

**Creating a security group:**

Now that we’ve defined the settings that will be used in the playbook, let’s start adding the tasks:

```yaml
  tasks:
    - name: Create a security group
      ec2_group:
        aws_access_key: "{{ aws_access_key }}"
        aws_secret_key: "{{ aws_secret_key }}"
        description: The webservers security group
        name: "{{ security_group }}"
        region: "{{ region }}"
        rules:
          - proto: tcp
            from_port: 22
            to_port: 22
            cidr_ip: 0.0.0.0/0
          - proto: tcp
            from_port: 80
            to_port: 80
            cidr_ip: 0.0.0.0/0
          - proto: tcp
            from_port: 443
            to_port: 443
            cidr_ip: 0.0.0.0/0
        rules_egress:
          - proto: all
            cidr_ip: 0.0.0.0/0
        state: present
```

Our first task will be to create a “Security Group” for our instance. As mentioned, a security group is nothing but a firewall that will selectively allow/deny traffic from and to your instances.

We use the **ec2_group** module provided natively by Ansible. The module needs a name for the security group. We passed the security_group variable. It also needs a region and a description.

Ansible **ec2_group** Module Documentation: https://docs.ansible.com/ansible/latest/collections/amazon/aws/ec2_group_module.html

Now comes the main part of the task: the rules. AWS security groups access two types of rules: incoming (ingress) and outgoing (engress). We’re more interested in what arrrives at our instance rather than what leaves it. 

So, we instruct our security group to allow:

- SSH on port 22 (that’s the only way you can remotely access your instance over the network). The security group can also filter the source IP address from which the traffic is originating. This is controlled by the cidr_ip option. AWS recommends that you set that to the IP or the IP range of the machine(s) you will be using to access the instance. If want to, you can leave it at 0.0.0.0/0, which means accept traffic from anywhere in the world.

- The web traffic that normally arrives at port 80. We also enabled port 443; as we will be adding HTTPS support later.

The rules_engress controls the network traffic leaving your instance to the outside world. We are not placing any filters on this.

**Creating and launching the EC2 instance:**

After creating the security group, our playbook may go ahead and create the instance itself. Add the following to the playbook file:

```yaml
    - name: Launch the new EC2 Instance
      ec2:
        aws_access_key: "{{ aws_access_key }}"
        aws_secret_key: "{{ aws_secret_key }}"
        count: "{{count}}"
        group: "{{ security_group }}"
        image: "{{ image }}"
        instance_type: "{{ instance_type }}"
        key_name: "{{ keypair }}"
        monitoring: yes
        region: "{{ region }}"
        state: present
        termination_protection: no
        wait: true
        wait_timeout: 320
      register: ec2
```

Nothing new here. We’ve just used a different Ansible module, **ec2**. Then, we passed the necessary parameters that it will need to create our instance:

- The necessary credential keys.
- The security group name.
- The instance type.
- The image AMI id.
- The wait parameter instructs the Ansible to wait for the instance to get created before reporting that the task is complete.
- Then the region, keypair, and count.
- Notice that the end of the task, we register a variable called ec2. We’ll further need the information inside this variable (like the instance id, the public IP and so on) later on. 

Ansible **ec2** Module Documentation: https://docs.ansible.com/ansible/latest/collections/amazon/aws/ec2_module.html

**Adding the newly created instance to the hosts file:**

Once the instance is created, we’ll need to be able to contact it. The following task will add the instance(s) to a group called webservers.

```yaml
    - name: Add the newly created host so that we can further contact it
      add_host:
        groups: "{{ ansible_host_group }}"
        name: "{{ item.public_ip }}"
      with_items: "{{ ec2.instances }}"
```

The **add_host** module allows you to add one or more hosts to a group. The group will be created if it does not already exist. In our case, we are adding the instance to webserversgroup.

Notice the use of with_items. It takes the instances list in the ec2 variable that we created in the previous task. This is necessary if you are creating more than one instance so that Ansible will loop through all of them. Each instance can be referred to by item. So, item_public_ip will get the public IP address assigned by AWS to that specific instance in the list.

Ansible **add_host** Module Documentation: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/add_host_module.html

**Tag the instance:**

AWS allows you to add tags to your instances. A tag consists of a name and a value. We will need to add at least one tag to our instance specifying its name. The reason we need this tag is to be able to identify our instances later on when we need to perform additional actions against them, including termination. You can add the following task to the playbook to tag the instance:

```yaml
    - name: Add tag to Instance(s)
      ec2_tag:
        aws_access_key: "{{ aws_access_key }}"
        aws_secret_key: "{{ aws_secret_key }}"
        region: "{{ region }}" 
        resource: "{{ item.id }}" 
        state: "present"
        tags:
          Type: webserver
      with_items: "{{ ec2.instances }}"
```

The task is pretty simple, use the **ec2_tag** module and specify the tags in the args parameter.

Ansible **ec2_tag** Module Documentation: https://docs.ansible.com/ansible/latest/collections/amazon/aws/ec2_tag_module.html

**Finishing up instance creation:**

We need to ensure that the creation process is complete and that the SSH daemon is ready to receive connections. This can be done with the following task:

```yaml
    - name: Wait for SSH to come up
      wait_for:
        connect_timeout: 5
        delay: 60
        host: "{{ item.public_ip }}"
        port: 22
        state: started 
        timeout: 320
      with_items: "{{ ec2.instances }}"
```

Here, we are making use of the **wait_for** Ansible module, which does nothing but pause playbook execution till a specific condition is met. In our case, it’s port 22 (default SSH port) on our host coming up and accepting connections.

Ansible **wait_for** Module Documentation: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/wait_for_module.html

**Running the playbook:**

Before running the playbook, we need to configure the following:

- The private key that Ansible will use to connect to the host.
- Avoid displaying the host identification dialog that SSH shows whenever you want to connect to a host for the first time. This is necessary if you want to run the playbook unattended.

To do this, we need to override the default Ansible’s configuration file, Ansible.cfg. It is located by default in '**/etc/ansible**'. But, placing a file with the same name in the working directory will override the default one. Create a new file called **ansible.cfg** in the current working directory and add the following:

```yaml
[defaults]
host_key_checking = False
private_key_file = myseckey.pem
```

You need to check file permissions for myseckey.pem.

```shell
$ sudo chmod 0600 myseckey.pem
```

Now, we’re ready to run the playbook by issuing the following command:

```shell
$ ansible-playbook -i hosts --ask-vault-pass aws_provisioning.yml
```

<img src=".\images\ansible-playbook-aws-provisioning.png" style="width:100%; height: 100%;"/>

After the playbook finishes running successfully, you can check your AWS console for a new EC2 instance created and assigned the correct security group.

<img src=".\images\amazon-aws-ansible-playbook-provisioning.png" style="width:100%; height: 100%;"/>

**Test Drive:**

After EC2 instance created, we can easily install different services/applications in to the EC2 instance with Ansible. 

You can ping created EC' nstance with Ansible ping module:

```shell
$ ansible webservers -i hosts -m ping -u ubuntu
```

<img src=".\images\ansible-ping-webservers.png" style="width:75%; height: 75%;"/>

We will install Apache2 as a service with given playbook:

```yaml
- name: Install Apache2 Webserver
  become: yes
  gather_facts: yes
  hosts: webservers
  pre_tasks:
    - name: 'Update system'
      raw: 'sudo apt-get update' 
    - name: 'Install Python'
      raw: 'sudo apt-get -y install python3'
  remote_user: ubuntu
  tasks:
    - name: Install Apache2
      apt:
        name: apache2
        state: present
    - name: Apache2 Service
      service:
        enabled: yes
        name: apache2
        state: started
```

```shell
$ ansible-playbook -i hosts ansible_ubuntu_apache_install.yml
```

<img src=".\images\ansible-playbook-aws-apache-deploy.png" style="width:100%; height: 100%;"/>

Further, you can fire up your browser and naviagate to http://ec2-ip, you should see the default Ubuntu page, where ec2-ip is the public IP address that got assigned to your instance by AWS.

<img src=".\images\apache2-default-page.png" style="width:100%; height: 100%;"/>

**Terminating the instance:**

Unless you are still in the free-tier period offered by Amazon, which lasts for 1 year, you are going to be charged for running the instance on a time basis. So, if you don’t need the instance for the time being or at all, you should stop or terminate it.

The difference between stopping and terminating the instance:

Stopping the instance is like issuing the shutdown command. You can start it up again without losing any data. You will not be charged for a stopped instance. You may be charged - however - for other resources related to the intance like storage.

The following playbook is very simple: it will grab all the instances by a specific tag and terminate them. Create a new file called **ec2_down.yml** and add the following:

```yaml
- hosts: local
  connection: local
  vars:
    region: us-east-1
  vars_files:
    - aws_keys.yml
  tasks:
    - name: Gather EC2 Facts
      ec2_instance_facts:
        region: "{{ region }}"
        filters:
          "tag:Type": "webserver"
        aws_access_key: "{{ aws_access_key }}"
        aws_secret_key: "{{ aws_secret_key }}"
      register: ec2
    - debug: var=ec2
    - name: Terminate EC2 Instance(s)
      ec2:
        instance_ids: '{{ item.instance_id }}'
        state: absent
        region: "{{ region }}"
        aws_access_key: "{{ aws_access_key }}"
        aws_secret_key: "{{ aws_secret_key }}"
      with_items: "{{ ec2.instances }}"
```

The playbook starts with declaring the required variables that will be used throughout the file, then it defines two tasks:

- ec2_instance_facts: This task is responsible for collecting the instance facts. Don’t confuse this with the traditional fact-gathering that Ansible performs by default when it executes any playbook. Here, Ansible is collecting facts that are related to the presence of this instance on the AWS platform. Facts like the tags that were assigned to the instance are collected, which is what interests us.

- ec2: Again, we use the ec2 module, but this time to terminate the instance. The state parameter can take other values than absent depending on your requirements. For example, stopped will just shut down the instance, restarted will reboot it, and running will ensure that it is running (it will start the machine if stopped).

```shell
$ ansible-playbook -i hosts --ask-vault-pass aws_ec2_down.yml
```

<img src=".\images\ansible-playbook-ec2-down.png" style="width:100%; height: 100%;"/>

You can see terminated EC2 instances on AWS web console:

<img src=".\images\amazon-aws-instances-ansible-playbook-down.png" style="width:100%; height: 100%;"/>

## Deploy Jenkins with Docker and Ansible

In the world of CI/CD, Jenkins is a popular tool for provisioning development/production environments as well as application deployment through pipeline flow. Still, sometimes, it gets overwhelming to maintain the application's status, and script reusability becomes harder as the project grows.

To overcome this limitation, Ansible plays an integral part as a shell script executor, which enables Jenkins to execute the workflow of a process.

### Install Docker with Ansible

Use Ansible’s apt module to install the Docker engine as a system service:

```yaml
- name: Install Docker
  become: yes
  gather_facts: yes
  hosts: webservers
  remote_user: ubuntu
  tasks:
    - name: Update System
      raw: 'sudo apt-get update'
    - name: 'Install Required Tools and Libraries'
      raw: 'sudo apt-get -y install python3-pip python3 apt-transport-https ca-certificates curl gnupg lsb-release'
    - name: Add Docker Group
      group: name=docker state=present
    - name: Add Docker Signing Key
      raw: 'curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg'
    - name: Add Docker Repo
      raw: 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null' 
    - name: Update System
      raw: 'sudo apt-get update' 
    - name: Install Docker
      raw: 'sudo apt-get -y install docker-ce docker-ce-cli containerd.io'
    - name: Install docker-py
      pip: name=docker-py
```

```shell
$ ansible-playbook -i hosts ansible_ubuntu_docker_install.yml
```

<img src=".\images\ansible-ubuntu-docker-install.png" style="width:100%; height: 100%;"/>

We can also check status of the Docker service directly from the created EC2 instance:

<img src=".\images\amazon-aws-ec2-console-docker-service.png" style="width:100%; height: 100%;"/>

### Run Jenkins Container with Ansible

The official Jenkins container image will install application files to **/var/jenkins_home** within the container, and this directory needs to be available outside the container, so use the Docker -v option to map the volume to, say, **/share/volumes/jenkins** on the host:

```yaml
- name: Run Jenkins Container
  become: yes
  gather_facts: yes
  hosts: webservers
  remote_user: ubuntu
  tasks:
  - name: Ensure jenkins directory on Docker host
    file:
      state: directory
      owner: 1000
      group: 1000
      path: /share/jenkins
  - name: Pull the latest official Jenkins Docker image
    docker_image:
      name: "jenkins:latest"
  - name: Create a container from the jenkins docker image
    docker_container:
      name: "jenkins-server"
      image: "jenkins"
      ports:
          - "8080:8080"
          - "50000:50000"
      volumes:
          - "/share/jenkins:/var/jenkins_home"
      state: present
      recreate: no
```

```shell
$ ansible-playbook -i hosts ansible_jenkins_container.yml
```
