🚀 Jenkins Controller + Agent Setup (with Terraform, Pipelines, GitHub Webhook)
1. Provision Servers with Terraform

Create 2 EC2 instances (or VMs):

Jenkins Controller

Jenkins Agent

In user_data of Controller:

#!/bin/bash
yum update -y
yum install -y java-11-amazon-corretto git
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
yum install -y jenkins
systemctl enable jenkins
systemctl start jenkins


In user_data of Agent:

#!/bin/bash
yum update -y
yum install -y java-11-amazon-corretto git docker
usermod -aG docker ec2-user

2. Access Jenkins UI

Open browser → http://<controller-public-ip>:8080.

Unlock Jenkins → copy admin password:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword


Install suggested plugins.

Create Admin User.

3. Install Extra Plugins

In Jenkins UI → Manage Jenkins > Plugins > Available plugins

Install:

Git

Pipeline

GitHub Integration

SSH Agent

Node and Label parameter

4. First Job (Freestyle Pipeline)

New Item → choose Freestyle Project.

In Build section → add Execute shell:

echo "Hello from Jenkins Freestyle!"


Save & Build.

Check Console Output.

5. Pipeline Job (Declarative Syntax)

New Item → choose Pipeline.

In Pipeline Script, paste:

pipeline {
    agent any
    stages {
        stage('Build') { steps { echo 'Building...' } }
        stage('Test')  { steps { echo 'Testing...' } }
        stage('Deploy'){ steps { echo 'Deploying...' } }
    }
}


Save & Build.

6. GitHub Integration

Add a Jenkinsfile to your repo with same pipeline code.

In Jenkins job → Pipeline > Definition → select Pipeline script from SCM.

SCM = Git

Repo URL = https://github.com/<your-repo>.git

Branch = main

Add Webhook in GitHub:

Repo → Settings → Webhooks → Add Webhook

Payload URL =

http://<controller-public-ip>:8080/github-webhook/


Content type = application/json

Events = "Just the push event".

First build: trigger manually.

After that → triggered automatically on git push.

7. Add Jenkins Agent

In Jenkins UI → Manage Jenkins > Nodes & Clouds > New Node.

Give name my-agent, set:

Remote root dir: /home/ec2-user/jenkins-practice

Usage: Use as much as possible

Launch method: Launch agents via SSH

Host: <agent-private-ip>

Credentials: Add SSH key for ec2-user.

Save → Jenkins will SSH and install remoting.jar on agent.

Node turns green (online).

8. Run Pipeline on Agent

Update Jenkinsfile:

pipeline {
    agent { label 'my-agent' }
    stages {
        stage('Build') { steps { echo "Running on agent" } }
    }
}


Build → Check console → It runs inside Agent workspace:

/home/ec2-user/jenkins-practice/workspace/<job-name>


✅ At this point you have:

Jenkins Controller running

Jenkins Agent connected

Freestyle job tested

Declarative pipeline tested

GitHub Webhook working

Jobs running on both controller & agent