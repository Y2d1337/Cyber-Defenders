# Bucket Challenge
![This is an image](/Bucket/Images/buckethead.png)

## Challenge Link
https://cyberdefenders.org/labs/84

## Challenge Information
- Use the provided credentials to access AWS cloud trail logs and answer the questions.
- Tags: 
    - AWS 
    - Cloud 
    - Log analysis 
    - IR 

## Credentials

    Your IAM credentials for the Security account:
    Login: https://flaws2-security.signin.aws.amazon.com/console
    Account ID: 322079859186
    Username: security
    Password: password
    Access Key: AKIAIUFNQ2WCOPTEITJQ
    Secret Key: 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


## Scenario
> Welcome, Defender! As an incident responder, we're granting you access to the AWS account called "Security" as an IAM user. This account contains a copy of the logs during the time period of the incident and has the ability to assume the "Security" role in the target account so you can look around to spot the misconfigurations that allowed for this attack to happen.

## Environment

The credentials above give you access to the Security account, which can assume the role of "security" in the Target account. You also have access to an S3 bucket, named flaws2_logs, in the Security account, that contains the CloudTrail logs recorded during a successful compromise

![This is an image](/Bucket/Images/defender_account.png)

## Tools Used
    • AWS-CLI
        ◦ The AWS Command Line Interface (CLI) is a unified tool to manage your AWS services
    • Notepad++
        ◦ Notepad++ is a free (as in “free speech” and also as in “free beer”) source code editor and Notepad replacement that supports several languages
              

 ## Questions:    
 ### 1.What is the full AWS CLI command used to configure credentials? 
 I used AWS CLI for windows to get the logs from the s3 bucket.
 
 #### Explanation:
 > The AWS Command Line Interface (CLI) is a unified tool to manage your AWS services
 
 First we need to configure the credentials in AWS CLI
 
 From [Amazon docss](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)
 
 Enter `aws configure` in AWS CLI
 
 And then enter the data 
 `Access Key`
 `Secret Key`
 `Default region name`
 `Default output format`
 
![q1](/Bucket/Images/awscliconf.png)
 
Now we can use the AWS CLI to execute command on our s3 bucket 

> **Flag: aws configure**

 ### 2.What is the 'creation' date of the bucket 'flaws2-logs'?
 
 
 ### 3.What is the name of the first generated event -according to time?
 ### 4.What source IP address generated the event dated 2018-11-28 at 23:03:20 UTC?
 ### 5.Which IP address does not belong to Amazon AWS infrastructure?
 ### 6.Which user issued the 'ListBuckets' request?
 ### 7.What was the first request issued by the user 'level1'?
