📘 AWS Day 6 – Route 53 Notes
1. What is Route 53?

Amazon Route 53 is an AWS service that provides DNS (Domain Name System) functionality.

👉 Simple definition:

Route 53 = DNS service provided by AWS

Jaise:

EC2 → Compute as a Service
EKS → Kubernetes as a Service
Route 53 → DNS as a Service
2. What is DNS?

DNS = Domain Name System

DNS ka main kaam:

Domain Name ko IP Address ke saath map/resolve karna.

Example:

Hum browser mein likhte hain:

amazon.com

Lekin server ko ultimately IP address chahiye.

DNS:

amazon.com
      ↓
IP Address

So DNS domain name ko IP address mein resolve karta hai.

3. Why do we need DNS?

Suppose tumhari application ka IP hai:

3.6.10.171

User ko ye IP yaad rakhna difficult hai.

Instead hum domain name use karte hain:

myapp.com
Main reasons:

1. Easy to remember

myapp.com

is easier than:

3.6.10.171

2. IP address can change

Agar server/load balancer ka IP change ho jaye, user ko new IP remember nahi karna padta.

User simply:

myapp.com

use karta hai.

DNS updated IP par request resolve kar dega.

4. DNS ka basic working

Suppose user access karta hai:

amazon.com

Flow:

User
 ↓
DNS / Route 53
 ↓
Find DNS Record
 ↓
Load Balancer IP / Target
 ↓
Application
Simple example:

DNS record:

amazon.com → 3.6.10.171

User:

amazon.com

DNS:

amazon.com = 3.6.10.171

Then request appropriate destination par jaati hai.

5. Route 53 with AWS Architecture

Typical AWS architecture:

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
              Application

Route 53 domain resolution provide karta hai:

User
 ↓
Route 53
 ↓
Load Balancer
 ↓
Application
6. Route 53 ke Important Components

Day 6 mein mainly 3 important things discuss hui:

1. Domain Registration
2. Hosted Zones
3. Health Checks
7. Domain Registration

Domain registration ka matlab:

Domain name purchase/register karna.

Example:

mywebsite.com

Route 53 ke through tum domain register kar sakte ho.

Ya tum domain kisi external provider se bhi purchase kar sakte ho, jaise:

GoDaddy

and then us domain ko Route 53 ke saath use/integrate kar sakte ho.

Example:
Domain purchased from GoDaddy
              ↓
       Configure Route 53
              ↓
        Hosted Zone
              ↓
        DNS Records
8. Hosted Zone

Hosted Zone is where DNS records for a domain are managed.

Simple language:

Hosted Zone = DNS records ko manage/store karne ki place.

Example:

Domain: myapp.com

Hosted Zone mein records ho sakte hain:

myapp.com → Load Balancer
www.myapp.com → Load Balancer
api.myapp.com → API Server
9. Public vs Private Hosted Zone

Route 53 mein hosted zones mainly do types ki hoti hain:

Public Hosted Zone

Internet se accessible domain ke DNS records ke liye.

Example:

myapp.com

Public users access kar sakte hain.

Private Hosted Zone

Private/internal resources ke DNS resolution ke liye.

Example:

database.internal

Ye generally VPC ke andar use hota hai.

10. DNS Records

DNS records basically mapping information provide karte hain.

Simple example:

Domain Name              Destination

example.com       →      Load Balancer
api.example.com   →      Server

So Route 53 DNS records ko use karke domain ko correct destination tak resolve karta hai.

11. Health Checks

Route 53 ka ek important feature:

Health Checks

Suppose tumhari application multiple servers par running hai:

Server 1
Server 2

Route 53 health check kar sakta hai ki server/application healthy hai ya nahi.

Route 53
   ↓
Health Check
   ↓
Server 1 → Healthy ✅
Server 2 → Unhealthy ❌

Agar koi endpoint unhealthy ho, Route 53 routing decisions mein us health information ka use kar sakta hai.

12. Complete Flow

Interview mein simple flow aise explain kar sakte ho:

User
  ↓
Domain Name
  ↓
Route 53
  ↓
DNS Record
  ↓
Load Balancer
  ↓
Application

Example:

User enters:

myapp.com

      ↓

Route 53

      ↓

DNS record

myapp.com → Load Balancer

      ↓

Load Balancer

      ↓

Application
⭐ Interview Important Questions
Q1. What is Route 53?

Answer:

Amazon Route 53 is an AWS managed DNS service used for domain registration, DNS management, routing, and health checks.

Q2. What does DNS stand for?

Answer:

Domain Name System.

Q3. What is the main purpose of DNS?

Answer:

DNS resolves domain names into IP addresses or appropriate destinations.

Example:

google.com → IP address
Q4. Why do we use domain names instead of IP addresses?

Answer:

Because domain names are:

Easier to remember
More user-friendly
IP addresses can change
Q5. What is a Hosted Zone?

Answer:

A Hosted Zone is a container in Route 53 where DNS records for a domain are managed.

Q6. What are the types of Hosted Zones?

Answer:

1. Public Hosted Zone
2. Private Hosted Zone
Q7. Can I buy a domain using Route 53?

Answer:

Yes.

You can register a domain through Route 53, or use a domain registered with an external registrar.

Q8. What are Route 53 Health Checks?

Answer:

Health Checks monitor the health/availability of endpoints and can be used with routing decisions.

🧠 One-Page Revision
                 ROUTE 53
                    │
          ┌─────────┼─────────┐
          ↓         ↓         ↓
       Domain    Hosted     Health
     Registration  Zones     Checks
                    │
              ┌─────┴─────┐
              ↓           ↓
           Public       Private
           Hosted       Hosted
            Zone          Zone
Remember this line:

Route 53 = AWS DNS Service

And:

DNS = Domain Name → Destination/IP

And:

Hosted Zone = DNS Records manage karne ki place

And:

Health Check = Endpoint healthy hai ya nahi check karna


Route 53: Must Remember
Route 53 = DNS service of AWS
DNS = Domain Name System
DNS ka main kaam:
Domain Name → IP Address / AWS resource
Example:
amazon.com → Load Balancer
Hosted Zone → DNS records store/manage karta hai.
Hosted Zones ke 2 types:
Public Hosted Zone
Private Hosted Zone
Domain Registration → domain purchase/register karna.
Route 53 se domain register kar sakte ho, ya externally purchased domain ko Route 53 ke saath use kar sakte ho.
Health Checks → check karte hain ki application/server healthy hai ya nahi.
Route 53 ke important concepts:
Domain Registration + Hosted Zones + DNS Records + Health Checks
Real-world architecture:
User → Route 53 → Load Balancer → Application
Domain name IP address se better hai because:
Remember karna easy hai
IP change hone par user ko new IP yaad nahi karna padta.
⭐ Interview Question

Q. Route 53 kya hai?

Amazon Route 53 is a highly available and scalable AWS DNS service used for domain registration, DNS routing, hosted zones, and health checks.


Route 53 MCQ Notes
Q1. Route 53 AWS mein kya provide karta hai?

A. Compute as a Service
B. DNS as a Service
C. Database as a Service
D. Storage as a Service

✅ Correct Answer: B. DNS as a Service

Reason: Route 53 AWS ki managed DNS service hai. DNS domain name ko IP address/destination se resolve karta hai.

Q2. DNS ka full form kya hai?

A. Domain Network System
B. Domain Name Service
C. Domain Name System
D. Data Name System

✅ Correct Answer: C. Domain Name System

Reason: DNS ka kaam human-readable domain name ko corresponding destination/IP se resolve karna hai.

Q3. amazon.com aur flipkart.com kis type ke examples hain?

A. IP Addresses
B. Domain Names
C. Subnets
D. Security Groups

✅ Correct Answer: B. Domain Names

Reason: Ye human-readable names hain jo users applications/websites access karne ke liye use karte hain.

Q4. DNS ka primary kaam kya hai?

A. EC2 server create karna
B. Domain name ko IP address/destination se resolve karna
C. Security Group create karna
D. VPC create karna

✅ Correct Answer: B. Domain name ko IP address/destination se resolve karna

Reason: Example:

amazon.com → destination/IP

DNS is mapping ko handle karta hai.

Q5. Users generally IP address ki jagah domain name kyun use karte hain?

A. IP addresses hamesha free hote hain
B. Domain names yaad rakhna easier hota hai
C. Domain names internet ke bina work karte hain
D. IP addresses websites ko block karte hain

✅ Correct Answer: B. Domain names yaad rakhna easier hota hai

Reason: 3.6.10.171 yaad rakhne ke comparison mein amazon.com yaad rakhna much easier hai.

Q6. IP address ke change hone ki possibility DNS/domain name use karne ka ek reason kyun hai?

A. Domain name automatically server ko create karta hai
B. IP address dynamic/change ho sakta hai
C. IP address ka koi use nahi hai
D. DNS IP address ko delete karta hai

✅ Correct Answer: B. IP address dynamic/change ho sakta hai

Reason: Infrastructure change hone par IP address change ho sakta hai. Domain name ko use karke users ko underlying IP ya infrastructure change ke baare mein concern nahi hota.

Q7. Route 53 mein DNS records kahan maintain kiye jaate hain?

A. Security Group
B. EC2
C. Hosted Zone
D. IAM

✅ Correct Answer: C. Hosted Zone

Reason: Hosted Zone mein domain ke DNS records maintain/configure kiye jaate hain.

Q8. Route 53 mein Hosted Zone ke main types kaun se hain?

A. Public aur Private
B. Internal aur External
C. Local aur Global
D. Static aur Dynamic

✅ Correct Answer: A. Public aur Private

Reason: Route 53 mein Public Hosted Zone internet-facing DNS resolution ke liye aur Private Hosted Zone VPC ke andar private DNS ke liye use hoti hai.

Q9. Agar domain GoDaddy se purchase kiya hai, kya use Route 53 ke saath use kar sakte hain?

A. No
B. Sirf EC2 ke saath
C. Yes
D. Sirf CloudFront ke saath

✅ Correct Answer: C. Yes

Reason: Domain kisi external registrar se purchase kiya ho, tab bhi DNS management ke liye Route 53 use kiya ja sakta hai.

Q10. Route 53 mein domain registration ka kya matlab hai?

A. EC2 instance register karna
B. Domain name purchase/register karna
C. Security Group register karna
D. VPC register karna

✅ Correct Answer: B. Domain name purchase/register karna

Reason: Route 53 ke through domain name register/purchase kiya ja sakta hai.

Q11. Route 53 ka kaunsa feature web server/application ki availability check kar sakta hai?

A. Hosted Zone
B. Domain Registration
C. Health Checks
D. Security Groups

✅ Correct Answer: C. Health Checks

Reason: Route 53 Health Checks ke through endpoints/web servers ki health monitor kar sakta hai.

Q12. Agar do servers hain aur ek unhealthy ho jata hai, Route 53 Health Checks ka use kis purpose mein helpful ho sakta hai?

A. Unhealthy endpoint ko traffic se avoid karna
B. EC2 ko automatically delete karna
C. VPC delete karna
D. Security Group remove karna

✅ Correct Answer: A. Unhealthy endpoint ko traffic se avoid karna

Reason: Route 53 health information ka use karke healthy endpoint ko prefer/route kiya ja sakta hai, depending on the routing configuration.

Q13. Route 53 mein DNS record ka basic example kya ho sakta hai?

A. Domain → Destination/IP
B. Password → Username
C. VPC → Region
D. EC2 → Security Group

✅ Correct Answer: A. Domain → Destination/IP

Reason: DNS record basically domain ko kisi destination/resource se map karta hai.

Example:

myapp.com → Load Balancer

Q14. Real-world AWS architecture mein Route 53 generally kis component ke domain name resolution mein help kar sakta hai?

A. Load Balancer
B. Keyboard
C. RAM
D. Laptop Battery

✅ Correct Answer: A. Load Balancer

Reason: Common architecture:

User → Route 53 → Load Balancer → Application

Route 53 domain ko appropriate AWS resource/destination par resolve karta hai.

Q15. Route 53 ke important features mein se kaun se hain?

A. Domain Registration
B. Hosted Zones / DNS Records
C. Health Checks
D. All of the above

✅ Correct Answer: D. All of the above

Reason: Route 53 ke important concepts/features:

Domain Registration
Hosted Zones
DNS Records
Health Checks
Routing Policies
🧠 Day 6 Quick Revision
Route 53
   ↓
DNS as a Service
   ↓
Domain Name → Destination
   ↓
Hosted Zone
   ↓
DNS Records
   ↓
Route 53 resolves request
   ↓
Load Balancer / AWS Resource
   ↓
Application
⭐ Interview ke liye yaad rakho:

Route 53 = AWS Managed DNS Service

DNS = Domain Name System

Hosted Zone = DNS records maintain karne ki jagah

Public Hosted Zone = Internet-facing DNS

Private Hosted Zone = VPC/private DNS

Health Check = Endpoint health monitor karna

Common flow:

User → Route 53 → Load Balancer → Application