# HireMe Challenge
![This is an image](/HireMe/Images/hiremehead.png)

## Challenge Link
https://cyberdefenders.org/labs/62

## Challenge Information
- Image Size: 	 327 MB
- Image Format: .ad1
- Tags: 
    - Registry
    - Windows 
    - Forensics
    - Autopsy 
    - AccessData
    - Skype   
    - Timeline Analysis  
    
    

## Scenario
> Karen is a security professional looking for a new job. A company called "TAAUSAI"  offered her a position and asked her to complete a couple of tasks to prove her technical competency. Analyze the provided disk image and answer the questions based on your understanding of the cases she was assigned to investigate.

## Tools Used
    • FTK Imager
        ◦ FTK® Imager is a data preview and imaging tool
    • Chromecacheview
        ◦ ChromeCacheView is a small utility that reads the cache folder of Google Chrome Web browser, and displays the list of all files currently stored in the cache
    • Regripper 
        ◦ RegRipper is an open source tool, written in Perl, for extracting/parsing information (keys, values, data) from the Registry and presenting it for analysis
    • browsinghistoryview
        ◦ BrowsingHistoryView is a utility that reads the history data of different Web browsers (Mozilla Firefox, Google Chrome, Internet Explorer, Microsoft Edge, Opera)
    • hashcat
        ◦ Hashcat is the world's fastest and most advanced password recovery utility, supporting five unique modes of attack for over 300 highly-optimized hashing algorithms
    • Shellbags Explorer
        ◦GUI for browsing shellbags data. Handles locked files
    • samdump2
         ◦This tool is designed to dump Windows password hashes from a SAM file
         
 ## Questions:       
         
### 1.What is the administrator's username?
I used `Regripper` to extract the information from the registry hive into readable text file, the files location `Horcrux.E01_Partition 2 [32216MB]_NONAME [NTFS]\[root]\Windows\System32\config\`

I used the `SAM` file to get the users in administrators localgroup

> SAM file is database used to store user account information, including password, account groups, access rights, and special privileges in Windows operating system.

I get two `SID` number 
> The SID (Security IDentifier) is a unique ID number that a computer or domain controller uses to identify you.

`S-1-5-21-1649836244-3544936428-1548601679-1001`

`S-1-5-21-1649836244-3544936428-1548601679-500`

To see the user behind the `SID` i used the `SOFTWARE` file to get the user profile list

#### Registry Path:
> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList.

![q1](/HireMe/Images/q1.png)

The `SID S-1-5-21-1649836244-3544936428-1548601679-1001` belong to the user karen


> **Flag: Karen**


### 2.What is the OS's build number?
I used the `SOFTWARE` file to get the os build

#### Registry Path:
> HKEY_LOCAL_MACHINE\software\microsoft\windows nt\currentversion

![q2](/HireMe/Images/q2.png)


> **Flag: 16299**

### 3.What is the hostname of the computer?
I used the `SOFTWARE` file to get the hostname of the computer

#### Registry Path:
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName

![q3](/HireMe/Images/q3.png)

> **Flag: TOTALLYNOTAHACK**

### 4.A messaging application was used to communicate with a fellow Alpaca enthusiest. What is the name of the software?

After mounting the image file, i got two disk partition,
One partition for the OS system and the other one contains different files

![q4](/HireMe/Images/q4.png)

> **Flag: Skype**

### 5.What is the zip code of the administrator's post?
Usually zip code is related to registration form in sites,so i searched in browser history and cache a POST request , but i didn't find anything

So i try to look at the broswer autofill option

#### Path:
> Users\Karen\AppData\Local\Google\Chrome\User Data\Default\Web Data

![q5](/HireMe/Images/q5.png)

> **Flag: 19709**


### 6.What are the initials of the person who contacted the admin user from TAAUSAI?

### 7.How much money was TAAUSAI willing to pay upfront?

### 8.What country is the admin user meeting the hacker group in?

### 9.What is the machine's timezone? (Use the three-letter abbreviation)

### 10.When was AlpacaCare.docx last accessed?

### 11.There was a second partition on the drive. What is the letter assigned to it?

### 12.What is the answer to the question Company's manager asked Karen?

### 13.What is the job position offered to Karen? (3 words, 2 spaces in between)

### 14.When was the admin user password last changed?

### 15.What version of Chrome is installed on the machine?

### 16.What is the HostUrl of Skype?

### 17.What is the domain name of the website Karen browsed on Alpaca care that the file AlpacaCare.docx is based on?
       
