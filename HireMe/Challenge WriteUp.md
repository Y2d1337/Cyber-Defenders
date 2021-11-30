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
Usually zip code is related to registration form in sites,so i searched in browser history a POST request, but i didn't find anything

So i looked at the browser autofill option, maybe the information is saved by the user

#### Path:
> Users\Karen\AppData\Local\Google\Chrome\User Data\Default\Web Data

![q5](/HireMe/Images/q5.png)

> **Flag: 19709**


### 6.What are the initials of the person who contacted the admin user from TAAUSAI?
When i looked at the files on the mounted image, i notice the user have his email account attached to outlook.
So i open the `klovespizza@outlook.com.ost` file with `Kernel OST Viewer` to see all the emails.

> OST files may contain email messages, contacts, tasks, calendar data, and other account information.

#### Path:
> \Users\Karen\AppData\Local\Microsoft\Outlook\

The email account gave me alots of new infomartion, i find conversation between Karen and Micheal Scotch from TAAUSAI

The email was signed with micheal initials

![q6](/HireMe/Images/q6a.png)

> **Flag: MS**

### 7.How much money was TAAUSAI willing to pay upfront?

Micheal offered karen job at TAAUSAI

![q7](/HireMe/Images/q7.png)

> **Flag: 150000**

### 8.What country is the admin user meeting the hacker group in?

One of the email contained GPS coordinates `"27°22’50.10″N, 33°37’54.62″E"`

![q8](/HireMe/Images/q8.png)

I Checked the coordinates and found the location

![q82](/HireMe/Images/q8-2.png)

> **Flag: Egypt**

### 9.What is the machine's timezone? (Use the three-letter abbreviation)

I used the `SYSTEM` file to get the machine timezone

#### Registry Path:
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation

![q9](/HireMe/Images/q9.png)

> **Flag: UTC**

### 10.When was AlpacaCare.docx last accessed?

To get the last accessed time i preferred to extract the data from the `$MFT` file because is much accurate

> The $MFT, Master File Table keeps track of all files on the volume, their logical location in folders, their physical location on the hard, and metadata about the files, including:Created Date, Entry Modified Date, Accessed Date and Last Written Date, in the Standard Information Attribute

To extract the `$MFT` into csv file i used `MFTECmd` 

> MFTECmd.exe -f "E:\Horcrux.E01_Partition 3 [3122MB]_PacaLady [NTFS]\[root]\$MFT" --csv "c:\temp\out" --csvf MyOutputFile.csv

![q10](/HireMe/Images/q10.png)

Convert the time into 12 HR format

> **Flag:  03/17/2019 09:52 PM**

### 11.There was a second partition on the drive. What is the letter assigned to it?

I used the `SYSTEM` file to get the second partition

> HKEY_LOCAL_MACHINE\SYSTEM\MountedDevices

![q11](/HireMe/Images/q11.png)

> **Flag:  A**


### 12.What is the answer to the question Company's manager asked Karen?

One of the email contained the question `Can you tell me what the answer to the thing at the bottom is? VGhlQ2FyZENyaWVzTm9Nb3Jl`

![q12](/HireMe/Images/q12.png)

Karen answered was TheCardCriesNoMore (after decode with base64)

> **Flag:  TheCardCriesNoMore**

### 13.What is the job position offered to Karen? (3 words, 2 spaces in between)

One of the email contained the job position

![q13](/HireMe/Images/q13.png)

> **Flag:  Cyber Security Analyst**


### 14.When was the admin user password last changed?

I used the `SAM` file to get the password last time changed

![q14](/HireMe/Images/q14.png)

 > **Flag:  03/21/2019 19:13:09** 

### 15.What version of Chrome is installed on the machine?

I used the `SOFTWARE` file to get the list of installed Programs

#### Registry Path:
> For 32bit system: Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall 

> For 64bit system: Software\Microsoft\Windows\CurrentVersion\Uninstall

![q15](/HireMe/Images/q15.png)

 > **Flag:   72.0.3626.121** 

### 16.What is the HostUrl of Skype?
I used `BrowserDownloadsView` to see the downloads from the browser

![q16](/HireMe/Images/q16.png)

 > **Flag:    https://download.skype.com/s4l/download/win/Skype-8.41.0.54.exe**

### 17.What is the domain name of the website Karen browsed on Alpaca care that the file AlpacaCare.docx is based on?

I opened the Alpaca Care.docx and inside it there was the domain name

![q17](/HireMe/Images/q17.png)

 > **Flag:     palominoalpacafarm.com **
