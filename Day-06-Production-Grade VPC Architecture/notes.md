Project — Overall Architecture

Production environment mein secure application deployment ke liye architecture:

                    INTERNET
                       |
                Internet Gateway
                       |
              +--------+--------+
              |                 |
        Public Subnet A   Public Subnet B
              |                 |
        Load Balancer      Load Balancer
              |
        -------------------------
        |                       |
   Private Subnet A       Private Subnet B
        |                       |
      EC2                    EC2
        |                       |
        +----------+------------+
                   |
              NAT Gateway
                   |
               INTERNET
Main idea:
Public Subnet → Load Balancer, NAT Gateway
Private Subnet → Application EC2 instances
Internet Gateway (IGW) → Internet se communication
NAT Gateway → Private EC2 ko internet access
Load Balancer → Incoming traffic ko EC2 instances mein distribute karta hai
Auto Scaling Group → EC2 instances ko automatically increase/decrease karta hai
Bastion Host → Private EC2 mein securely SSH karne ke liye
2. Why 2 Availability Zones?

Production environment mein generally 2 Availability Zones (AZs) use karte hain.

Example:

Region
 |
 +-- AZ-1
 |    +-- Public Subnet
 |    +-- Private Subnet
 |
 +-- AZ-2
      +-- Public Subnet
      +-- Private Subnet
Why?

Agar ek Availability Zone fail ho jaye, doosri AZ application ko serve kar sakti hai.

Benefit:

High Availability + Fault Tolerance

3. Public Subnet

Public subnet mein generally woh resources hote hain jinko internet se directly communicate karna hota hai.

Day 7 architecture mein:

Load Balancer
NAT Gateway
Bastion Host
Public subnet ka route:
Public Subnet
      |
Route Table
      |
Internet Gateway
      |
Internet
4. Private Subnet

Private subnet mein application servers/EC2 instances rakhe jaate hain.

Important:

Private EC2:

Public IP nahi hota
Internet se directly accessible nahi hota
Direct SSH nahi kar sakte

Ye security improve karta hai.

Internet
   ❌
   |
Private EC2
5. NAT Gateway
NAT = Network Address Translation

NAT Gateway ka main purpose:

Private subnet ke resources ko internet access provide karna without exposing them directly to the internet.

Example:

Private EC2
10.0.1.10
    |
    v
NAT Gateway
Public IP
    |
    v
Internet

Private EC2 jab internet par request bhejta hai, NAT Gateway uski private IP ko apni public IP se represent karta hai.

Important:

NAT Gateway allows:

Private EC2 → Internet ✅

But normally:

Internet → Private EC2 ❌
6. Elastic IP

Elastic IP = Static Public IPv4 address

Normally public IP change ho sakti hai.

Elastic IP use karne par IP address static rehta hai.

Day 7 project mein:

Elastic IP
     |
NAT Gateway

NAT Gateway ko Elastic IP assign kiya gaya.

7. Auto Scaling Group (ASG)
Definition:

Auto Scaling Group automatically EC2 instances ki capacity manage karta hai.

Example:

Initially:

EC2 1
EC2 2

Traffic increase hua:

EC2 1
EC2 2
EC2 3
EC2 4

Traffic decrease hua:

EC2 1
EC2 2
Important terms:

Desired Capacity
→ Normally kitne instances chahiye.

Minimum Capacity
→ Minimum instances.

Maximum Capacity
→ Maximum instances.

Example:

Minimum = 2
Desired = 2
Maximum = 4
8. Launch Template

Auto Scaling Group directly EC2 configuration nahi banata.

Launch Template EC2 configuration ka blueprint/reference hota hai.

Isme define kar sakte ho:

AMI
Instance type
Key pair
Security Group
Other EC2 configurations
Launch Template
       |
       v
Auto Scaling Group
       |
   +---+---+
   |       |
  EC2     EC2
Simple definition:

Launch Template tells ASG how to create EC2 instances.

9. Load Balancer

Load Balancer ka kaam:

Incoming traffic ko multiple servers/EC2 instances ke beech distribute karna.

Example:

        100 Requests
              |
        Load Balancer
          /       \
         /         \
     EC2-1        EC2-2
      50%           50%

Ya workload ke according:

60% → EC2-1
40% → EC2-2
Benefits:
Load distribution
High availability
Scalability
Multiple routing mechanisms
10. Application Load Balancer (ALB)

Day 7 mein Application Load Balancer use kiya gaya.

ALB:

Layer 7 load balancer hai.

Ye primarily:

HTTP
HTTPS

traffic handle karta hai.

ALB generally:
Internet
    |
Internet Gateway
    |
Application Load Balancer
    |
Target Group
    |
EC2 Instances
11. Target Group

Target Group define karta hai:

Load Balancer ko kaunse EC2 instances ko traffic bhejna hai.

Example:

Load Balancer
      |
Target Group
   /       \
 EC2-1    EC2-2

Target Group mein targets ho sakte hain:

EC2 instances
etc. depending on configuration
12. Health Check

Load Balancer/Target Group check karta hai ki EC2 healthy hai ya nahi.

Example:

EC2-1 → Healthy ✅
EC2-2 → Unhealthy ❌

Then traffic:

Load Balancer
      |
      v
    EC2-1 ✅

Unhealthy instance ko traffic normally nahi bheja jaata.

Example:

Application Port = 8000

Health Check:

HTTP : 8000
13. Bastion Host / Jump Host

Private EC2 mein directly SSH nahi kar sakte because it has no public IP.

Solution:

Bastion Host

Bastion Host public subnet mein hota hai.

Your Laptop
     |
     | SSH
     v
Bastion Host
(Public Subnet)
     |
     | SSH
     v
Private EC2
(Private Subnet)
Bastion ka purpose:
Private servers ko securely access karna
SSH access ka central point
Logging/Auditing possible
Key concept:

Bastion Host = Jump point to private servers

14. SCP

Day 7 practical mein .pem file Bastion Host par copy karne ke liye SCP use hua.

SCP:

Secure Copy Protocol

Purpose:

Ek machine se doosri machine par securely file copy karna.

Example:

Your Laptop
     |
    SCP
     |
     v
Bastion Host
15. Complete Traffic Flow
User → Application
User
 |
 v
Internet
 |
 v
Internet Gateway
 |
 v
Load Balancer
 |
 v
Target Group
 |
 +--------+
 |        |
 v        v
EC2-1    EC2-2
Private Subnets
Private EC2 → Internet
Private EC2
     |
     v
Route Table
     |
     v
NAT Gateway
     |
     v
Internet Gateway
     |
     v
Internet
16. Route Table

Route Table decide karta hai:

Traffic ko kahan jaana chahiye?

Public subnet:
0.0.0.0/0
     ↓
Internet Gateway
Private subnet:
0.0.0.0/0
     ↓
NAT Gateway

Simple difference:

Subnet	Internet Route
Public	Internet Gateway
Private	NAT Gateway
17. Security Groups

Security Group EC2/Load Balancer ke liye virtual firewall ki tarah kaam karta hai.

Day 7 example:

EC2:
SSH → Port 22
App → Port 8000
Load Balancer:
HTTP → Port 80
Important principle:

Only required ports open karo.

Unnecessarily:

All Traffic → Anywhere ❌

open karna security risk hai.

18. Why Application is in Private Subnet?

Production architecture mein application servers ko directly internet se expose nahi karna chahiye.

Instead:

Internet
   |
Load Balancer
   |
Private EC2

Benefits:

Better security
No direct public access
Centralized traffic through Load Balancer
19. Why Load Balancer in Public Subnet?

Because users internet se application access karenge.

Therefore:

Internet
   ↓
Internet Gateway
   ↓
Public Load Balancer
   ↓
Private EC2
20. Day 7 Practical — Main Steps
Step 1

Create VPC.

Step 2

Create:

2 Public Subnets
2 Private Subnets
Across 2 AZs
Step 3

Configure:

Route Tables
Internet Gateway
NAT Gateway
Step 4

Create Launch Template.

Step 5

Create Auto Scaling Group.

Example:

Min = 2
Desired = 2
Max = 4
Step 6

Launch EC2 instances in private subnets.

Step 7

Create Bastion Host in public subnet.

Step 8

SSH:

Laptop → Bastion → Private EC2
Step 9

Deploy application on EC2.

Example:

python3 -m http.server 8000
Step 10

Create Target Group.

Protocol = HTTP
Port = 8000
Step 11

Create Application Load Balancer.

Internet-facing
Public Subnets
Step 12

Attach Target Group.

Step 13

Allow HTTP Port 80 in Load Balancer Security Group.

Step 14

Access ALB DNS name from browser.

⭐ Most Important Interview Points

Ye pakka yaad rakhna:

Public Subnet → Internet Gateway
Private Subnet → NAT Gateway for outbound internet
Application EC2 → Private Subnet
Load Balancer → Public Subnet
Bastion Host → Public Subnet
Bastion → SSH access to Private EC2
ASG → Automatically manages EC2 capacity
Launch Template → Blueprint for EC2
Target Group → Group of backend targets
ALB → Layer 7
ALB → HTTP/HTTPS
Health Check → Determines healthy/unhealthy targets
2 AZs → High Availability
Elastic IP → Static public IP
SCP → Securely copies files
Security Group → Virtual firewall
Private EC2 has no direct public access
Internet → IGW → ALB → Private EC2
Private EC2 → NAT Gateway → IGW → Internet
Production architecture = secure + highly available + scalable
🧠 One-line architecture to memorize

Internet → IGW → Public ALB → Target Group → Private EC2, while Private EC2 uses NAT Gateway for outbound internet and Bastion Host for administrative SSH access.



=====================================================================================

Q1. Production application ko 2 Availability Zones (AZs) mein deploy karne ka main reason kya hai?
✅ Answer:

High Availability + Fault Tolerance

Reason:

Agar ek Availability Zone fail ho jaye, to doosri AZ me running instances application ko serve karte rahenge.

Load Balancer
   ↓
AZ-1 ❌
AZ-2 ✅ → Traffic

Remember:

Multiple AZ → High Availability
Load Balancer → Traffic Distribution
Q2. NAT Gateway ka purpose kya hai?
✅ Answer:

Private subnet ke resources ko outbound Internet access dena without giving them a public IP.

Private EC2
    ↓
NAT Gateway
    ↓
Internet Gateway
    ↓
Internet
Reason:

Private EC2 directly Internet par nahi ja sakta, isliye NAT Gateway uske behalf par Internet request bhejta hai.

Remember:

NAT = Private EC2 → Internet

Q3. Bastion Host kya hota hai?
✅ Answer:

Bastion Host ek server hai jo public subnet me hota hai aur administrator ko private EC2 tak securely access karne deta hai.

Admin Laptop
     ↓ SSH
Bastion Host
     ↓
Private EC2
Reason:

Private EC2 ka public IP nahi hota, isliye administrator Bastion Host ke through SSH access kar sakta hai.

Remember:

Bastion = Admin → Private EC2

Q4. Public Subnet aur Private Subnet me difference kya hai?
✅ Answer:

Public Subnet: Internet Gateway ke through Internet connectivity possible hoti hai.

Private Subnet: Direct Internet connectivity nahi hoti.

Public:
Subnet → Internet Gateway → Internet

Private:
Subnet → No direct Internet Gateway route
Reason:

Public subnet me route table Internet Gateway ki taraf route provide karti hai, jabki private subnet me direct IGW route nahi hota.

Q5. Load Balancer ka main purpose kya hai?
✅ Answer:

Load Balancer incoming traffic ko multiple servers/EC2 instances ke beech distribute karta hai.

Users
  ↓
Load Balancer
 ├── EC2-1
 ├── EC2-2
 └── EC2-3
Reason:

Agar saari requests ek hi server par aa jayein, to server overloaded ho sakta hai. Load Balancer traffic distribute karta hai.

Remember:

Load Balancer = Traffic Distribution

Q6. Launch Template kya hota hai?
✅ Answer:

Launch Template EC2 configuration ka blueprint hota hai.

Isme define ho sakta hai:

AMI
Instance Type
Key Pair
Security Group
Other configuration
Reason:

Auto Scaling Group ko jab naya EC2 create karna hota hai, to Launch Template se configuration milti hai.

Launch Template
      ↓
Auto Scaling Group
      ↓
New EC2

Remember:

Launch Template = EC2 Blueprint

Q7. Desired = 2, Minimum = 2, Maximum = 5. Traffic badhne par ASG kya karega?
✅ Answer:

Agar scaling policy trigger hoti hai, ASG EC2 instances ko increase kar sakta hai, maximum 5 tak.

2 → 3 → 4 → 5
Reason:

Maximum capacity 5 hai, isliye ASG 5 se zyada instances nahi create karega.

⚠️ Scaling ke liye scaling policy/metric trigger hona zaroori hai.

Q8. Agar ek EC2 unhealthy ho jaye to Load Balancer kya karega?
✅ Answer:

Load Balancer unhealthy EC2 ko traffic bhejna band kar dega aur healthy instances ko requests bhejega.

EC2-1 ✅
EC2-2 ❌
EC2-3 ✅

Traffic:

LB → EC2-1 / EC2-3
Reason:

Load Balancer health checks perform karta hai aur unhealthy targets ko traffic se remove karta hai.

Q9. Auto Scaling Group aur Load Balancer me difference?
✅ Answer:

Auto Scaling Group:
EC2 instances ki number manage karta hai.

Load Balancer:
Incoming traffic ko available/healthy EC2 instances ke beech distribute karta hai.

Users
  ↓
Load Balancer
  ↓
EC2 EC2 EC2
  ↑
 ASG
Easy Trick:

ASG = Kitne EC2 chahiye?

LB = Request kis EC2 ko bhejni hai?

Q10. Route Table ka kya role hai?
✅ Answer:

Route Table network traffic ka path/direction decide karti hai.

Example:

0.0.0.0/0 → Internet Gateway

ya

0.0.0.0/0 → NAT Gateway
Reason:

Route Table decide karti hai ki traffic ko next destination/target kahan bhejna hai.

Remember:

Route Table = Traffic ka Path

Q11. Internet Gateway kya hota hai?
❌ Tumhara answer:

Security purpose.

✅ Correct Answer:

Internet Gateway VPC ko Internet se connect karta hai.

Internet
   ↕
Internet Gateway
   ↕
Public Subnet
Reason:

Public subnet ki route table Internet Gateway ke through Internet ka route provide kar sakti hai.

Security ke liye:

Security Group
NACL

Remember:

IGW = VPC ↔ Internet connectivity

Q12. Security Group aur NACL me difference?
✅ Answer:

Security Group:

Instance/ENI level
Stateful
Allow rules

NACL:

Subnet level
Stateless
Allow + Deny rules
Security Group	NACL
Instance level	Subnet level
Stateful	Stateless
Allow	Allow + Deny
Easy Trick:

SG = Server/Instance Security

NACL = Network/Subnet Security

Q13. EC2 ko Private Subnet me kyun rakhte hain?
✅ Answer:

Direct Internet exposure ko avoid karne aur security improve karne ke liye.

Architecture:

Internet
   ↓
Load Balancer
   ↓
Private EC2
   ↓
Database
Reason:

Users directly EC2 ko access nahi karte. Public Load Balancer request receive karta hai aur private EC2 ko forward karta hai.

Q14. Private EC2 ko Internet par request bhejni ho to kya use karega?
❌ Tumhara answer:

Bastion

✅ Correct:

NAT Gateway

Private EC2
    ↓
NAT Gateway
    ↓
Internet Gateway
    ↓
Internet
Reason:

NAT Gateway private resources ko outbound Internet access deta hai without public IP.

Q15. Load Balancer Public Subnet me aur EC2 Private Subnet me kyun?
✅ Answer:

Load Balancer → Public Subnet

Because Internet users ko requests bhejni hain.

EC2 → Private Subnet

Because application servers ko directly Internet se expose nahi karna.

Internet
   ↓
Public Load Balancer
   ↓
Private EC2
Reason:

Is architecture se application servers ka direct Internet exposure reduce hota hai.

Q16. https://myapp.com open karne par request ka flow kya hai?
✅ Answer:
User
 ↓
Route 53 / DNS
 ↓
Load Balancer
 ↓
Private EC2
 ↓
Application
Step-by-step:
User myapp.com enter karta hai.
DNS domain ko appropriate destination tak resolve karta hai.
Request Load Balancer tak pahunchti hai.
Load Balancer healthy EC2 choose karta hai.
Request private EC2 ko forward hoti hai.
EC2 application request process karti hai.
Response same path se user ko wapas milta hai.
Remember:

DNS finds destination → Load Balancer distributes → EC2 processes

Q17. Desired, Minimum aur Maximum Capacity kya hain?

Example:

Minimum = 2
Desired = 2
Maximum = 5
Minimum

ASG normally 2 se neeche scale nahi karega.

Desired

ASG normally 2 instances maintain karne ki koshish karega.

Maximum

Scaling out ke time 5 se zyada instances nahi honge.

Memory Trick:

Minimum = Minimum kitne?

Desired = Normally kitne?

Maximum = Maximum kitne?

Q18. Health Check kya hota hai?
✅ Answer:

Health Check EC2/target ki health check karta hai.

Load Balancer
 ├── EC2-1 ✅
 ├── EC2-2 ❌
 └── EC2-3 ✅

Agar EC2 unhealthy hai, Load Balancer usko traffic bhejna band kar deta hai.

Reason:

Traffic sirf healthy targets ko serve karwana hai.

Q19. Agar ek Availability Zone completely down ho jaye?
✅ Answer:

Load Balancer healthy instances wali doosri AZ me traffic route karega.

AZ-1 ❌
AZ-2 ✅
      ↑
   Traffic
Reason:

Multiple AZs use karne se High Availability + Fault Tolerance milti hai.

Q20. NAT Gateway aur Bastion Host me difference?
NAT Gateway:
Private EC2
    ↓
NAT Gateway
    ↓
Internet

Purpose: Private EC2 → Internet

Bastion:
Admin
  ↓
Bastion
  ↓
Private EC2

Purpose: Admin → Private EC2

🔥 Golden Rule:

Bastion = Human → Private EC2

NAT = Private EC2 → Internet

Q21. Internet Gateway aur NAT Gateway me difference?
Internet Gateway

VPC ko Internet se connect karta hai.

VPC ↔ Internet
NAT Gateway

Private subnet ke resources ko outbound Internet access deta hai.

Private EC2
 ↓
NAT
 ↓
IGW
 ↓
Internet
Easy Trick:

IGW = Internet ka main Gate

NAT = Private EC2 ka Internet route

Q22. Load Balancer Public aur EC2 Private kyun?
✅ Answer:

Load Balancer ko Internet users ki requests receive karni hoti hain, isliye wo public-facing hota hai.

EC2 ko direct Internet exposure se bachane ke liye private subnet me rakha jata hai.

Internet
   ↓
Load Balancer
   ↓
Private EC2
Q23. Private EC2 ko Internet se package download karna hai. Kya use karega?

Options:

A. Bastion
B. NAT Gateway
C. Load Balancer
D. Route 53

✅ Answer:

B) NAT Gateway

Reason:
Private EC2
 ↓
NAT Gateway
 ↓
Internet Gateway
 ↓
Internet
Q24. Administrator ko Private EC2 par SSH karna hai. Kya use karega?
✅ Answer:

Bastion Host

Admin Laptop
     ↓ SSH
Bastion
     ↓
Private EC2
Reason:

Bastion Host administrative access ke liye use hota hai.

Q25. EC2-2 unhealthy ho gaya. Load Balancer kya karega?
✅ Answer:

Unhealthy EC2 ko traffic bhejna band karega aur healthy EC2 instances ko request bhejega.

EC2-1 ✅ ← Traffic
EC2-2 ❌ ← No Traffic
EC2-3 ✅ ← Traffic
Reason:

Load Balancer health checks ke basis par healthy targets ko traffic route karta hai.

Q26. Minimum = 2, Maximum = 5. Traffic kam ho gaya. ASG kitne tak scale down kar sakta hai?
✅ Answer:

2

Reason:

Minimum capacity 2 hai.

5 → 4 → 3 → 2

2 se neeche normally nahi jayega.

Q27. Launch Template ka main purpose?

Options:

A. Traffic distribute karna
B. EC2 configuration ka blueprint provide karna
C. Private EC2 ko Internet dena
D. DNS resolve karna

✅ Answer:

B

Reason:

Launch Template me EC2 configuration define hoti hai.

Launch Template
 ↓
ASG
 ↓
EC2
Q28. Private EC2 ko apt update karna hai. Architecture?

Options:

A. Private EC2 → Bastion → Internet
B. Private EC2 → NAT Gateway → Internet Gateway → Internet
C. Private EC2 → Load Balancer → Internet
D. Private EC2 → Route 53 → Internet

✅ Answer:

B

Reason:

NAT Gateway private EC2 ko outbound Internet access provide karta hai.

Q29. AZ-1 completely fail ho gayi. Kya hoga?
✅ Answer:

Load Balancer AZ-2 ke healthy EC2 ko traffic route karega.

AZ-1 ❌

AZ-2 ✅
  ↑
Traffic
Reason:

Multiple AZ architecture High Availability + Fault Tolerance provide karti hai.

Q30. Complete Day 7 Architecture
Flow:
                    INTERNET
                       ↓
                Load Balancer
                 Public Subnet
                       ↓
             ┌─────────┴─────────┐
             ↓                   ↓
           AZ-1                AZ-2
        Private EC2          Private EC2
             ↓                   ↓
             └─────────┬─────────┘
                       ↓
                    Database
Components:

Load Balancer
→ Incoming traffic distribute karta hai.

Auto Scaling Group
→ EC2 instances ki number automatically manage karta hai.

Launch Template
→ EC2 configuration ka blueprint.

Private EC2
→ Application securely run karta hai.

NAT Gateway
→ Private EC2 ko outbound Internet access.

Bastion Host
→ Admin ko private EC2 access.

Route Table
→ Traffic ka path decide karti hai.

Security Group
→ Instance-level traffic control.

Multiple AZs
→ High Availability + Fault Tolerance.

Q31. EC2 ko directly Public Subnet me kyun nahi rakhte?
✅ Answer:

Because application EC2 ko directly Internet par expose karne se attack surface aur security risk increase hota hai.

Better architecture:

Internet
   ↓
Load Balancer
   ↓
Private EC2
Interview Answer:

We keep EC2 instances in private subnets to prevent direct Internet exposure and reduce the attack surface. The public Load Balancer receives user requests and forwards them to private EC2 instances.

Q32. Public Load Balancer Private EC2 ko request kaise bhejta hai?
❌ Wrong:

NAT Gateway

✅ Correct:

VPC ke internal network ke through directly.

Internet
   ↓
Load Balancer
   ↓
VPC Internal Network
   ↓
Private EC2
Reason:

NAT Gateway ki requirement nahi hai because Load Balancer aur EC2 same VPC ke internal network me communicate kar sakte hain.

Q33. Private EC2 → Database ke liye NAT Gateway chahiye?
❌ Wrong:

NAT Gateway

✅ Correct:

No.

Agar EC2 aur Database same VPC me hain, to private network ke through communicate kar sakte hain.

Private EC2
     ↓
VPC Internal Network
     ↓
Database
Reason:

NAT Gateway sirf Internet-bound traffic ke liye required hota hai.

Q34. EC2 Database se connect nahi kar pa raha. Sabse pehle kya check karoge?
✅ Answer:

Database ke Security Group ke inbound rules.

Example MySQL:

Database Security Group

Inbound:
MySQL
Port: 3306
Source: EC2 Security Group
Reason:

Database ko allow karna hoga ki EC2 Security Group se required database port par traffic aa sake.

❌ Generally database ko:

0.0.0.0/0

se open nahi karna chahiye.

Instead:

EC2 Security Group
       ↓
Database Security Group

allow karna better approach hai.

Q35. MySQL ka default port kya hai?
✅ Answer:

3306

Important ports:
Service	Port
SSH	22
HTTP	80
HTTPS	443
MySQL	3306
PostgreSQL	5432
🔥 FINAL DAY 7 CHEAT SHEET

Isko ye wala section definitely copy kar lena:

AZ
→ High Availability + Fault Tolerance

Load Balancer
→ Incoming traffic distribute karta hai

ASG
→ EC2 instances ki number manage karta hai

Launch Template
→ EC2 configuration ka blueprint

Public Subnet
→ Internet Gateway ke through Internet connectivity

Private Subnet
→ Direct Internet access nahi

Internet Gateway
→ VPC ↔ Internet connectivity

NAT Gateway
→ Private EC2 → Internet

Bastion Host
→ Admin → Private EC2

Route Table
→ Traffic ka path/direction

Security Group
→ Instance-level traffic control

NACL
→ Subnet-level traffic control

Health Check
→ EC2/target healthy hai ya nahi check karta hai

Multiple AZ
→ High Availability

EC2 → Database
→ VPC internal network; NAT required nahi

Load Balancer → Private EC2
→ VPC internal network; NAT required nahi

Private EC2 → Internet
→ NAT Gateway required

Admin → Private EC2
→ Bastion Host

MySQL
→ Port 3306
🧠 Sabse important 5 lines

LB = Traffic distribute

ASG = EC2 count manage

NAT = Private EC2 → Internet

Bastion = Admin → Private EC2

Multiple AZ = High Availability