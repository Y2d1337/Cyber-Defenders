# DeepDive Challenge
![This is an image](/DeepDive/Images/DeepDivehead.png)

## Challenge Link
https://cyberdefenders.org/labs/78

## Challenge Information
- Image Size: 	 526 MB
- Image Format: .mem file
- Tags: 
    -  	Volatility 
 

## Scenario
> You have given a memory image for a compromised machine. Analyze the image and figure out attack details.

## Tools Used
    • Volatility
        ◦ Volatility is an open-source memory forensics framework for incident response and malware analysis.     
         
## Questions:  
### 1 What profile should you use for this memory sample?
To determine the image profile i used volatility plugin `imageinfo`  

#### Explanation:
>imageinfo- Identify information for the image

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem imageinfo`

![q1](/DeepDive/Images/q1.png)

> **Flag: Win7SP1x64_24000**

### 2 What is the KDBG virtual address of the memory sample?
To get the KDBG virtual address i used volatility plugin `kdbgscan`

#### Explanation:
>kdbgscan - Search for and dump potential KDBG values

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 kdbgscan`

![q2](/DeepDive/Images/q2.png)

> **Flag: 0xf80002bef120**

### 3 There is a malicious process running, but it's hidden. What's its name?
To find the hidden process we need to use `psxview`,this plugin helps you detect hidden processes by comparing what PsActiveProcessHead contains with what is reported by various other sources of process listings

A False within the column indicates that the process is not found in that area

#### Explanation:
>psxview - Find hidden processes with various process

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 psxview`

![q3](/DeepDive/Images/q3.png)

> **Flag:  vds_ps.exe**

### 4 What is the physical offset of the malicious process?
`psxview` plugin shows the physical address of each process, so we can looked at 3rd questions output.

![q4](/DeepDive/Images/q4.png)

> **Flag:  0x000000007d336950**

### 5 What is the full path (including executable name) of the hidden executable?
To find the full path of the hidden executable we need to use `filescan` plugin

#### Explanation:
>filescan - Pool scanner for file objects

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 filescan | grep vds_ps.exe` 

![q5](/DeepDive/Images/q5.png)

> **Flag:  C:\Users\john\AppData\Local\api-ms-win-service-management-l2-1-0\vds_ps.exe**

### 6 Which malware is this?
To find the malware type, i dumped the exe file from the memory and check his hash in virustotal.
We need to use the process physical offset from the filescan results NOT the PID. 

Because the process is hidden and dont shows up on `pslist` and `psscan`

#### Explanation:
>dumpfiles - Extract memory mapped and cached files

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 dumpfiles -Q 0x000000007d0d57e0 --dump-dir=/home/sansforensics/Desktop/dump` 

![q6a](/DeepDive/Images/q6a.png)

After we get the file lets get his md5 hash

#### Commandline:
`md5sum vds_ps.exe`

![q6b](/DeepDive/Images/q6b.png)

Now we search the hash in virustotal

![q6](/DeepDive/Images/q6.png)

> **Flag:  Emotet**

### 7 The malicious process had two PEs injected into its memory. What's the size in bytes of the Vad that contains the largest injected PE? Answer in hex, like: 0xABC
There are two PEs injected into the memory so it's mean there are 2 protected VADs, When a `VAD` has a specific protection that includes the `WRITE` and `EXECUTE` rights,it will identify this `VAD` as potentially malicious 
So i looked for `PAGE_EXECUTE_WRITECOPY` or `PAGE_EXECUTE_READWRITE` in `VadS` protection using the plugin `malfind`.

#### Explanation:
>malfind - Find hidden and injected code, based on characteristics such as VAD tag and page permissions.

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 malfind -o 0x000000007d336950`

![q7a](/DeepDive/Images/q7a.png)

We can see there are two executables injected (`MZ` header) into alicious process memory.
We need to get `VAD` info of the executables to calculate the size between the start and end using the plugin `vadinfo`.

#### Explanation:
>vadinfo - Dump the VAD info

#### Commandlines:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 vadinfo -a 0x2a80000 -o 0x000000007d336950`

`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 vadinfo -a 0x2a10000 -o 0x000000007d336950`

![q7](/DeepDive/Images/q7.png)

### 8 This process was unlinked from the ActiveProcessLinks list. Follow its forward link. Which process does it lead to? Answer with its name and extension
### 9 What is the pooltag of the malicious process in ascii? (HINT: use volshell)
### 10 What is the physical address of the hidden executable's pooltag? (HINT: use volshell)
