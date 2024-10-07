# flAWS.cloud
This repository is to document flAWS.cloud fun!
<br>
http://flaws.cloud/

## [Level 4](https://github.com/Judewakim/flAWS.cloud/blob/main/level4.md)

For the next level, you need to get access to the web page running on an EC2 at 4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud
<br>
It'll be useful to know that a snapshot was made of that EC2 shortly after nginx was setup on it.

<br>

In this level you need to get access to the webpage but it asks for credentials you do not have but you know there is a snapshot you can look for. You get the snapshot ID and create an EC2 volume with that snapshot and mount it to an EC2 that you create in your AWS account. Then you ssh into the instance and look for the credentials for the webpage.
<br>

## Level 5
<br>
