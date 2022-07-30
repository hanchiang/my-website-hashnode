## Automating infrastructure provisioning, configuration, and application deployment

As a software engineer, I read and write application code every day. Occasionally, I get to work a little on CI/CD pipeline - Fix build issues, deploying a new application, etc. However, I have little understanding on how the entire process is orchestrated and automated.

So, I began with the goal of learning how to automate the entire stack: from infrastructure **provisioning**, **configuration**, application **deployment**, and starting and stopping the stack itself.

After spending some time crawling through Reddit and Stack Overflow, I gained a basic understanding of the whole process and the tools that are commonly used.

## Overview
The target application is a [URL shortener](https://urlshortener.yaphc.com/) that makes a long URL into a shorter one, like [TinyURL](https://tinyurl.com).

Backend repository: https://github.com/hanchiang/url-shortener-backend

Infra repository: https://github.com/hanchiang/url-shortener-infra

Technologies used:

- Provisioning: packer, terraform
- Configuration: ansible
- Deployment: docker, github actions
- Cloud computing: AWS EC2

**Provisioning, configuration, deployment**

![deployment-pipeline.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659163892638/_FDRD2nby.png align="left")


## What is provisioning, configuration, and deployment?
It can be confusing to understand what provisioning, configuration, and deployment mean exactly.

My understanding is summarised in a timeline: provisioning -> configuration -> deployment

**Provisioning**

Provisioning is the process of setting up servers to be ready for use.
In the context of cloud computing, this server is likely a **virtual machine**, on which a machine image is installed.
Here, the operating system, along with other required softwares are installed.

In AWS terms, this means creating a VPC, subnet, route table, and EC2.

![iaas-paas-saas-diagram.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659153623858/1VscWWRlW.png align="left")
Source: [redhat](https://www.redhat.com/en/topics/cloud-computing/iaas-vs-paas-vs-saas)

The diagram above is useful to remember which layers we have to take care of under different cloud computing models.


**Configuration**

Configuration can be considered as an extension to the process of provisioning the server. Once the server is created, and loaded with the required software, it needs to be configured so that it is usable.

e.g. Configure PostgreSQL data directory, logging and authentication settings

**Deployment**

With the infrastructure ready to serve live traffic, an application can finally be deployed from an artifact repository to the server.

This could be as simple as SSH into the server, pulling a docker image, and running a container, or deploying to a kubernetes cluster with helm.

**Visualising the whole pipeline**

![infrastructure and code.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659164575324/TjP0LHbV_.png align="left")
Source: [golibrary](https://www.golibrary.co/infrastructure-as-code-orchestration-provisioning-configuration-management-ansible-terraform)


## Deployment
I chose deployment as the first step because this is the easiest part of the pipeline. After building features and committing code, the next step is to deploy the new application.

**Application setup**

The URL shortener application is built on docker. Docker compose is used for the convenience of local setup. It consists of 3 containers: nodejs server, postgres, redis

%%[urlshortener-docker-compose]

**Deploying the application**

Application deployment is done with [github actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions).

%%[urlshortener-github-actions-deploy]

The docker image is built and pushed to Github container registry.
Then, it connects to the EC2 instance via SSH, pulls and runs the new image.

## Provisioning
With the deployment part of the process handled, let's move on to provisioning the server.

There are 2 parts to provisioning:

- Build a system image with all the necessary software
- Build the infrastructure and servers


### Building image with packer
[Packer](https://www.packer.io/) is used to create a golden [AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) with software and configurations, which is installed on the EC2 instance.

I used packer to create a user, install PostgreSQL, Redis, Nginx, Docker.

Using packer here takes advantage of [immutable infrastructure](https://www.bridge-global.com/blog/mutable-vs-immutable-infrastructure), in which a standardised fleet of servers is built, achieving consistency and maintainability.

%%[urlshortener-packer]

### Building infrastructure with terraform
After that, the infrastructure is **orchestrated** with [Terraform](https://www.terraform.io/).
It takes care of creating a VPC, subnet, EC2, route table, security groups.

Terraform is a tool that allows us to build the entire infrastructure in the **correct order**, and keeps track of the **desired state**, in a declarative style.

It can also detect state drift by synchronising its state to match the real-world state.


Again, the benefits of consistency and maintainability are present.

%%[urlshortener-terraform]

## Configurations
Lastly, configurations is the missing middle piece in the entire pipeline. 

After the system image is built with packer, and AWS infrastructure created with terraform, there are a few configuration tasks that need to be done in order for the server to be ready to serve live traffic.

Ansible is a simple yet powerful tool that allows us to configure servers in yaml.

*I am using ansible to run shell scripts imperatively instead of managing configurations declaratively using the pre-defined modules, because I found it the easiest way to learn*

### Set up file system for postgres
Set up file system on a separate block storage.

*This step isn't required, but I wanted to store postgres data separately from the root device so that its data is preserved each time a new server is provisioned*.

%%[urlshortener-setup-file-system-ansible]

### Configure postgres
Configure postgres data directory and copy data over.

%%[urlshortener-copy-postgres-data-ansible]

### Configure Nginx
Configure SSL on nginx, redirect HTTP to HTTPS, and proxy all requests to the port that the URL shortener container is listening on.

%%[urlshortener-nginx-ansible]

## Schedule start and stop
As an additional step, I wanted to schedule the start and stop of the EC2 and application via [scheduled workflow in github actions](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule). 

This consist of the configurations steps, as well as interaction with [AWS CLI](https://aws.amazon.com/cli/) and [Github API](https://docs.github.com/en/rest).

The script can be found [here](https://github.com/hanchiang/url-shortener-infra/blob/master/instances/scripts/start.sh)


## Summary
The end result achieved is a full stack pipeline from infrastructure provisioning to application deployment.
Now that I have a basic understanding of DevOps, the next step for me is to explore site reliability, that is [observability](https://iamondemand.com/blog/the-3-pillars-of-system-observability-logs-metrics-and-tracing).

Website: [URL shortener](https://urlshortener.yaphc.com/)

Backend repository: https://github.com/hanchiang/url-shortener-backend

Infra repository: https://github.com/hanchiang/url-shortener-infra


