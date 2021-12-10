# Injector Challenge
![This is an image](/Injector/Images/Injectorhead.png)

## Challenge Link
https://cyberdefenders.org/labs/23

## Challenge Information
- Image Size: 	 2.9 GB
- Image Format: .mem & Image file
- Tags: 
    -  	Volatility 
    -  	Autopsy 
    -  	Web Server 
    -  	DFIR 
    -  	Memory analysis 
    -  	FTK Imager 
    
    

## Scenario
> A company’s web server has been breached through their website. Our team arrived just in time to take a forensic image of the running system and its memory for further analysis. As a security analyst, you are tasked with mounting the image to determine how the system was compromised and the actions/commands the attacker executed.
## TO add tool

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
  	
### 1 	What is the computer's name?
I used `Regripper` to extract the information from the registry hive into readable text file, the files location `\Windows\System32\config\`

Open the `SOFTWARE` file to get the computer name

#### Registry Path:
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName

![q1](/Injector/Images/q1.png)

> **Flag: WIN-L0ZZQ76PMUF**

### 2 	What is the Timezone of the compromised machine? Format: UTC+0 (no-space)
I used `Regripper` to extract the information from the registry hive into readable text file, the files location `\Windows\System32\config\`

Open the `SYSTEM` file to get the machine timezone

### Registry Path:
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation

![q2](/Injector/Images/q2.png)

The time zone is in `Pacific Standard Time` format, Places in this zone observe standard time by subtracting eight hours from Coordinated Universal Time (UTC−08:00). During daylight saving time, a time offset of UTC−07:00 is used. 

#### Source:
> https://en.wikipedia.org/wiki/Pacific_Time_Zone

> **Flag: UTC-7**

### 3 	What was the first vulnerability the attacker was able to exploit?
When I went through the files inside the IMAGE, I notice the folder `xampp`.

#### #### Explanation:
> XAMPP is a software distribution which provides the Apache web server, MySQL database (actually MariaDB), Php and Perl.

`XAMPP `store the access log in `\xampp\apache\logs\access.log`

So I know that the most common web vulnerabilities are: 

#### XSS:
> Cross-Site Scripting (XSS) attacks are a type of injection, in which malicious scripts are injected into otherwise benign and trusted websites

#### SQL:
> SQL injection, also known as SQLI, is a common attack vector that uses malicious SQL code for backend database manipulation to access information that was not intended to be displayed

#### LFI:
> Local file inclusion occurs when the web application executes a local files i.e. files on the current server can be included for execution. These files are usually obtained in the form of an HTTP or FTP URI as a user-supplied parameter to the web application. 

First, i started to search evidence for XSS attack in the access log.
The common XSS attack are with script tags

#### Example:
`<script>alert('xss');</script>`

![q3](/Injector/Images/q3.png)

I found this line in the log `GET /dvwa/security.php?test=%22><script>eval(window.name)</script> HTTP/1.1" 200`

The line contain XSS attempt

> **Flag: XSS**


### 4 	What is the OS build number?
I used `Regripper` to extract the information from the registry hive into readable text file, the files location `\Windows\System32\config\`
Open the `SOFTWARE` file to get the os build

#### Registry Path:
> HKEY_LOCAL_MACHINE\software\microsoft\windows nt\currentversion

![q4](/Injector/Images/q4.png)

> **Flag: 6001**

### 5 	How many users are on the compromised machine?
I used `Regripper` to extract the information from the registry hive into readable text file, the files location `\Windows\System32\config\`
Open the `SAM` file to get the list of the users 

> SAM file is database used to store user account information, including password, account groups, access rights, and special privileges in Windows operating system.

![q5](/Injector/Images/q5.png)

> **Flag: 4**

### 6 	What is the webserver package installed on the machine?
I used `Regripper` to extract the information from the registry hive into readable text file, the files location `\Windows\System32\config\`
I used the `SOFTWARE` file to get the list of installed Programs

#### Registry Path:
> For 32bit system: Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall 

> For 64bit system: Software\Microsoft\Windows\CurrentVersion\Uninstall

![q6](/Injector/Images/q6.png)

#### Explanation:
> XAMPP is a software distribution which provides the Apache web server, MySQL database (actually MariaDB), Php and Perl 

> **Flag: xampp**

### 7 	What is the name of the vulnerable web app installed on the webserver?
So we know that the webserver program is `XAMPP`.

`XAMPP` stored the web apps in `\xampp\htdocs`

![q7](/Injector/Images/q7.png)

#### Explanation:
> DVWA The Damn Vulnerable Web Application is a software project that intentionally includes security vulnerabilities and is intended for educational purposes.

> **Flag: dvwa**

### 8 	What is the user agent used in the HTTP requests sent by the SQL injection attack tool?

I searched evidence for SQL injection attack in access.log 

The common SQL injection syntex

#### Example:
> a'+or+1=1

![q8](/Injector/Images/q8.png)

I found this line in the log `"GET /dvwa/vulnerabilities/sqli/?id=a%27+or+1%3D1&Submit=Submit"`
I decoded the data with url decoding and get`"GET/dvwa/vulnerabilities/sqli/?id=a'+or+1=1&Submit=Submit"', this match to SQL injection attempt
I looked down the log and saw the line `"sqlmap/1.0-dev-nongit-20150902 (http://sqlmap.org)"`

#### Explanation:
> sqlmap is an open source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over of database 

> **Flag: sqlmap/1.0-dev-nongit-20150902**

### 9 	The attacker read multiple files through LFI vulnerability. One of them is related to network configuration. What is the filename?

As i explaind before: 

#### LFI:
> Local file inclusion occurs when the web application executes a local files i.e. files on the current server can be included for execution. These files are usually obtained in the form of an HTTP or FTP URI as a user-supplied parameter to the web application. 

The common LFI attack syntex 

#### Example:
> ../../../../../etc/passwd%00 - allows an attacker to read the contents of the local /etc/passwd

I searched `./../../../../` in access.log and found `"GET /dvwa/vulnerabilities/fi/?page=../../../../../../windows/system32/drivers/etc/hosts`

![q9](/HireMe/Images/q9.png)

The `hosts` file is a plain text file used to map host names to IP addresses

> **Flag: hosts**

### 10 The attacker tried to update some firewall rules using netsh command. Provide the value of the type parameter in the executed command?


> **Flag: remotedesktop**

### 11 How many users were added by the attacker?


> **Flag: 2**

### 12 When did the attacker create the first user?

> **Flag:  2015-09-02 09:05:06 UTC **

### 13 What is the NThash of the user's password set by the attacker?

> **Flag: 817875ce4794a9262159186413772644**

### 14 What is The MITRE ID corresponding to the technique used to keep persistence?

> **Flag:  T1136.001**

### 15 The attacker uploaded a simple command shell through file upload vulnerability. Provide the name of the URL parameter used to execute commands?

> **Flag: cmd**

### 16 One of the uploaded files by the attacker has an md5 that starts with "559411". Provide the full hash.

> **Flag: 5594112b531660654429f8639322218b**

### 17 The attacker used Command Injection to add user "hacker" to the "Remote Desktop Users" Group. Provide the IP address that was part of the executed command?

> **Flag:  192.168.56.102**

### 18 The attacker dropped a shellcode through SQLi vulnerability. The shellcode was checking for a specific version of PHP. Provide the PHP version number?

> **Flag:  4.1.0**

         
