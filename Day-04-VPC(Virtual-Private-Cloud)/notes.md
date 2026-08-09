AWS VPC — Easy Notes
1. VPC kya hai?

VPC = Virtual Private Cloud

VPC AWS ke andar tumhara private network hota hai jisme tum apne AWS resources jaise:

EC2
Load Balancer
Database
etc.

ko organize aur secure karte ho.

Simple example

Socho:

AWS Region = Ek huge city
VPC = Tumhari secure colony
Subnet = Colony ke andar alag blocks
EC2 = Tumhare houses
Internet Gateway = Colony ka main gate
Route Table = Road/map jo traffic ko direction deta hai
Security Group = Security guard

Why do we need VPC?

VPC allows us to control:

IP address ranges
Network segmentation
Internet connectivity
Routing
Security
Access between resources

2. VPC ka size kaise decide hota hai?

VPC ko IP Address Range / CIDR block se define karte hain.

Example:

172.16.0.0/16

Iska matlab VPC ke paas ek large IP address range available hai.

Tum is range ko further divide kar sakte ho.

Example:

VPC
172.16.0.0/16
       |
       |---- Subnet A → 172.16.1.0/24
       |
       |---- Subnet B → 172.16.2.0/24
       |
       |---- Subnet C → 172.16.3.0/24
Important

VPC → large network

Subnet → VPC ke andar smaller network

Important

VPC size is defined using its CIDR block.

3. Subnet

Subnet ka full meaning:

Sub-network

VPC ko smaller networks mein divide karne ke liye subnet use hota hai.

Example:

VPC
172.16.0.0/16

   ├── Public Subnet
   ├── Private Subnet
   └── Private Subnet
Public Subnet

Jahan resources ko internet se accessible banaya ja sakta hai.

Example:

Load Balancer
Private Subnet

Jahan resources directly internet se accessible nahi hone chahiye.

Example:

EC2 Application Server
Database

Placement ke liye yaad rakho:

Public subnet = Internet-facing resources
Private subnet = Internal/protected resources

Public Subnet

A public subnet is a subnet whose route table has a route to an Internet Gateway.

It is generally used for resources that need to be internet-facing.

Examples:

Load Balancer
Bastion Host
Public-facing resources
Important

A subnet is not public merely because it is named "Public Subnet."

Its routing configuration determines whether it is public.


Private Subnet

A private subnet does not have a direct route to the Internet Gateway.

It is generally used for resources that should not be directly reachable from the internet.

Examples:

Application servers
Databases
Internal services

Example:

Internet
   ↓
Load Balancer
   ↓
Private Subnet
   ↓
EC2

4. Internet Gateway

Internet Gateway (IGW) VPC ko internet se connect karta hai.

Basic flow:

Internet
   ↓
Internet Gateway
   ↓
VPC

Lekin sirf Internet Gateway bana dene se subnet automatically public nahi ban jata.

Subnet ko appropriate route table ke through internet gateway ka route dena hota hai.

5. Route Table

Route Table basically decide karta hai:

"Traffic ko kahan jaana hai?"

Example:

Destination        Target
0.0.0.0/0          Internet Gateway

Meaning:

Agar traffic kisi bhi external destination ke liye hai, Internet Gateway ke through bhejo.

Simple analogy:

Route Table = Google Maps

Ye traffic ko direction deta hai.

6. Load Balancer

Load Balancer incoming requests ko multiple servers ke beech distribute karta hai.

Example:

             User
               ↓
        Load Balancer
          ↙    ↓    ↘
       EC2-1 EC2-2 EC2-3

Agar 100 users aaye:

User requests
      ↓
Load Balancer
   ↙   ↓   ↘
 EC2  EC2  EC2

Isse ek single server par unnecessary load nahi padta.
Benefits:

Better availability
Better scalability
Traffic distribution
Fault tolerance

7. Target Group

Load Balancer ko pata hona chahiye ki request kis servers ko bhejni hai.

Ye information Target Group mein hoti hai.

Example:

Load Balancer
      ↓
Target Group
   ↓    ↓    ↓
 EC2   EC2   EC2
8. Security Group

Security Group EC2/resource ke liye virtual firewall ki tarah kaam karta hai.

It controls:

Inbound traffic
Outbound traffic
Ports
Sources/destinations

Example:

Security Group

SSH    → Port 22  → Allow
HTTP   → Port 80  → Allow
HTTPS  → Port 443 → Allow

If port 22 is not allowed, SSH traffic will be blocked.

Ye decide karta hai:

Kaunsa traffic allow hai?
Kaunsa traffic deny hai?
Kaunsa port allowed hai?
Kis source se traffic allowed hai?

Example:

Port 22  → SSH
Port 80  → HTTP
Port 443 → HTTPS

Suppose:

Security Group

22  → Allow
80  → Allow
443 → Allow
3306 → Deny

To EC2 par sirf allowed traffic aa sakega.

Interview definition

Security Group is a virtual firewall that controls inbound and outbound traffic for AWS resources such as EC2.

Important Properties

Security Groups are:

Stateful
Associated with resources such as EC2
Primarily used to control inbound and outbound traffic
Interview Definition

A Security Group is a stateful virtual firewall that controls inbound and outbound traffic for AWS resources.

9. NACL

NACL = Network Access Control List

Ye subnet level par traffic control karta hai.

Simple difference:

Security Group	NACL
Resource level	Subnet level
Stateful	Stateless
Allow rules	Allow + Deny rules
EC2 ke saath commonly used	Subnet ke saath
Easy trick

SG → Server ki security

NACL → Subnet ki security

Easy Trick

Security Group → Resource security

NACL → Subnet security

10. NAT Gateway

Ye concept important hai.

Suppose:

Internet
   ↑
   |
Private EC2

Private EC2 ko internet se package download karna hai.

Example:

EC2 → download package from internet

Lekin EC2 private subnet mein hai.

Yahan NAT Gateway useful hota hai.

Typical architecture:

                 Internet
                    ↑
                    |
             Internet Gateway
                    |
             Public Subnet
                    |
              NAT Gateway
                    |
             Private Subnet
                    |
                   EC2
NAT Gateway ka main purpose

Private subnet ke resources ko outbound internet access provide karna while preventing unsolicited inbound internet connections to those private resources.

Easy definition

NAT Gateway allows resources in a private subnet to access the internet without making those resources directly reachable from the internet.

Important

NAT Gateway is mainly used for outbound internet access from private subnets.

11. VPC Flow Logs

VPC Flow Logs traffic information ko record karne ke liye use hote hain.

They are useful for:

Troubleshooting
Security analysis
Monitoring network traffic
Understanding accepted/rejected traffic

Simple meaning:

VPC Flow Logs = Network traffic ka record

Ye debugging/monitoring mein useful hai.

Example:

Who sent traffic?
Where did it go?
Was traffic accepted/rejected?

So:

VPC Flow Logs = Network traffic ka record

⭐ Complete VPC Architecture

Ab sabko ek saath connect karo:

                         INTERNET
                            |
                            ↓
                  Internet Gateway
                            |
                            ↓
                  ┌─────────────────┐
                  │       VPC       │
                  │                 │
                  │  Public Subnet  │
                  │       |         │
                  │  Load Balancer  │
                  │       |         │
                  │       ↓         │
                  │  Route Table    │
                  │       |         │
                  │       ↓         │
                  │ Private Subnet  │
                  │       |         │
                  │      EC2        │
                  │       |         │
                  │ Security Group  │
                  │                 │
                  └─────────────────┘

Private EC2 ko internet se kuch download karna ho:

Private EC2
    ↓
NAT Gateway
    ↓
Internet Gateway
    ↓
Internet
🔥 Most Important Request Flow

Agar user internet se application access karta hai:

User
 ↓
Internet
 ↓
Internet Gateway
 ↓
Public Subnet
 ↓
Load Balancer
 ↓
Target Group
 ↓
Route Table
 ↓
Private Subnet
 ↓
Security Group
 ↓
EC2
 ↓
Application

Ye flow tumhe interview ke liye definitely samajhna hai.

🧠 Components ko ek line mein yaad karo
Component	Simple Meaning
VPC	AWS ka private network
CIDR	VPC ka IP range
Subnet	VPC ka smaller network
Public Subnet	Internet-facing resources
Private Subnet	Protected/internal resources
Internet Gateway	VPC ↔ Internet connection
Route Table	Traffic ka direction
Load Balancer	Traffic distribute karta hai
Target Group	LB ke backend targets
Security Group	Resource-level firewall
NACL	Subnet-level firewall
NAT Gateway	Private resources ko outbound internet
VPC Flow Logs	Network traffic records
⚠️ Ek important correction

Lecture mein kuch explanations oversimplified hain. Placement/interview ke liye ye accurate understanding rakho:

Public subnet ka matlab ye nahi ki user automatically subnet mein "enter" karta hai.

Actually:

Subnet is considered public when its route table has a route to an Internet Gateway, and the resource also has the necessary public addressing/configuration.

Similarly, NAT Gateway private subnet ko inbound access nahi deta; primarily private resources ke outbound connections ke liye use hota hai.

🎯 Tumhare AWS roadmap ke liye

Is lecture se abhi tumhe ye 9 concepts pakke karne hain:

VPC → CIDR → Subnet → Public/Private → Internet Gateway → Route Table → Security Group → NAT Gateway → NACL

VPC Flow Logs ko basic level par rakhna enough hai abhi.

Practical mein inko ek actual architecture bana ke connect karoge to VPC ka concept aur clear ho jayega.


🧠 One-Line Revision
Component	Remember This
VPC	Private network in AWS
CIDR	Defines VPC IP range
Subnet	Smaller network inside VPC
Public Subnet	Has route to Internet Gateway
Private Subnet	No direct route to Internet Gateway
Internet Gateway	VPC ↔ Internet
Route Table	Decides traffic path
Load Balancer	Distributes traffic
Target Group	Backend targets for Load Balancer
Security Group	Stateful resource-level firewall
NACL	Stateless subnet-level firewall
NAT Gateway	Private subnet → outbound internet
VPC Flow Logs	Records network traffic



🔥 Interview Questions You Must Know
Q1. What is VPC?

VPC is a logically isolated virtual network in AWS where we can launch and manage AWS resources securely.

Q2. What is a subnet?

A subnet is a smaller network created inside a VPC.

Q3. Difference between public and private subnet?

A public subnet has a route to an Internet Gateway, while a private subnet does not have a direct route to an Internet Gateway.

Q4. What is an Internet Gateway?

It provides connectivity between a VPC and the internet.

Q5. What is a Route Table?

A Route Table contains rules that determine where network traffic should be directed.

Q6. What is a Security Group?

A Security Group is a stateful virtual firewall that controls inbound and outbound traffic for AWS resources.

Q7. Security Group vs NACL?

Security Group works at the resource level and is stateful, whereas NACL works at the subnet level and is stateless.

Q8. Why do we use NAT Gateway?

To allow resources in private subnets to access the internet for outbound connections without making them directly internet-facing.

Q9. What is VPC Flow Logs?

VPC Flow Logs capture information about network traffic to and from network interfaces in a VPC.

🎯 Tumhare liye abhi kya yaad hona chahiye?

Sabse important:

VPC
 ↓
Subnet
 ↓
Public / Private
 ↓
Internet Gateway
 ↓
Route Table
 ↓
Security Group
 ↓
NAT Gateway

Aur ye 2 flows bina dekhe explain kar pao:

Internet → IGW → Public Subnet → Load Balancer → Private Subnet → EC2
Private EC2 → NAT Gateway → IGW → Internet

Itna VPC ka foundation placement ke liye strong hai. Practical karte waqt ye concepts aur solid ho jayenge.



Most important takeaway:

Security Group → Instance level → Allow rules → Stateful
NACL → Subnet level → Allow + Deny → Stateless


📒 AWS Day 5 – MCQ Practice Notes
Q1. Security Group aur NACL mein sabse important difference kya hai?

A) Security Group subnet level par hota hai, NACL instance level par
B) Security Group instance level par hota hai, NACL subnet level par
C) Dono subnet level par hote hain
D) Dono instance level par hote hain

✅ Correct Answer: B

Reason:

Security Group → EC2 instance level
NACL → Subnet level
Q2. Security Group Port 8000 ko ALLOW kar raha hai, lekin NACL Port 8000 ko DENY kar raha hai. User kya karega?

A) Access milega
B) Access nahi milega
C) Kabhi milega, kabhi nahi
D) AWS automatically Security Group ko priority dega

✅ Correct Answer: B

Reason:
NACL subnet level par traffic ko DENY kar raha hai. Security Group mein ALLOW hone ke baad bhi traffic application tak nahi pahunch sakta.

NACL DENY → Traffic blocked

Q3. Security Group mein by default kya hota hai?

A) Inbound allow, Outbound deny
B) Inbound deny, Outbound allow
C) Inbound aur Outbound dono deny
D) Inbound aur Outbound dono allow

✅ Correct Answer: B

Reason:
Default Security Group mein:

Inbound → Deny
Outbound → Allow

Required inbound traffic ko explicitly allow karna padta hai.

Q4. NACL mein ye rules hain:
Rule 100 → Port 8000 → DENY
Rule 200 → Port 8000 → ALLOW

Port 8000 ki request ka kya hoga?

A) Allow hoga
B) Deny hoga
C) Dono rules apply honge
D) Security Group decide karega

✅ Correct Answer: B

Reason:
NACL rules lowest rule number se evaluate hote hain.

100 → DENY

Rule 100 par request match ho gayi, isliye rule 200 check nahi hoga.

Lower rule number = Higher priority

Q5. Security Group Stateful aur NACL Stateless kyun kaha jata hai?

A) Security Group automatically return traffic allow karta hai, NACL mein return traffic ke liye separate rule chahiye
B) NACL automatically return traffic allow karta hai, Security Group mein separate rule chahiye
C) Dono stateful hain
D) Dono stateless hain

✅ Correct Answer: A

Reason:

Security Group → Stateful

Agar inbound request allow hai, to uska return traffic automatically allowed hota hai.

NACL → Stateless

Inbound aur outbound traffic ko separately configure karna padta hai.

Q6. Python application Port 8000 par running hai. Security Group mein sirf Port 22 aur Port 80 allowed hain. User Port 8000 access karta hai. Kya hoga?

A) Application open hogi
B) Connection timeout / access fail hoga
C) SSH connection fail hoga
D) NACL automatically Port 8000 allow kar dega

✅ Correct Answer: B

Reason:
Application Port 8000 par running hai, lekin Security Group Port 8000 ko allow nahi kar raha.

Python App → Port 8000 ✅
Security Group → Port 8000 ❌

Therefore, traffic block ho jayega.

Q7. NACL aur Security Group dono Port 8000 allow kar rahe hain, Python app bhi Port 8000 par running hai aur EC2 ke paas Public IP bhi hai. Phir bhi application access nahi ho rahi. Sabse pehle kya check karoge?

A) Python application actually Port 8000 par running hai ya nahi
B) Laptop ka wallpaper
C) AWS account ka password
D) VPC ka naam

✅ Correct Answer: A

Reason:
Agar networking configuration correct hai, to next check karna chahiye ki application/service actually running hai ya nahi.

Example:

python3 -m http.server 8000
Q8. NACL mein * (star) rule ka kya meaning hota hai?

A) Sabhi traffic ko automatically ALLOW karta hai
B) Previous rules match na hone par default DENY
C) Sirf HTTP traffic allow karta hai
D) Security Group ko bypass karta hai

✅ Correct Answer: B

Reason:
NACL ke numbered rules pehle check hote hain. Agar traffic kisi rule se match nahi karta, to * rule apply hota hai aur traffic DENY ho jata hai.

NACL * = Default DENY

Q9. Ek EC2 instance private subnet mein hai. User internet se directly EC2 ko access karna chahta hai. Kya reason ho sakta hai?

A) Private subnet ka direct internet route nahi hota
B) Security Group mein Port 22 allow karne se private EC2 automatically public ho jata hai
C) Private subnet ka purpose hi direct internet access ko avoid karna hai
D) A aur C dono

✅ Correct Answer: D

Reason:
Private subnet resources ko normally direct internet inbound access nahi diya jata.

Internet
   ↓
Internet Gateway
   ↓
Public Subnet
   ↓
Load Balancer
   ↓
Private Subnet
   ↓
EC2

Sirf Security Group mein Port 22 allow karne se private EC2 public nahi ban jata.

Q10. Ek subnet mein 50 EC2 instances hain. Tum chahte ho ki sabhi 50 instances par Port 8000 block ho jaye, chahe kisi individual EC2 ke Security Group mein Port 8000 allowed ho. Kya use karoge?

A) Security Group on every EC2
B) NACL on the subnet
C) Internet Gateway
D) Route Table

✅ Correct Answer: B

Reason:
NACL subnet level par apply hota hai.

Agar subnet ke NACL mein:

Port 8000 → DENY

hai, to subnet ke andar ke multiple EC2 instances par ye restriction apply ho sakti hai.

🧠 Day 5 Quick Revision
Concept	Answer
Security Group	Instance level
NACL	Subnet level
Security Group	Stateful
NACL	Stateless
Security Group	Allow rules
NACL	Allow + Deny
SG Default Inbound	Deny
SG Default Outbound	Allow
NACL Priority	Lowest number first
NACL *	Default DENY
SG ALLOW + NACL DENY	❌ Blocked
Multiple EC2s ko subnet level par control	NACL
⭐ Interview Trick

Security Group protects the Instance, while NACL protects the Subnet.