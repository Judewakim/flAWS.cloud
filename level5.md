## Level 5

This EC2 has a simple HTTP only proxy on it. See if you can use this proxy to figure out how to list the contents of the level6 bucket at level6-cc4c404a8a8b876167f5e70a7d8c9880.flaws.cloud that has a hidden directory in it.

In this level you are told the EC2 only uses HTTP proxy and are given an example link. Your task is to find the contents of the level 6 S3 bucket using the HTTP proxy on the EC2 instance.

## Detailed Walkthrough

-In level 5, we are told this EC2 instance is using simple HTTP only proxy and we are given links as examples, like: http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/flaws.cloud/ and http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/summitroute.com/blog/feed.xml and http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/neverssl.com/.

-I noticed they all have /proxy/something after the website.

-We are told to leverage this proxy to find a way to list the contents of the level 6 bucket.

-Hint one tells you that the IP 169.254.169.254 is metadata service.

-Tried http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254 and got a list of metadata category version release dates.

-Kept appending to the url, going through directories, looking for something juicy and eventually got to this address: http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data/iam/security-credentials/flaws/ but kept looking and also got to this address: http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance. Both of these addresses had access keys.

-Seeing the access keys with an access token told me these are IAM roles or users. So I decided to continue on with the IAM address (the first one).

-Tried using aws configure command to make IAM users but it did not ask for tokens and I received an authentication error when trying to use aws s3 commands on the level 6 bucket. That's when I Googled and learned that IAM roles need tokens and IAM users do not.

-Hint three told me I needed to make an IAM role in the aws credentials file, which is found at: <code>~/.aws/credentials</code> which is a file of all credentials.

-Now that I had a valid IAM user and group for the level 6 bucket I was able to use the profile to list buckets at the level 6 address using this command: <code>aws --profile [MY_LEVEL5_PROFILE] s3 ls level6-cc4c404a8a8b876167f5e70a7d8c9880.flaws.cloud</code>.

-That returned me one bucket, which I appended to the address, giving me the level 6 page, completing level 5. 

