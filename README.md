![](https://visitor-badge.laobi.icu/badge?page_id=sreejithaws123/Github-Actions-Continuous-Deployment)

# 
Github Actions Continuous Deployment

Github Actions Continuous Deployment for Git based website/App with Packer, Terraform and Ansible. 

# Description

Here I have created a sample Jenkins Continuous Deploymnet Pipeline with Git Website, Packer, Terraform and Ansible. The work flow is described below,

1. Developer commits changes to Git Repository
2. The Git webhook will alert Jenkins regarding the commit and the Pipeline starts
3. First Job in pipeline is creating the Golden image of the current website/app using [Packer](https://www.packer.io/)
4. Once the AMI creation is done successfully the next Job will be automatically triggered. In this step the [Terraform](https://www.terraform.io/) will create following infrastucture in AWS
    a) Application Load Balancer
    b) Target Group for ALB
    c) Https Listner and http to https redirection
    d) Security Group
    e) Launch Configuration with Packer Golden image
    f) Auto Scaling Group with the Launch Configuration.
5. Once second job is completed successfully the third one starts, which is offloading and adding new website contents to the instances. This is done using [Ansible](https://www.ansible.com/). In this job, the ansible will offload the instances one by one ensuring that there is no downtime for the website. By doing this we can avoid unnecessary termination of old instances from ASG and change the contents of current instances itself. This job will be skipped in the first run since the contents are same. 

Below, I have included a simple diagram showing this CD pipeline,

![](https://github.com/sreejithaws123/first/blob/05edaaa883c4d202ee4af034af93d862a5db48c2/Architecture.JPG)

## How to use

##### Prerequisites

- Git, Terraform, Ansible, Packer and jenkins installed on the master server. 
- All necessary dependencies for these tools.
- One AWS key pair created
- ARM of ACM certificate created/imported in AWS
- IAM use with necessary progamatic access. 
- Domain name and Git repo with website/App files.

##### Files to edit

- autoscaling_vars.yml (Variable file for Ansible)
- Git-Script.sh  (Script for Packer - Enter the Git URL)
- terraform.tfvars (Variable file for Terraform) 
- GitWebsite.pkr.hcl (Edit the variables section) 

```
cd /var 
git clone https://github.com/MarkAntonyGit/Jenkins-Continuous-Deployment.git
Chown -R jenkins. Jenkins-Continuous-Deployment
cd Jenkins-Continuous-Deployment 
# Edit the above mentioned files and add key pem file with 600 permission and jenkins ownership
Setup Jenkins with Ansible, Teraform, Packer, Deployment pipeline plugins and Create 3 jobs for pipeline
Setup Webhook for the Git Repository
``` 

This is it! 
Once any commits are done on the Git repository the Jenkins will be triggered and the pipeline will start automatically. 

#### Sample Screenshots

-- Git Hub Webhook

![](https://github.com/sreejithaws123/first/blob/05edaaa883c4d202ee4af034af93d862a5db48c2/webhook.JPG)

-- Jenkins Pipeline

![](https://github.com/sreejithaws123/first/blob/05edaaa883c4d202ee4af034af93d862a5db48c2/2.JPG)

-- Git Website Version 1

![](https://github.com/sreejithaws123/first/blob/05edaaa883c4d202ee4af034af93d862a5db48c2/website%201.JPG)

-- Once Version 2 is commited

![](https://github.com/sreejithaws123/first/blob/05edaaa883c4d202ee4af034af93d862a5db48c2/website%202.JPG)

### Connect with me

--------<img src="https://img.shields.io/badge/-Sreejith%20Rajan-brightgreen"/> ----------------------------------------------------------------------------------------------------------------------------------- <a <href="https://www.linkedin.com/in/profile-markantony/"><img src="https://img.shields.io/badge/-Linkedin%20Profile-blue"/></a> ------------------------------------------------------------------------------------------------------------------------------------ <a <href="mailto:sreejithrjn1995@gmail.com"><img src="https://img.shields.io/badge/-markantony.alenchery@gmail.com-D14836?style=flat&logo=Gmail&logoColor=white"/></a>-------------------------------------------------------
