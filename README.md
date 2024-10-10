# flAWS.cloud
This repository is to document flAWS.cloud fun!
<br>
http://flaws.cloud/
<br>
<br>
Flaws.cloud is an intentionally vulnerable AWS S3 bucket security challenge created by Scott Piper, a cloud security expert. It is designed to teach security professionals, developers, and AWS users about the common misconfigurations in Amazon S3 that can lead to security vulnerabilities. The challenge highlights various ways an attacker could exploit poorly configured AWS S3 buckets. 
<br>
<br>
flAWS.cloud demonstrates:
<br>
<br>
-**Public Access Issues**: many companies mistakenly expose sensitive data by making their S3 buckets public. The challenge shows how attackers can easily access data that should have been kept private. 
<br>
-**Misconfigured Permissions**: flaws.cloud helps users learn about incorrect S3 permissions (both bucket and object-level permissions). An attacker could take advantage of excessive permissions granted to everyone or certain users, enabling unauthorized access.
<br>
-**Access Control Policies**: Users are guided through understanding how S3 bucket policies, IAM policies, and access control lists (ACLs) work. Misconfigurations in these policies can lead to unintended exposure of sensitive information.
<br>
-**Logging and Monitoring**: It demonstrates the importance of logging (e.g., AWS CloudTrail and S3 Access Logs) and monitoring AWS accounts for unauthorized access attempts and potential breaches.
<br>
-**Privilege Escalation**: The challenge shows how attackers might exploit improper IAM role or policy setups to escalate privileges within the AWS account.
<br>


## [Level 4](https://github.com/Judewakim/flAWS.cloud/blob/main/level4.md)

For the next level, you need to get access to the web page running on an EC2 at 4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud
<br>
It'll be useful to know that a snapshot was made of that EC2 shortly after nginx was setup on it.
<br>
<br>
In this level you need to get access to the webpage but it asks for credentials you do not have but you know there is a snapshot you can look for. You get the snapshot ID and create an EC2 volume with that snapshot and mount it to an EC2 that you create in your AWS account. Then you ssh into the instance and look for the credentials for the webpage.
<br>

## [Level 5](https://github.com/Judewakim/flAWS.cloud/blob/main/level5.md)

This EC2 has a simple HTTP only proxy on it. See if you can use this proxy to figure out how to list the contents of the level6 bucket at level6-cc4c404a8a8b876167f5e70a7d8c9880.flaws.cloud that has a hidden directory in it.
<br>
<br>
In this level you are told the EC2 only uses HTTP proxy and are given an example link. Your task is to find the contents of the level 6 S3 bucket using the HTTP proxy on the EC2 instance.
<br>

## [Level 6](https://github.com/Judewakim/flAWS.cloud/blob/main/level6.md)

For this final challenge, you're getting a user access key that has the SecurityAudit policy attached to it. See what else it can do and what else you might find in this AWS account.
<br>
<br>
In this level you are given access keys and are told it says the SecurityAudit policy permissions. Using that information, you have to find what else you can do with this account.
<br>
