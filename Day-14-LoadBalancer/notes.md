AWS LOAD BALANCER — COMPLETE NOTES
1. What is a Load Balancer? ⭐⭐⭐⭐⭐

Load Balancer incoming traffic/requests ko multiple backend servers/EC2 instances ke beech distribute karta hai.

Problem

Agar ek hi EC2 instance hai:

100 Users
    ↓
   EC2
    ↓
Overload

Isse application:

slow ho sakti hai
downtime aa sakta hai
user experience kharab ho sakta hai

Solution:

              EC2-1
             /
Users → Load Balancer → EC2-2
             \
              EC2-3

Multiple EC2 instances same application run karte hain aur Load Balancer requests distribute karta hai. Isse application ki availability improve hoti hai.

2. Why do we use Load Balancer?

Main reasons:

Traffic distribute karna
Single server ka load kam karna
Application availability improve karna
Multiple EC2 instances use karna
Downtime/slowness reduce karna
Highly available application banana
Important:

Load Balancer itself application ko scale nahi karta; ye available servers ke beech traffic distribute karta hai.

3. Basic Load Balancing — Round Robin ⭐⭐⭐⭐

Round Robin ek basic load-balancing technique hai.

Suppose 3 EC2 instances hain:

EC2-1
EC2-2
EC2-3

100 requests hain.

Round Robin approximately requests ko distribute karega:

EC2-1 → 33
EC2-2 → 33
EC2-3 → 33

Isse ek single instance ko saara load handle nahi karna padega.

4. Weight / Ratio-Based Load Balancing ⭐⭐⭐

Agar servers ki capacity same nahi hai, equal traffic distribute karna ideal nahi ho sakta.

Example:

EC2-1 → Powerful
EC2-2 → Medium
EC2-3 → Medium

Tum configure kar sakte ho:

EC2-1 → 50 requests
EC2-2 → 25 requests
EC2-3 → 25 requests

Isse weight/ratio-based load balancing kaha gaya hai transcript me.

Easy:

Powerful server → More traffic

Less powerful server → Less traffic

5. OSI Model — Why important for Load Balancer? ⭐⭐⭐⭐⭐

Transcript me Load Balancer samajhne ke liye OSI ke 7 layers explain kiye gaye hain.

Different Load Balancers different layers par operate karte hain.

Most important:

Layer 7 → Application Load Balancer
Layer 4 → Network Load Balancer

Isliye OSI layers ka basic understanding important hai.

6. OSI 7 Layers — Basic Concept
7 → Application
6 → Presentation
5 → Session
4 → Transport
3 → Network
2 → Data Link
1 → Physical
Layer 7 — Application Layer

Yahan application-level protocols/request information deal hoti hai.

Examples:

HTTP
FTP
SFTP

Browser se website access karte time HTTP request initiate hoti hai.

Browser
 ↓
HTTP Request
 ↓
Application Layer

Transcript me HTTP ko Layer 7 ke main example ke roop me explain kiya gaya hai.

7. Layer 6 — Presentation Layer

Presentation layer ka basic role transcript me:

Encoding
Encryption
Secure communication

SSL/TLS ka concept yahan discuss kiya gaya hai.

HTTP
 ↓
TLS/Encryption
 ↓
Secure Communication

Transcript specifically TLS/SSL ke context me Layer 6 explain karta hai.

8. Layer 5 — Session Layer

Session layer client aur server ke beech session ko manage karti hai.

Session me information ho sakti hai:

Client information
Session creation information
Session-related details

9. Layer 4 — Transport Layer ⭐⭐⭐⭐⭐

Transport Layer = Layer 4

Ye Load Balancer ke context me bahut important hai because:

NLB operates at Layer 4.

Transcript ke explanation me transport layer par data ko smaller chunks/packets me transmit karne aur TCP/UDP traffic ka concept explain kiya gaya hai.

Important protocols:
TCP
UDP
10. Layer 3 — Network Layer

Network Layer = Layer 3

Yahan packets different routers ke through travel karte hain.

Client
 ↓
Router
 ↓
Router
 ↓
Router
 ↓
Server

Transcript me Layer 3 ko routers aur packet routing ke context me explain kiya gaya hai.

11. Layer 2 — Data Link Layer

Data Link Layer = Layer 2

Transcript ke according data center me switches ka role yahan explain kiya gaya hai.

Router
 ↓
Switch
 ↓
Server

12. Layer 1 — Physical Layer

Physical Layer = Layer 1

Yahan actual physical transmission medium aata hai:

Cables
Physical connections
Hardware

13. Application Load Balancer — ALB ⭐⭐⭐⭐⭐

ALB = Application Load Balancer

ALB operates at:

Layer 7 — Application Layer

It mainly deals with:

HTTP
HTTPS

Architecture:
User
 ↓
ALB
 ↓
Target Group
 ↓
EC2 Instances
14. Why ALB is called Application Load Balancer?

Because it can inspect information at the application/HTTP level.

It can look at:

HTTP request
Path
Host
Domain
Headers

Then routing decision le sakta hai.

15. Path-Based Routing ⭐⭐⭐⭐⭐

ALB request ke path ko inspect karke different backend groups ko request bhej sakta hai.

Example:

example.com/payments
        ↓
Payment Target Group
example.com/login
        ↓
Login Target Group
example.com/transactions
        ↓
Transaction Target Group

Ye ALB ka very important feature hai.

16. Host-Based Routing ⭐⭐⭐⭐⭐

ALB host/domain ke basis par bhi routing kar sakta hai.

Example:

payment.example.com
        ↓
Payment Servers
login.example.com
        ↓
Login Servers

Transcript me host/domain-based routing ko ALB ke Layer 7 capabilities ke part ke roop me explain kiya gaya hai.

17. Header-Based / HTTP Request Routing

ALB HTTP request ke headers aur other HTTP information ko inspect karke routing decisions le sakta hai.

Simple:

HTTP Request
      ↓
ALB
      ↓
Inspect request
      ↓
Choose target

18. Ratio-Based Routing with ALB

Transcript me HTTP requests ke count ke basis par routing ka example diya gaya hai.

Example:

10 requests → Server A
10 requests → Server B

Ye advanced routing/load-balancing capabilities ka part hai.

19. SSL Offloading ⭐⭐⭐⭐

Transcript me ALB ka SSL offloading concept bhi explain hua hai.

Idea:

Client
  ↓
HTTP
  ↓
ALB
  ↓
Secure connection
  ↓
Backend Server

ALB secure connection establish karne ka kaam handle kar sakta hai, reducing that responsibility from backend servers.

Interview:

SSL offloading means handling SSL/TLS processing at the Load Balancer instead of making backend servers handle all of it.

20. ALB — Cost & Latency

Transcript ke according ALB:

More advanced capabilities provide karta hai
HTTP requests inspect karta hai
Advanced routing provide karta hai
Isliye comparatively costly hai
Layer 7 processing ki wajah se additional latency ho sakti hai

Easy:

ALB = More features → More processing

21. Network Load Balancer — NLB ⭐⭐⭐⭐⭐

NLB = Network Load Balancer

NLB operates at:

Layer 4 — Transport Layer

It works with network traffic such as:

TCP
UDP

22. Why NLB?

NLB is useful when:

Very low latency required
High-speed transmission required
Layer 4 routing required
HTTP-level inspection/routing nahi chahiye

23. NLB Routing

NLB request ko mainly:

IP
Port

ke basis par route kar sakta hai.

Example:

User
 ↓
NLB
 ↓
IP + Port
 ↓
Server

Transcript me NLB ko IP/port-based routing ke context me explain kiya gaya hai.

24. NLB — Low Latency ⭐⭐⭐⭐⭐

NLB Layer 4 par work karta hai aur HTTP request ko Layer 7 par deeply inspect nahi karta.

Isliye transcript ke according:

NLB is faster and has lower latency compared with ALB for appropriate Layer 4 workloads.

25. NLB Use Cases ⭐⭐⭐⭐⭐

Transcript ke important examples:

Game Servers
Users
 ↓
NLB
 ↓
Game Servers
Video Streaming
Users
 ↓
NLB
 ↓
Streaming Servers

NLB low latency/high transmission requirements ke liye useful hai.

26. Sticky Sessions ⭐⭐⭐⭐⭐

NLB ka transcript me important advantage:

Sticky Sessions

Meaning:

Agar first request User A ki Server 1 ko gayi:

User A
 ↓
NLB
 ↓
Server 1

Then subsequent requests bhi Server 1 ko ja sakti hain:

User A
 ↓
NLB
 ↓
Server 1

Isse same user's traffic same server ke saath maintain kiya ja sakta hai.

Transcript ne long video streaming ko example banaya hai.

27. Gateway Load Balancer — GWLB ⭐⭐⭐⭐⭐

GWLB = Gateway Load Balancer

Ye normal application traffic load balancing ke liye nahi hai.

Ye primarily virtual network/security appliances ke liye use hota hai.

Examples:

Firewall
VPN
Other virtual appliances

28. Why GWLB?

Firewall/VPN applications ko specific type ka traffic handle karna hota hai.

Transcript ke according ALB/NLB is type ke virtual-appliance traffic ke liye suitable nahi hote in the discussed scenario.

GWLB specialized traffic ko virtual appliances tak securely pass karne ke liye designed hai.

29. GWLB and Security

Transcript ke according virtual appliances jaise:

Firewall
VPN

ke case me traffic ko highly secure hona chahiye.

GWLB encrypted packets ko virtual appliances tak forward karne ke context me explain kiya gaya hai.

30. ALB vs NLB vs GWLB ⭐⭐⭐⭐⭐
Feature	ALB	NLB	GWLB
Layer	L7	L4	Specialized gateway
Main traffic	HTTP/HTTPS	TCP/UDP	Virtual appliance traffic
Routing	Advanced/application-level	IP/Port based	Appliance traffic
Latency	More processing	Very low	Security/appliance focused
Use case	Web applications	Gaming/streaming	Firewall/VPN
Special feature	Path/Host routing	Sticky sessions	Virtual appliances

Transcript ka core selection rule:

Layer 7 + Advanced Routing → ALB

Layer 4 + Low Latency → NLB

Firewall/VPN/Virtual Appliance → GWLB

31. Production Architecture in Transcript ⭐⭐⭐⭐⭐

Transcript ke practical project me architecture tha:

                    INTERNET
                        ↓
                Internet Gateway
                        ↓
              ┌─────────────────┐
              │ Load Balancer   │
              │      ALB        │
              └────────┬────────┘
                       ↓
                Target Group
                  /       \
                 ↓         ↓
              EC2-1      EC2-2
              Private    Private
              Subnet     Subnet

Load Balancer public subnet me tha, while application instances private subnets me deployed the.

32. Why Two Availability Zones? ⭐⭐⭐⭐⭐

Production architecture me two AZs use kiye gaye.

Example:

AZ-A
 ├── Public Subnet
 └── Private Subnet

AZ-B
 ├── Public Subnet
 └── Private Subnet

Reason:

Agar ek Availability Zone fail ho jaye, dusri AZ application ko serve kar sakti hai.

Two AZs → Better availability / fault tolerance

33. Public Subnet

Public subnet me transcript ke architecture me:

Load Balancer
NAT Gateway

rakhe gaye.

Public subnet ka route table Internet Gateway ki taraf route provide karta hai.

34. Private Subnet

Private subnet me:

Application EC2 instances

deploy kiye gaye.

Ye instances directly internet se accessible nahi the aur public IP nahi diya gaya.

35. NAT Gateway

Private subnet ke applications ko internet se outbound access chahiye ho sakta hai.

Example:

Private EC2
   ↓
NAT Gateway
   ↓
Internet

NAT Gateway private instance ke private IP ko apne public IP ke through internet par access karne me help karta hai.

Important:

NAT Gateway allows private resources to access the internet without making them directly internet-accessible.

36. Elastic IP

Transcript me NAT Gateway ke saath Elastic IP use hua.

Elastic IP ek persistent/static public IPv4 address hai jo resource ke saath associated reh sakta hai.

Transcript me ise simple language me static IP explain kiya gaya hai.

37. Auto Scaling Group — ASG ⭐⭐⭐⭐

Production project me EC2 instances manually create karne ke bajay Auto Scaling Group use hua.

Basic idea:

Auto Scaling Group
        ↓
EC2-1
EC2-2
EC2-3
...

Agar traffic increase ho:

High Traffic
     ↓
Auto Scaling
     ↓
More EC2 Instances

Transcript me desired, minimum aur maximum capacity ka concept bhi explain hua.

38. Desired / Minimum / Maximum Capacity

Example:

Desired = 2
Minimum = 2
Maximum = 4

Meaning:

Normally 2 instances
Minimum 2
Requirement hone par maximum 4 tak scale kar sakta hai
39. Launch Template

Auto Scaling Group directly instances ke configuration ko define karne ke bajay Launch Template use karta hai.

Launch Template me things like:

AMI
Instance type
Key pair
Security Group
Other instance configuration

define ki ja sakti hain.

Transcript me ASG create karne se pehle Launch Template create kiya gaya tha.

40. Bastion Host ⭐⭐⭐⭐⭐

Private EC2 instances ke paas public IP nahi hota.

Therefore:

Internet
   ↓
Cannot directly SSH
   ↓
Private EC2

Instead:

Your Laptop
     ↓
Bastion Host
     ↓
Private EC2

Bastion Host = Jump Host

It acts as a controlled entry point for accessing private instances.

41. Bastion Host — Why?

Benefits mentioned in transcript:

Private instances ko directly expose nahi karna
SSH access ka controlled point
Logging
Auditing
Access rules configure karna

42. Bastion Host Network

Bastion Host:

Public Subnet

Private application instances:

Private Subnet

Bastion same VPC me hona chahiye taaki private instances tak access kar sake.

43. SCP

Transcript ke practical setup me SCP use hua.

SCP = Secure Copy

It securely copies files from one machine to another.

Example:

Laptop
 ↓
SCP
 ↓
Bastion Host

Then Bastion se private EC2 par SSH kiya gaya.

44. Target Group ⭐⭐⭐⭐⭐

Target Group = Backend targets ka group.

Example:

Target Group
    ↓
EC2-1
EC2-2

Load Balancer Target Group ko use karke backend instances ko traffic forward karta hai.

45. Target Group + Port

Target Group ko batana padta hai ki backend application kis port par listen kar rahi hai.

Transcript me application Port 8000 par run ki gayi thi.

ALB
 ↓
Target Group
 ↓
EC2 : 8000

46. Health Check ⭐⭐⭐⭐⭐

Target Group backend instances ki health check karta hai.

Example:

EC2-1 → Healthy
EC2-2 → Unhealthy

Load Balancer traffic healthy target ko forward karega.

Transcript ke demo me ek EC2 healthy aur doosra unhealthy tha, isliye traffic healthy EC2 ko hi ja raha tha.

47. Listener ⭐⭐⭐⭐⭐

Listener incoming traffic ko specific protocol + port par receive karta hai.

Example:

Internet
   ↓
HTTP : 80
   ↓
ALB Listener
   ↓
Target Group
   ↓
EC2 : 8000

Transcript me ALB ke liye HTTP listener on port 80 configure kiya gaya.

48. Listener Port vs Target Port

Ye important distinction hai.

Example:

Client
 ↓
ALB : 80
 ↓
Target Group
 ↓
EC2 : 8000

Yahan:

80 = Load Balancer listener port

8000 = Backend application port

Transcript ke practical configuration me exactly ye concept use hua.

49. Security Group and Load Balancer ⭐⭐⭐⭐⭐

Agar ALB HTTP port 80 par listen kar raha hai, Security Group ko port 80 ka inbound traffic allow karna hoga.

Otherwise:

Internet
 ↓
ALB : 80
 ✕
Security Group blocks

Transcript me practical error exactly isi wajah se aaya tha aur inbound HTTP/port 80 allow karke solve kiya gaya.

50. Complete Request Flow ⭐⭐⭐⭐⭐

Production project ka complete flow:

User
 ↓
Internet
 ↓
Internet Gateway
 ↓
Public Subnet
 ↓
Application Load Balancer
 ↓
Listener
 ↓
Target Group
 ↓
Healthy EC2
 ↓
Application

Backend:

EC2
 ↓
Private Subnet

Aur private EC2 ko outbound internet chahiye:

Private EC2
 ↓
NAT Gateway
 ↓
Internet

51. Complete Architecture to Remember ⭐⭐⭐⭐⭐
                         INTERNET
                             │
                             ↓
                    ┌─────────────────┐
                    │ Internet Gateway│
                    └────────┬────────┘
                             │
                     PUBLIC SUBNET
                             │
                    ┌─────────────────┐
                    │      ALB        │
                    │  Load Balancer  │
                    └────────┬────────┘
                             │
                         Listener
                             │
                       Target Group
                         /       \
                        /         \
                       ↓           ↓
                PRIVATE SUBNET  PRIVATE SUBNET
                    EC2-1           EC2-2
                     │                │
                     └──────┬─────────┘
                            │
                      Application

For outbound internet:

Private EC2
    ↓
NAT Gateway
    ↓
Internet Gateway
    ↓
Internet

For SSH administration:

Your Laptop
    ↓
Bastion Host
    ↓
Private EC2
🔥 FINAL INTERVIEW REVISION
Load Balancer

Distributes incoming traffic across multiple backend servers.

Round Robin

Requests are distributed sequentially among available servers.

ALB

Layer 7 load balancer for HTTP/HTTPS with advanced application-level routing.

NLB

Layer 4 load balancer for TCP/UDP, suitable for high-performance and low-latency workloads.

GWLB

Used for virtual network appliances such as firewalls and VPNs.

Target Group

Group of backend targets to which the Load Balancer forwards traffic.

Health Check

Checks whether backend targets are healthy and eligible to receive traffic.

Listener

Receives incoming connections on a configured protocol and port.

Bastion Host

Jump host used to securely access private EC2 instances.

NAT Gateway

Allows private resources to make outbound internet connections without exposing them directly to the internet.

Auto Scaling Group

Automatically manages the number of EC2 instances according to configured capacity/scaling requirements.

🧠 ALB vs NLB vs GWLB — ONE LINE
ALB  → Layer 7 → HTTP/HTTPS → Advanced Routing

NLB  → Layer 4 → TCP/UDP → High Performance / Low Latency

GWLB → Virtual Appliances → Firewall / VPN