# L'espion Challenge
![This is an image](/L'espion/Images/L'espionhead.png)

## Challenge Link
https://cyberdefenders.org/blueteam-ctf-challenges/73

## Challenge Information
- Image Size: 	 1.14 MB
- Image Format: Zip file
- Tags: 
    - OSINT 
    - Github 
    - BloodHound 
    - Mimikatz 
 
## Scenario
>You have been tasked by a client whose network was compromised and brought offline to investigate the incident and determine the attacker's identity.
>Incident responders and digital forensic investigators are currently on the scene and have conducted a preliminary investigation. Their findings show that the attack originated from a single user account, probably, an insider.
>Investigate the incident, find the insider, and uncover the attack actions.
            
## Questions:  
### 1 File -> Github.txt: What is the API key the insider added to his GitHub repositories?

Inside the txt file was link to github user https://github.com/EMarseille99/, I looked at user contribution activity

The last time the user has contribution was on may 24,2020 

![q1](/L'espion/Images/q1.png)

I clicked the green square to see all changes, 
I saw that on may 24 2020 the user created 4 commits in the repository

![q1a](/L'espion/Images/q1a.png)

Inside the repository - `Project-Build---Custom-Login-Page` there was 4 commits

![q1b](/L'espion/Images/q1b.png)

I checked the `Update Login Page.js` 

![q1c](/L'espion/Images/q1c.png)

> **Flag: aJFRaLHjMXvYZgLPwiJkroYLGRkNBW**

### 2 File -> Github.txt: What is the plaintext password the insider added to his GitHub repositories?
Continuing from the previous question, I checked Create Login Page

![q2a](/L'espion/Images/q2a.png)

The password was encoded with base64

```base64
UGljYXNzb0JhZ3VldHRlOTk=
```
So i decoded it

![q2b](/L'espion/Images/q2b.png)

> **Flag: PicassoBaguette99**

### 3 File -> Github.txt: What cryptocurrency mining tool did the insider use?
I checked the user other contribution and found on may 20,2020 

![q3](/L'espion/Images/q3.png)

I know `xmrig` is cryptocurrency mining tool

> **Flag: xmrig**

### 4 What university did the insider go to?
The answer will not appeared in github so i googled the username `EMarseille99`

I found here linkedin profile

https://www.linkedin.com/in/%C3%A9milie-marseille-4b353a1aa/

![q4](/L'espion/Images/q4.png)

> **Flag: Sorbonne**

### 5 What gaming website the insider had an account on?
I got the answer to this question after i found her instagram account, the last image has qr code inside

![q5a](/L'espion/Images/q5a.png)

After i scaned the code i redircated to https://steamcommunity.com/id/emarseille99

![q5](/L'espion/Images/q5.png)

> **Flag: steam**


### 6 What is the link to the insider Instagram profile?
From google :)

![q6](/L'espion/Images/q6.png)

> **Flag: https://<i></i>instagram.com/emarseille99/**

### 7 Where did the insider go on the holiday? (Country only)
From the instagram i saw this post

![q7a](/L'espion/Images/q7a.png)

I decided to go for reverse image search 

![q7](/L'espion/Images/q7.png)

Works!

> **Flag: Singapore**

### 8 Where is the insider's family live? (City only)
From the instagram i saw this post

![q8](/L'espion/Images/q8.png)

The flag seem belong to United Arab Emirates so i tried `Dubai` :)

> **Flag: Dubai**

### 9 File -> office.jpg: You have been provided with a picture of the building in which the company has an office. Which city is the company located in?

![q9](/L'espion/Images/office.jpg)

I tried to use reverse image search with no success

So i decided to search places from the street signs, I chose `hippodrome theatre`

![q9](/L'espion/Images/q9.png)

> **Flag: Birmingham**

### 10 File -> Webcam.png: With the intel, you have provided, our ground surveillance unit is now overlooking the person of interest's suspected address. They saw them leaving their apartment and followed them to the airport. Their plane took off and has landed in another country. Our intelligence team spotted the target with this IP camera. Which state is this camera in?

From looked at the picture i decide to search the camera description `A View from the Dome`

The first results from google was `https://dome.nd.edu/`

![q10](/L'espion/Images/WebCam.png)

> **Flag: indiana**
