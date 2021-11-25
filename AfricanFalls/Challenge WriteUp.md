# AfricanFalls Challenge
![This is an image](/AfricanFalls/Images/AfricaHead.png)


## Challenge Information
- Image Size: 	 672 MB
- Image Format: .ad1
- Tags: 
    - FTKImager 
    - Windows 
    - DFIR 
    - Autopsy 
    
    

## Scenario
> John Doe was accused of doing illegal activities. A disk image of his laptop was taken. Your task is to analyze the image and understand what happened under the hood.

## Tools Used
    • FTK Imager
        ◦ FTK® Imager is a data preview and imaging tool
    • Chromecacheview
        ◦ ChromeCacheView is a small utility that reads the cache folder of Google Chrome Web browser, and displays the list of all files currently stored in the cache
    • PECmd
        ◦ Prefetch parser by Eric Zimmerman
    • browsinghistoryview
        ◦ BrowsingHistoryView is a utility that reads the history data of different Web browsers (Mozilla Firefox, Google Chrome, Internet Explorer, Microsoft Edge, Opera)
    • hashcat
        ◦ Hashcat is the world's fastest and most advanced password recovery utility, supporting five unique modes of attack for over 300 highly-optimized hashing algorithms
    • Shellbags Explorer
        ◦GUI for browsing shellbags data. Handles locked files
    • samdump2
         ◦This tool is designed to dump Windows password hashes from a SAM file
         
## Questions:
### 1.What is the MD5 hash value of the suspect disk?
The disk image folder contains two files
`DiskDrigger.ad1` and `DiskDrigger.ad1.txt`,
`DiskDrigger.ad1.txt` contain the information about the image file, include the `MD5` of the file
![q1](/AfricanFalls/Images/q1.png)

> **Flag: 9471e69c95d8909ae60ddff30d50ffa1**

### 2.What phrase did the suspect search for on 2021-04-29 18:17:38 UTC? (three words, two spaces in between)
To find the suspect internet search i used nirsoft tool `chromecacheview` to see all chrome cache history and get the search he did.
![q2](/AfricanFalls/Images/q2.png)

> **Flag: password cracking lists**

### 3.What is the IPv4 address of the FTP server the suspect connected to?
When i was looking at john chrome history i saw he search to download `FileZilla Software`

> FileZilla is a free and open-source, cross-platform FTP application) 

![q3a](/AfricanFalls/Images/q3a.png)

FileZilla have configuration file that contain information about recenets servers used.

`01Win10.e01_Partition 2_[50647MB]_NONAME{[NTFS]\[root]\Users\John Doe\AppData\Roaming\FileZilla\`

In that folder was the file `recentservers.xml`

![q3b](/AfricanFalls/Images/q3b.png)

> **Flag: 192.168.1.20**


### 4.What date and time was a password list deleted in UTC? (YYYY-MM-DD HH:MM:SS UTC)
This one came from `FTK Imager`, recycle bin section has two deleted files `$IW9BJ2Z.txt` and `$RW9BJ2Z.txt`. The original path of `$RW9BJ2Z.txt` was `C:\Users\John Doe\Downloads\10-million-password-list-top-100.txt`

![q4](/AfricanFalls/Images/q4.png)

> **Flag: 2021-04-29 18:22:17 UTC**

### 5.How many times was Tor Browser ran on the suspect's computer? (number only)
> The Tor Browser is a web broswer that anonymizes your web traffic using the Tor network, making it easy to protect your identity online

This was a hard one and took me a while, program execution on Windows systems are store in several places, i used windows `prefetch` to search for evidences of `Tor Browser` execution. I used `PECmd` tool to process Prefetch files `(.pf)` and output the results to csv with the command:
`G:\>PECmd.exe -d "G:\cyberdefenders.org\files\[root]\Windows\Prefetch\" --csv "g:\temp" --csvf foo.csv`

I opend the csv and search for Tor Browser

![q5](/AfricanFalls/Images/q5.png)

we can see `TORBROWSER-INSTALL-WIN64-10.0` was executed,but there's is no mention in Installed Programs of Tor being installed, so maybe Tor Browser program wasn't executed?

> **Flag: 0**

### 6.What is the suspect's email address?
I assumed that john was logged in to the email from browser ,so i search the evidences in broswer history

![q6](/AfricanFalls/Images/q6.png)

Found only one email address in broswer history.
John logged in to his ProtonMail inbox.

> “ProtonMail is an email provider/service that respects privacy and puts people (not advertisers) first.”


 > **Flag :dreammaker82<i></i>@protonmail.com**

### 7.What is the FQDN did the suspect port scan?
During my investigation i was looking for command history artifacts, this artifacts can give me the ability to see what happens in command prompt history and console output.
#### PowerShell console output default location of this file:
> $env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

![q7](/AfricanFalls/Images/q7.png)

The command `nmap` dfir.science was executed in `PowerShell` console.
> Nmap is a free and open-source network scanner )

> **Flag: dfir.science**

### 8.What country was picture "20210429_152043.jpg" allegedly taken in?
The `Exif` format has standard tags for location information. many cameras and most mobile phones have a built-in GPS receiver that stores the location information in the Exif header when a picture is taken.
I saved the image From `FTK Imager` and got to properties and get the `Latitude and Longtitude` and check the coordinates to get the location

![q8](/AfricanFalls/Images/q8.png)
 
> **Flag:Zambia**

### 9.What is the parent folder name picture "20210429_151535.jpg" was in before the suspect copy it to "contact" folder on his desktop?
By looking in image properties i saw its taken by `LG Electronics LM-Q725K` cell phone

![q9](/AfricanFalls/Images/q9.png)


I know many cameras store photos in a `DCIM`, or a subfolder of this folder.
I try to looked at `ShellBags` Artifact With `Shellbags Explorer`

> ShellBags - Microsoft Windows registry keeps track of folder settings in order to enhance the user experience in ShellBags. Windows ShellBags primary purpose is to improve user experience and “remember” preferences while browsing folders. Information stored in ShellBags can be critical during forensic investigation.

ShellBags located at `UsrClass.dat` hive in the registry:

`HKCR\Local Settings\Software\Microsoft\Windows\Shell\Bags`

`HKCR\Local Settings\Software\Microsoft\Windows\Shell\Bag\MRU`


![q9b](/AfricanFalls/Images/q9b.png)

Picture located in `My Computer\LGQ7\Internal storage\DCIM\Camera`
> **Flag:Camera**


### 10.A Windows password hashes for an account are below. What is the user's password?Anon:1001:aad3b435b51404eeaad3b435b51404ee:3DE1A36F6DDB8E036DFD75E8E20C4AF4:::
The `aad3b435b51404eeaad3b435b51404ee` is LM hash and `3DE1A36F6DDB8E036DFD75E8E20C4AF4` is NT.

I try to use `Hashcat` to crack the password through `rockyou.txt` list and found nothing. 

So i added `OneRuleToRuleThemAll` ruleset and try again. 

> hashcat -m 1000 -a 0 "3DE1A36F6DDB8E036DFD75E8E20C4AF4" /home/rhc/Downloads/rockyou.txt -r /home/rhc/Downloads/OneRuleToRuleThemAll.rule 

![q10](/AfricanFalls/Images/q11.png)

**Bingo!**
Its works

> **Flag:AFR1CA!**


### 11.What is the user "John Doe's" Windows login password?
Windows login password stored in `SAM` file located in `/Windows/System32/config/SAM`

I extracted the login data using `samdump2` tool with this command line

`samdump2 -o /home/rhc/Desktop/1.txt /home/rhc/Downloads/SYSTEM /home/rhc/Downloads/SAM`

![q11a](/AfricanFalls/Images/q11a.png)

The hash for John Doe is `ecf53750b76cc9a62057ca85ff4c850e`

I used `Hashcat` to crack the password

> hashcat  -m 1000 -a3  "ecf53750b76cc9a62057ca85ff4c850e"

![q11](/AfricanFalls/Images/q10.png)

> **Flag:ctf2021**
