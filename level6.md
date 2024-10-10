## Level 6

For this final challenge, you're getting a user access key that has the SecurityAudit policy attached to it. See what else it can do and what else you might find in this AWS account.
<br>
<br>
In this level you are given access keys and are told it says the SecurityAudit policy permissions. Using that information, you have to find what else you can do with this account.
<br>

## Detailed Walkthrough

-For this final challenge, you're getting a user access key that has the SecurityAudit policy attached to it. See what else it can do and what else you might find in this AWS account.

-I created a new IAM user using the access key and secret access key provided.

-Hint one tells you the SecurityAudit group can get a high level overview of the resources in an AWS account, but it's also useful for looking at IAM policies.

-This command gets info about the user: <code>aws --profile level6profile iam get-user</code>.

-Now that we have some information about the IAM user like the username, lets see what policies are attached to the user. You can do that using this command: <code>aws --profile level6profile iam list-attached-user-policies --user-name Level6</code>.
   
-There are two policies, one was the one we knew was there (MySecurityAudit) but this other one is not a default policy, its a custom one so lets get more info on it using the the get-policy command. Looks like this: <code> aws --profile level6profile iam get-policy --policy-arn arn:aws:iam::975426262029:policy/list_apigateways</code>.

-So we still don't know what permissions this policy has but we can get that using the get-policy-version command and in order to use that command we need the version id which we just got from the last command, "DefaultVersionId": "v4". Now we can you the get-policy-version command to see the permissions: <code>aws --profile level6profile iam get-policy-version --policy-arn arn:aws:iam::975426262029:policy/list_apigateways --version-id v4</code>.

-We found out that this policy has the permission to GET api gateways. Lambda functions get api gateways so this probably has something to do with a lambda function. So the goal now is to figure out the api gateway invoke url that the lambda function uses. The invoke url format goes: https://[REST_API_ID].execute-api.[REGION].amazonaws.com/[STAGENAME]/[RESOURCENAME].

-Hint two tells us we can use lambda list-functions to see all the functions: <code>aws --region us-west-2 --profile level6profile lambda list-functions</code>.

-So there is one function. Lets get more info on it using the lambda get-policy command: <code>aws --region us-west-2 --profile level6profile lambda get-policy --function-name Level6</code>.

-Now we know the rest-api-id is "s33ppypa75". Now lets find the stage of the api gateway using the command get-stages: <code>aws --region us-west-2 --profile level6profile apigateway get-stages --rest-api-id s33ppypa75</code>.

-Now we have all the info: https://s33ppypa75.execute-api.us-west-2.amazonaws.com/Prod/level6

-I went to that URL which gives you another website to go to, which is the end. 
