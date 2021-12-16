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
To see the information about the bucket 'flaws2-logs` i ran the command `aws s3 ls` on AWS CLI
 
![q2](/Bucket/Images/q2.png)
 
 
 > **Flag: 2018-11-19 20:54:31 UTC**
 
### 3.What is the name of the first generated event -according to time?
 To see the all logs from the bucket 'flaws2-logs i ran the command
 
`aws s3 ls flaws2-logs/AWSLogs/653711331788/CloudTrail/us-east-1/2018/11/28/`

![q3](/Bucket/Images/awscli-ls.png)
 
The first generated log is `653711331788_CloudTrail_us-east-1_20181128T2235Z_cR9ra7OH1rytWyXY.json`
I search for the event using `notepad++`

![q3a](/Bucket/Images/q3.png)
  
 > **Flag: AssumeRole**
  
### 4.What source IP address generated the event dated 2018-11-28 at 23:03:20 UTC?
 Using `notepad++` advanced search `find in files` to search in all the logs for the IP address

#### Example:
~search menu-->find in files

![q4a](/Bucket/Images/findinfiles.png)

I searched for`sourceIPAddress` and get two results 
 
![q4](/Bucket/Images/q4.png)
 
I compared the results to the relevant date. 

> **Flag:  34.234.236.212**
  
### 5.Which IP address does not belong to Amazon AWS infrastructure?
I got two ip address ~34.234.236.212~ and ~104.102.221.250~
I checked them on whois site to see the owenr

34.234.236.0/22 belong to  AS14618  ·  Amazon.com, Inc. 

104.102.221.250 belong to  ASN 	AS16625 - Akamai Technologies, Inc.

> **Flag:  104.102.221.250**
 
 ### 6.Which user issued the 'ListBuckets' request?
 Using  `find in files` to search for `ListBuckets`
 
 ![q6](/Bucket/Images/q6.png)
 
 > **Flag:  level3**

 ### 7.What was the first request issued by the user 'level1'?
  Using  `find in files` to search for `level1` 
  
  ![q7](/Bucket/Images/q7.png)
  
 I compared the results to the relevant date. 
 
 > **Flag:  1 CreateLogStream**
