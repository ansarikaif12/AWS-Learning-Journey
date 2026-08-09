# Day 03 - EC2 + Jenkins

EC2 kya hota hai?

EC2 = Elastic Compute Cloud

Simple language mein:

EC2 AWS ka ek virtual computer/server hai jo tum internet ke through rent par lete ho.

Bas ye line pehle samajh lo. 👆

Real-life example 🏠

Maan lo tumhare paas ghar par ek laptop hai:

Tumhara Laptop
├── CPU
├── RAM
├── Storage
└── Operating System

AWS mein tum ek virtual laptop/computer bana sakte ho:

AWS
  ↓
EC2
  ↓
Virtual Computer
├── CPU
├── RAM
├── Storage
└── Operating System

Difference ye hai ki EC2 AWS ke data center mein physically kisi server par chal raha hota hai, tumhare room mein nahi.

Toh EC2 banane ki zarurat kyun?

Maan lo tumne ek website/application banayi:

Tumhari Application
        ↓
     ????

Application ko 24×7 users ke liye available rakhne ke liye usse kisi computer/server par run karna padega.

Tum apna laptop 24×7 chala sakte ho, but practical nahi hai.

Instead:

Your Application
       ↓
     EC2
       ↓
   Internet
       ↓
     Users

EC2 woh computer provide karta hai jahan tumhari application run ho sakti hai.

"Cloud" ka matlab kya hai?

Cloud ka matlab koi magic computer nahi 😂

AWS ke paas bahut saare physical data centers hain.

AWS Data Center
│
├── Physical Server
├── Physical Server
├── Physical Server
├── Physical Server
└── Physical Server

AWS in physical resources ko virtualize karke tumhe ek virtual machine deta hai.

Wahi tumhara:

EC2 Instance

hai.

EC2 Instance kya hota hai?

Ye important hai.

EC2 = AWS ki service

EC2 Instance = us service ke through banaya gaya virtual computer

Example:

EC2
│
├── Instance 1 → Ubuntu
├── Instance 2 → Amazon Linux
└── Instance 3 → Windows

Tumne jo banaya tha:

jenkins-server

woh EC2 instance tha.

Jab tumne EC2 launch kiya tha, tumne kya kiya?

AWS console mein tumne basically AWS ko bola:

"Mujhe ek virtual computer chahiye."

AWS ne poocha:

1. Kaunsa Operating System?

Tumne choose kiya:

Amazon Linux 2023

Matlab virtual computer ke andar Linux OS hoga.

2. Kitna powerful computer?

Tumne instance type choose kiya, jaise:

t3.micro

Iska matlab roughly:

Mujhe itna CPU/RAM wala virtual computer do.

3. Login kaise karunga?

Tumne:

Key pair
aws_login1

select kiya.

Ye basically SSH login ke liye key hai.

4. Internet se kaun connect kar sakta hai?

Yahan aaya:

Security Group

Ye EC2 ka virtual firewall hai.

Example:

Internet
   ↓
Security Group
   ↓
   EC2

Security Group decide karta hai:

Kaunsa traffic EC2 ke andar aa sakta hai?

5. AWS ne tumhe ek address diya

Example:

54.xx.xx.xx

Ye Public IP tha.

Is IP ki help se tum apne computer se EC2 ko access kar sakte ho.

Tumne SSH se kya kiya tha?

Tum Windows PowerShell mein likh rahe the:

ssh -i .\aws_login1.pem ec2-user@54.xx.xx.xx

Iska simple meaning:

"Mere laptop se AWS wale virtual computer mein login karo."

Then:

Your Laptop
     ↓
   Internet
     ↓
 AWS EC2
     ↓
Amazon Linux

Aur jab tumhe ye mila:

[ec2-user@ip-172-31-17-200 ~]$

tum actually AWS ke computer ke andar aa chuke the. 🔥

Ye sabse important realization hai.

Aur Jenkins?

Ab samjho Jenkins humne EC2 par kyun install kiya tha.

EC2:

Computer provide karta hai.

Jenkins:

Software/tool hai jo us computer par run hota hai.

So:

AWS
 ↓
EC2 (Virtual Computer)
 ↓
Amazon Linux
 ↓
Java
 ↓
Jenkins

Jenkins EC2 nahi hai.

Jenkins ek software hai jo EC2 ke andar install hua.

Ekdum simple analogy 🧠

Socho:

AWS = Building owner

EC2 = Tumhara rented room

Amazon Linux = Room ke andar operating system

Jenkins = Room ke andar install kiya hua software

Security Group = Room/building ka security gate

Public IP = Room ka address

SSH = Room mein enter karne ka method

AWS
│
└── EC2 = Virtual Computer
     │
     ├── OS = Amazon Linux
     │
     ├── Java
     │
     └── Jenkins
Bas EC2 ki ek line yaad rakho:

EC2 AWS ki service hai jiske through hum cloud mein virtual computers/servers create karke un par applications aur software run kar sakte hain. ✅

Abhi AMI, EBS, Elastic IP, Load Balancer, Jenkins kuch mat padho.

Pehle mujhe ye batao:

Agar main bolun "Maine AWS mein ek EC2 instance launch kiya", toh practically maine kya create kiya?

Bilkul! 🔥 Exactly.

EC2 launch kiya = AWS ke cloud/data center mein ek virtual computer create kiya. ✅

Aur us virtual computer ke andar tum:

OS install/use kar sakte ho → Ubuntu / Amazon Linux / Windows
Applications install kar sakte ho
Website/application run kar sakte ho
Jenkins jaise tools install kar sakte ho
Commands execute kar sakte ho
Tumhare case mein:
AWS
 ↓
EC2 Instance
 ↓
Virtual Computer
 ↓
Amazon Linux
 ↓
Jenkins

Aur jab tum SSH se connect hue:

Your Windows Laptop
        ↓
      SSH
        ↓
AWS EC2 Virtual Computer

Isliye terminal mein:

[ec2-user@ip-172-31-17-200 ~]$

dikh raha tha — tum apne laptop ke terminal mein nahi, EC2 wale virtual computer ke terminal mein commands chala rahe the. 😎

Ye concept clear ho gaya to EC2 ka 50% confusion khatam. Ab next naturally "EC2 launch karte waqt AMI, Instance Type, Key Pair aur Security Group kya hote hain?" samajhna hai.

Haan bhai! 🔥 Almost 100% correct. Bas Key Pair wali line mein ek important correction hai.

Tumhari understanding:
Component	Tumne bola	Correct?
AMI	Operating System	✅
Instance Type	CPU + RAM batata hai	✅
Key Pair	Login/access ke liye	🟡 Mostly correct
Security Group	Firewall ki tarah	✅
🔑 Key Pair — thoda correction

Key pair kisi bhi application ko access karne ke liye nahi hota.

Ye mainly EC2 instance mein securely login karne ke liye authentication method hai.

Example:

ssh -i aws_login.pem ec2-user@PUBLIC-IP

Yahan:

aws_login.pem → Key Pair ka private key
PUBLIC-IP     → EC2 ka address
ec2-user      → EC2 ka user

Matlab:

Key Pair = EC2 mein securely login karne ki key. 🔐

🛡️ Security Group

Bilkul sahi:

Security Group = EC2 ka virtual firewall.

Example:

Internet
   ↓
Security Group
   ↓
   EC2

Agar port 22 allowed hai → SSH connection aa sakta hai.

Agar port 8080 allowed hai → Jenkins ko browser se access kar sakte ho.

🧠 Ab ekdum final picture:
EC2 = Virtual Computer
│
├── AMI          → Kaunsa OS
│
├── Instance Type → Kitna CPU/RAM
│
├── Key Pair      → EC2 mein login kaise
│
└── Security Group → Kaunsa network traffic allowed

Ye 4 concepts tumhe EC2 launch karte waqt sabse pehle aane chahiye. ✅🔥

Ab next concept naturally EBS (storage) hai — kyunki computer mein CPU/RAM ke saath storage bhi chahiye.


ok but mjhe commands nhi yaad hai aur na hi smjh aayi hai 

Bilkul normal hai bhai. 😄 Commands ratne ki zarurat nahi hai abhi. Pehle ye samjho ki command kyun use hoti hai. Tumne abhi mostly copy-paste kiya tha, isliye yaad nahi rehna natural hai.

Hum EC2 ko 2 parts mein karenge:

Part 1 — Linux basic commands

EC2 ke andar login karne ke baad ye commands sabse important hain:

Command	Meaning
pwd	Main abhi kis folder mein hoon?
ls	Is folder mein kya hai?
cd Downloads	Downloads folder mein jao
cd ..	Ek folder peeche jao
mkdir test	test naam ka folder banao
touch file.txt	Empty file banao
cat file.txt	File ke andar kya hai dekho
rm file.txt	File delete karo
clear	Terminal clean karo

Example:

pwd

→ current location

ls

→ current folder ke andar files/folders

mkdir aws
cd aws

→ aws folder banao aur uske andar jao.

Part 2 — EC2 se related commands

Tumne ye use kiya tha:

ssh -i .\aws_login1.pem ec2-user@13.xxx.xxx.xxx

Isko ratna nahi hai.

Iska meaning:

ssh
 ↓
EC2 mein remotely login karo

-i aws_login1.pem
 ↓
Ye meri authentication key hai

ec2-user
 ↓
EC2 ka login user

13.xxx.xxx.xxx
 ↓
EC2 ka public IP
sudo kya hai?

Ye bhi important hai.

sudo dnf install jenkins

Simple meaning:

Admin/root permission ke saath ye command chalao.

Isliye software install karte waqt sudo frequently dikhega.

dnf kya hai?

Amazon Linux 2023 mein software/package manage karne ke liye dnf use hota hai.

sudo dnf install jenkins

Matlab:

Admin permission ke saath Jenkins software install karo.

Aur:

sudo dnf clean all

Matlab:

DNF ka cached/package data clean karo.

systemctl kya hai?

Services ko control karne ke liye.

sudo systemctl start jenkins

→ Jenkins start karo.

sudo systemctl stop jenkins

→ Jenkins stop karo.

sudo systemctl status jenkins

→ Jenkins chal raha hai ya nahi, check karo.

🔥 Abhi tumhe sirf ye 10 commands yaad/understand karni hain
pwd
ls
cd
cd ..
mkdir
touch
cat
rm
sudo
systemctl

Baaki commands jab actual kaam karenge tab seekhenge.

Aur ek important correction: Ubuntu aur Amazon Linux ke commands 100% same nahi hote. Tumhare current EC2 mein Amazon Linux 2023 tha, isliye dnf use hua. Ubuntu mein usually apt dikhega.


🔥 Bas ye difference abhi pakad lo:

PS C:\Users\acer> = Tumhara Windows
[ec2-user@... ~]$ = AWS EC2

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