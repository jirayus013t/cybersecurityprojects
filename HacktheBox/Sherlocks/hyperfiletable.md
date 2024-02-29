

**Evidence:** mft.raw
**Tool:** MFTExplorer.exe 


**Optional** (In case something goes wrong and mft.raw could't be loaded in MFTExplorer.exe): 
Reference: https://github.com/jon-brandy/hackthebox/blob/main/Categories/Sherlocks/Hyperfiletable/README.md
Converting mft.raw into .csv and then load it in TimelineExplorer
```
python3 'C:\CTF\TOOLS-FOREN\analyzeMFT\analyzeMFT.py' -f .\mft.raw -o analyzed_mft.csv
```



**Questions:**


- **What is the MD5 hash of the MFT?**


Upload MFT.raw to https://emn178.github.io/online-tools/md5_checksum.html to get MD5 Checksum

![[Pasted image 20240203171943.png]]

Answer: 3730c2fedcdc3ecd9b83cbea08373226



- **What is the name of the only user on the system?**

![[Pasted image 20240203172617.png]]

Answer: Randy Savage

- **What is the name of the malicious HTA that was downloaded by that user?**

![[Pasted image 20240203181746.png]]


Answer: Onboarding.hta

- **What is the ZoneId of the download for the malicious HTA file?**


![[Pasted image 20240203182002.png]]

Answer: 3

- **What is the download URL for the malicious HTA?**


Copy the following and paste it in notepad to make it easier to read:

![[Pasted image 20240203182222.png]]

![[Pasted image 20240203182255.png]]



Answer: https://doc-10-8k-docs.googleusercontent.com/docs/securesc/9p3kedtu9rd1pnhecjfevm1clqmh1kc1/9mob6oj9jdbq89eegoedo0c9f3fpmrnj/1680708975000/04991425918988780232/11676194732725945250Z/1hsQhtmZJW9xZGgniME93H3mXZIV4OKgX?e=download&uuid=56e1ab75-ea1e-41b7-bf92-9432cfa8b645&nonce=u98832u1r35me&user=11676194732725945250Z&hash=j5meb42cqr57pa0ef411ja1k70jkgphq

- **What is the allocated size for the HTA file? (bytes)**


convert it to bytes
![[Pasted image 20240203183628.png]]


Answer 4096

- **What is the real size of the HTA file? (bytes)**



![[Pasted image 20240203183843.png]]

Answer: 1144



- **When was the powerpoint presentation downloaded by the user?**

![[Pasted image 20240203184237.png]]


Answer: 05/04/2023 13:11:49


- **The user has made notes of their work credentials, what is their password?**

![[Pasted image 20240203184442.png]]


Answer: ReallyC00lDucks2023!

- **How many files remain under the C:\Users\ directory? (Recursively)**
Answer: 3471
