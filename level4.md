## Level 4
For the next level, you need to get access to the web page running on an EC2 at 4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud
It'll be useful to know that a snapshot was made of that EC2 shortly after nginx was setup on it.


In this level you need to get access to the webpage but it asks for credentials you do not have but you know there is a snapshot you can look for. You get the snapshot ID and create an EC2 volume with that snapshot and mount it to an EC2 that you create in your AWS account. Then you ssh into the instance and look for the credentials for the webpage.

## Detailed Walkthrough

-So the goal here is to get access to the webpage running on EC2 and you will probably have to get a snapshot of it.<br>

-When you put that link in a web browser you are accessed to sign into an account. Well, we don't have an account with access.<br>

-First thing I did is <code>nslookup 4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud</code> and got its IP and did and "nslookup" on that and confirmed it is an EC2 instance.<br>

-Hint one says "You can snapshot the disk volume of an EC2 as a backup. In this case, the snapshot was made public, but you'll need to find it." which is a common practice in the workplace where you don't have access to the instance itself but you have the snapshot you can mount yourself elsewhere.<br>

-To do find the snapshot of this EC2 instance you need the accountID where the snapshot is and hint one also tells you what command to use to get it: <code>aws --profile [PROFILE_NAME_YOU_CREATED_IN_LVL3] sts get-caller-identity</code>. This "sts get-caller-identity" command is like a "whoami" command. It gives you information about your profile.<br>

-That "sts get-caller-identity" command gave me the UserId, AccountId(OwnerId), and Arn(which tells you have neame of the account which is "backup" in this case).<br>

-"The backups this account makes are snapshots of EC2s."<br>

-Now that you have the OwnerId you can find the snapshot using the command also provided in hint one: <code>aws --profile [PROFILE_NAME_YOU_CREATED_IN_LVL3] ec2 describe-snapshots --owner-id [OWNER_ID]</code> which gives you information about the snapshot.<br>

-Thats all that hint one tells you so when you go to hint two it tells you have that you have the snapshot Id (which is part of the snapshot info you got from the last command) you need to mount it, which means you need to log into an AWS account and create a volume.<br>

-I was lowkey cheating with this level because I was following along with Tyler from Hack Smarter (link below) and so I created an EC2 instance from the console in the us-west-2 region (you are told in the hints to be in us-west-2) before the creating a volume and so when I run this next command I knew which availability zone I needed to be in. Basically all I did was switch the next two parts of this hint.<br>

-Hint two tells you to create a volume using this command <code>aws --profile [YOUR_AWS_USER_ACCOUNT] ec2 create-volume --availability-zone [us-west-2a OR us-west-2b] --region us-west-2 --snapshot-id [SNAPSHOT ID]</code> and and it took a minute to get this to work because I was getting this error "An error occurred (AuthFailure) when calling the CreateVolume operation: AWS was not able to validate the provided access credentials" so I confirms the access keys for my account were stored, I gave my aws user account EC2 access in IAM but that didn't fix it, and when I looked it up it was probably a time sync issue. So I ran this command to see what time Kali thought it was: <code>date</code> and it was actually the wrong time. So now that I knew what the issue is the fix is easy right, just sync the time...of course not. After "ntp" and "chrony" both causing problems I ended up using this command to sync the time: <code>sudo ntpdate pool.ntp.org</code>. Then I was able to create the volume using that "create-volume" command.<br>

-Once you've created the volume now you have to mount it to your instance. If you notice, there is already a volume that is mounted by default (every EC2 instance does) to contain OS and boot process and stuff. So from the console I attached the volume I created to the instance then went back to hint two.<br>

-The next part of hint two tell you that you have ssh into the instance (which now has the snapshot) and mount it from there. You can ssh into the instance using this command: <code>ssh -i [YOUR_KEY_PAIR.pem] ubuntu@[PUBLIC DNS]</code> and if you need help ssh'ing go in the console and hit connect and go to the ssh client tab and it'll tell you how to ssh into the instance.<br>

-Once you're ssh'ed into the instance look at the block devices using <code>lsblk</code> and notice the name of the volume you attached (a volume uses Elastic Block Storage that why you can see it looking at the block devices) and enter this command: <code>sudo file -s /dev/[YOUR VOLUME NAME]</code> which uses the "file" utility to determine the type of data in the specified device. Then mount it using <code>sudo mount /dev/[YOUR_VOLUME_NAME] /mnt</code> then you can dig around the snapshot for the answer to this level.<br>

## Links 

Tyler Ramsbey | Hack Smarter: <br> 
https://youtu.be/PFhfmbo8r-w?si=Ukti-T1sHjgvoWPT 
