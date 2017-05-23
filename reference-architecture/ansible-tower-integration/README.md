DISCLAIMER: The code within openshift-ansible-contrib is unsupported code that can be used in conjunction with openshift-ansible.

# Tower Integration Guide

This guide explains how to integrate the reference architectures within openshift-ansible-contrib/reference-architectures into Ansible Tower. By integrating the reference architecture playbooks into Ansible Tower it is possible to centralize and control the OpenShift infrastructure with a visual dashboard, role-based access control, job scheduling, integrated notifications and graphical inventory management.

## Overview

The diagram below illustrates the desired end state of this guide - to have Ansible Tower workflows able to deploy OpenShift and other services on various cloud providers, including creating the necessary virtual infrastructure on each cloud provider. 

![Overview Diagram](https://github.com/strategicdesignteam/openshift-ansible-contrib/blob/master/reference-architecture/ansible-tower-integration/Overview_Diagram.png)

Here are the major stages to achieving the desired end state. 
1. Deploying Ansible Tower on the desired cloud provider. 
2. Configuring Ansible Tower to deploy OpenShift and other services on the cloud provider of choice.
3. Performing a deployment. 

Currently this guide has support for Amazon Web Services, but we intend to add instructions for other providers. Deployment of the AWS reference architecture for OpenShift Container Platform including registration to the Red Hat Insights service and deployment of CloudForms with the ability to manage OpenShift Container Platform takes approximately 60 minutes.

## Deploying Tower

This section provides information about how to deploy Ansible Tower on various cloud providers. It then provides information about how to configure the deployed Ansible Tower. No matter where you deploy Tower you'll need to do two things:

1. [Obtain a license key](https://www.ansible.com/license)
2. [Download Ansible Tower](https://www.ansible.com/tower-trial)

### Deploying Tower on Amazon Web Services (AWS)

Follow the [Deployment Steps](http://docs.aws.amazon.com/quickstart/latest/ansible-tower/deployment.html) section of the [Ansible Tower on AWS](http://docs.aws.amazon.com/quickstart/latest/ansible-tower/welcome.html) quickstart guide to deploy Ansible Tower on AWS.

## Future Sections

We would like to add the following sections.

+ Deploying Tower on Google Cloud
+ Deploying Tower on Microsoft Azure
+ Deploying Tower on OpenStack
+ Deploying Tower on VMware


## Configuring Ansible Tower for deployments on Amazon Web Services

This guide shows you how to use the master branch of [OpenShift-Ansible-Contrib](https://github.com/openshift/openshift-ansible-contrib). If you want to ensure that no changes are made to the deployment configuration, you may want to [fork the repository](https://help.github.com/articles/fork-a-repo/) to ensure nothing changes without your knowledge.

Once you have Ansible Tower running and licensed you can clone and run the tower_config playbook with the appropriate variables. This will configure Ansible. Just make sure you have your tower_cli.cfg file setup and also make sure you have your AWS_KEY and AWS_SECRET as well as the ssh key you wish to use to authenticate to your machines. Then you can do the following:

Set the appropriate host, username, and password in ~/.tower_cli.cfg.
```$ $ vi ~/.tower_cli.cfg 
host=tower.domain.com
username=admin
password=yourpassword
```

```
$ git clone https://github.com/strategicdesignteam/openshift-ansible-contrib.git
$ cd reference-architecture/ansible-tower-integration/tower_config
$ ansible-playbook tower_config.yaml --extra-vars "AWS_MACHINE_SSH_KEY=<PATH/TO/PRIVKEY> AWS_KEY=<AWS_KEY> AWS_SECRET=<YOUR_AWS_SECRET>"
```

| Variable                   | Required           | Description  |
| ---------------------------|:------------------:| ------------:|
| AWS_MACHINE_KEY            | yes                | Your ssh key for connecting to AWS instances  |
| AWS_KEY                    | yes                | Your AWS Key|
| AWS_SECRET                 | yes                | Your AWS Extra Key |

This will *soon* configure tower with all the inventories, credentials, job_templates, and workflows to begin deploying across Amazon Web Services.

Some interesting notes:

There are many modules for tower (search tower_ [here](http://docs.ansible.com/ansible/list_of_all_modules.html)). There is no ansible module for creating a workflow. It would be helpful if this were available. For now, we will try to use the tower-cli. Also, there is no way to start an SCM update using the tower_project ansible module. It would be helpful if that existed, so the SCM update could be issued and then job_templates could reference the available playbooks that were synchronized.

## Future Sections

We would like to add the following sections.

+ Configuring Ansible Tower for deployments on Google Cloud Platform
+ Configuring Ansible Tower for deployments on Microsoft Azure
+ Configuring Ansible Tower for deployments on OpenStack
+ Configuring Ansible Tower for deployments on VMware



