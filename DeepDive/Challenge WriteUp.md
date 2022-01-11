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

![q1](/Injector/Images/q1.png)

> **Flag: Win7SP1x64_24000**

### 2 What is the KDBG virtual address of the memory sample?
To get the KDBG virtual address i used volatility plugin `kdbgscan`

#### Explanation:
>kdbgscan - Search for and dump potential KDBG values

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem kdbgscan`

![q2](/Injector/Images/q2.png)

### 3 There is a malicious process running, but it's hidden. What's its name?
### 4 What is the physical offset of the malicious process?
### 5 What is the full path (including executable name) of the hidden executable?
### 6 Which malware is this?
### 7 The malicious process had two PEs injected into its memory. What's the size in bytes of the Vad that contains the largest injected PE? Answer in hex, like: 0xABC
### 8 This process was unlinked from the ActiveProcessLinks list. Follow its forward link. Which process does it lead to? Answer with its name and extension
### 9 What is the pooltag of the malicious process in ascii? (HINT: use volshell)
### 10 What is the physical address of the hidden executable's pooltag? (HINT: use volshell)
