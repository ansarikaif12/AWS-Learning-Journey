# Day 02 - IAM 🔐

## What is IAM?

IAM stands for Identity and Access Management.

It is an AWS service used to control who can access AWS resources
and what actions they are allowed to perform.

---

## IAM Components

### 1. IAM User

An IAM User represents an individual person or application
that needs access to AWS resources.

Example:

- Developer
- Administrator
- Application

---

### 2. IAM Group

An IAM Group is a collection of IAM users.

Instead of assigning permissions to every user individually,
permissions can be assigned to a group.

Example:

Developers Group

- Kaif
- Aman
- Rahul

If the Developers group has EC2 permissions,
its users can receive those permissions.

---

### 3. IAM Policy

An IAM Policy is a JSON document that defines permissions.

A policy specifies:

- Effect → Allow or Deny
- Action → What action is allowed
- Resource → Which AWS resource is affected

Example:

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "*"
}


---

### 4. IAM Role

An IAM Role is an identity that provides permissions
to users, applications or AWS services.

A common example is:

EC2 → IAM Role → S3

Instead of storing AWS access keys inside an EC2 instance,
we attach an IAM Role to the EC2 instance.

The role provides temporary credentials.

---

## User vs Role

| User | Role |
|---|---|
| Represents a person/application | Provides temporary permissions |
| Long-term identity | Usually temporary credentials |
| Commonly used by humans | Commonly used by AWS services/applications |

---

## Authentication vs Authorization

### Authentication

Answers:

**"Who are you?"**

Example:

Username and password.

### Authorization

Answers:

**"What are you allowed to do?"**

Example:

Can the user read an S3 bucket?

Can the user create an EC2 instance?

---

## Least Privilege Principle

Give a user or application only the permissions
that are actually required.

Example:

If a developer only needs to read files from S3,
give S3 read permission instead of full Administrator access.

This improves security.

---

## Root User

The AWS Root User has complete access to the AWS account.

The root user should not be used for normal daily activities.

Important:

- Enable MFA for the root account.
- Avoid using the root account for regular tasks.

---

## Important Architecture

User
  ↓
Group / Role
  ↓
Policy
  ↓
AWS Resource

Example:

EC2
 ↓
IAM Role
 ↓
IAM Policy
 ↓
S3

---

## Important Interview Questions

### Q1. What is IAM?

IAM stands for Identity and Access Management.
It controls who can access AWS resources and what actions
they can perform.

### Q2. What is an IAM User?

An identity representing an individual person or application.

### Q3. What is an IAM Group?

A collection of IAM users used to manage permissions collectively.

### Q4. What is an IAM Policy?

A JSON document that defines permissions for AWS resources.

### Q5. What is an IAM Role?

An identity that provides permissions to users, applications,
or AWS services.

### Q6. Why use IAM Roles with EC2?

IAM Roles provide temporary credentials and avoid storing
long-term AWS access keys inside the EC2 instance.

### Q7. What is Least Privilege?

Giving only the minimum permissions required to perform a task.

### Q8. Authentication vs Authorization?

Authentication verifies who you are.

Authorization determines what you are allowed to do.

---

## Key Takeaways

- IAM = Identity and Access Management
- User = Individual identity
- Group = Collection of users
- Policy = Permission rules
- Role = Temporary permissions
- Authentication = Who are you?
- Authorization = What can you do?
- Least Privilege = Minimum required permissions
- Root User = Full account access

## Status

Completed ✅



==========================================================================================================================================



Day 2 — IAM: Placement ke liye kya yaad rakhna hai
1. IAM kya hai?

IAM = Identity and Access Management

AWS mein authentication + authorization handle karta hai.

Simple:

Authentication = Tum kaun ho?
Authorization = Tum kya kar sakte ho?

Example:

Login karna → Authentication
S3 bucket delete karne ki permission → Authorization
2. IAM User 👤

IAM User ek individual person/application ke liye identity hai.

Example:

Developer → IAM User
QA → IAM User
Admin → IAM User

User se mainly authentication hoti hai.

3. IAM Policy 📜

Policy batati hai:

User ko kya karne ki permission hai?

Example:

User → Kaif
Policy → S3 Read Only

Kaif S3 ko read kar sakta hai, lekin delete nahi.

Policy basically permissions ka JSON document hota hai.

Important parts:

Effect    → Allow / Deny
Action    → Kya action?
Resource  → Kis resource par?

Example:

{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "*"
}
4. IAM Group 👥

Group = multiple users ka collection.

Example:

Development Group
       ↓
 ┌─────┼─────┐
Kaif  Aman  Rahul

Agar group ko:

S3 Full Access

policy mili hai, to group ke users ko bhi woh permission mil jayegi.

Why groups?

Suppose 100 developers hain.

Without group:

User 1 → Policy
User 2 → Policy
User 3 → Policy
...
User 100 → Policy

😵‍💫

With group:

Development Group
       ↓
S3 Policy
       ↓
100 Developers

Much easier.

5. IAM Role 🔑

Ye lecture ka sabse important concept hai jo abhi detail mein nahi padhna hai.

Role generally AWS resources/services ko permissions dene ke liye use hota hai.

Example:

EC2
 ↓
IAM Role
 ↓
S3 Access

Matlab EC2 instance ko S3 access chahiye → EC2 ko role diya ja sakta hai.

Abhi bas itna yaad rakho:

User → Person

Role → AWS service/application

Role ko hum later properly karenge, exactly jaise Abhishek ne lecture mein bola.

🔥 Ekdum short revision

Isko yaad kar lo:

IAM
│
├── User     → Who are you?
│
├── Policy   → What can you do?
│
├── Group    → Collection of users
│
└── Role     → Permissions for services/applications

And:

Authentication → User
Authorization  → Policy
Organization   → Group
Service Access → Role
Placement question

Q. IAM User aur IAM Role mein difference?

Answer:

IAM User is generally created for an individual person who needs AWS access, whereas an IAM Role is assumed by AWS services, applications, or other trusted entities to obtain temporary permissions.




Q. IAM User aur IAM Role mein difference?

IAM User:
IAM User generally kisi person ko AWS access dene ke liye banaya jata hai.

Example:

Kaif ek developer hai → Kaif ke liye IAM User banega.

IAM Role:
IAM Role generally AWS service, application ya kisi trusted entity ko temporary permissions dene ke liye use hota hai.

Example:

EC2 ko S3 access chahiye → EC2 ko IAM Role de sakte hain.

Ek line mein yaad rakho 🔥

User = Person ke liye 👤
Role = Service/Application ke liye ⚙️

Aur Role ki permissions generally temporary credentials ke through milti hain, isliye password permanently store karne ki zarurat nahi hoti.


🧠 Day 2 ka final revision
Concept	Simple meaning
IAM User 👤	Person ko AWS access
IAM Policy 📜	Kya permission hai
IAM Group 👥	Multiple users ko organize karna
IAM Role 🔑	AWS service/application ko permission
Ek real example
Kaif
 ↓
IAM User
 ↓
AWS-Developers Group
 ↓
S3 ReadOnly Policy
 ↓
S3 access
🔥 Placement mein ye 3 lines yaad rakhna

Authentication: User kaun hai?
Authorization: User kya kar sakta hai?
Group: Multiple users ki permissions manage karne ka easy way.

Role: Service/application ko permissions dene ke liye.

⚠️ Ab ek cleanup

Humne practical ke liye aws-test-user banaya hai.

Abhi delete mat karo. Isko future IAM practice mein reuse kar sakte hain.

Lekin jo S3 bucket humne practice mein banayi ho, agar koi bucket/object tumne create kiya hai aur future mein use nahi karna, use delete kar dena.