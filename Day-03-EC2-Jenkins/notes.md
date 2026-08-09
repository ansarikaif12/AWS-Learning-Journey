# Day 03 - EC2 + Jenkins

## 1. EC2 (Elastic Compute Cloud)

Amazon EC2 provides virtual servers in the cloud.

We can use an EC2 instance to run applications, websites, Jenkins, etc.

### Important points

- Instance = Virtual Server
- AMI = Operating System image
- Instance Type = CPU + RAM configuration
- Security Group = Virtual firewall
- Public IPv4 = Used to access the server from the internet
- Private IPv4 = Used for communication inside the AWS network

---

## 2. Launching EC2 Instance

I launched an EC2 instance using:

- Amazon Linux 2023
- SSH for connecting to the instance
- Security Group for controlling incoming traffic

To connect to the instance:

```bash
ssh -i key.pem ec2-user@<PUBLIC-IP>



3. Security Groups

A Security Group controls inbound and outbound traffic for an EC2 instance.

For Jenkins, port 8080 is required because Jenkins runs on port 8080 by default.

Example:

Type: Custom TCP
Port: 8080
Source: My IP / Required IP

Then Jenkins can be accessed using:

http://<PUBLIC-IP>:8080
4. Java

Jenkins requires Java.

Checked Java version using:

java --version

Java 21 was already available on the EC2 instance.

Output showed:

openjdk 21
Amazon Corretto

Amazon Corretto is AWS's distribution of OpenJDK.

5. Installing Jenkins

Added the Jenkins repository:

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/rpm/jenkins.repo

Installed Jenkins:

sudo dnf install jenkins

Started Jenkins:

sudo systemctl start jenkins

Checked Jenkins status:

sudo systemctl status jenkins

If the service shows:

Active: active (running)

Jenkins is running successfully.

6. Accessing Jenkins

Jenkins was accessed through the EC2 public IP:

http://<PUBLIC-IP>:8080

Completed the initial Jenkins setup.

Steps included:

Unlocking Jenkins
Installing suggested plugins
Creating Jenkins administrator account
Configuring Jenkins URL
7. Jenkins Pipeline

Created a Pipeline project named:

my-first-pipeline

A Pipeline defines the steps that Jenkins should execute automatically.

Basic Pipeline:

pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello from Jenkins!'
            }
        }
    }
}
8. Pipeline Components
Pipeline

Defines the complete CI/CD process.

Agent

Defines where the Pipeline should run.

agent any

means Jenkins can use any available agent.

Stage

Represents a major phase of the Pipeline.

Example:

stage('Hello')
Step

The actual command/action performed inside a stage.

Example:

echo 'Hello from Jenkins!'
9. Build

A build is an execution of the Jenkins Pipeline.

Used:

Build Now

to manually start the Pipeline.

Jenkins then creates a build number such as:

#1
#2
10. Console Output

Console Output shows the logs generated while Jenkins executes a build.

Example:

[Pipeline] Start of Pipeline
[Pipeline] node
...

It helps in understanding whether the Pipeline succeeded or failed.

11. Jenkins Executor

An executor is a slot where Jenkins can run a build.

If no executor is available, Jenkins may show:

Still waiting to schedule task
Waiting for next available executor

This means Jenkins currently does not have an available node/executor to run the Pipeline.

12. Built-In Node

The Built-In Node is the Jenkins controller's own node.

It can execute builds on the same machine where Jenkins is running.

During the practice, the Built-In Node became offline, which caused the Pipeline to remain in the queue.

13. Jenkins CI/CD Flow

Basic CI/CD flow:

Developer
    ↓
GitHub
    ↓
Jenkins
    ↓
Build
    ↓
Test
    ↓
Deploy

Jenkins can automate these steps.

14. AWS Cleanup

After completing the Jenkins practice, I cleaned up the AWS resources to avoid unnecessary charges.

Checked:

EC2 Instance
Elastic IP
EBS Volumes
AWS Billing

The EC2 instance was terminated.

There were no Elastic IP addresses allocated.

EBS volumes were checked.

15. AWS Billing

Checked:

Billing and Cost Management → Bills

Current estimated bill:

USD 0.00

So there were no charges shown for the practice.

Key Learnings
EC2 provides virtual servers in AWS.
Security Groups act as virtual firewalls.
Jenkins is a CI/CD automation tool.
Jenkins can run on an EC2 instance.
Jenkins Pipelines are written using Groovy syntax.
A Pipeline contains stages and steps.
Builds are executed using executors on nodes.
Console Output helps debug Jenkins builds.
AWS resources should be cleaned up after practice to avoid unnecessary costs