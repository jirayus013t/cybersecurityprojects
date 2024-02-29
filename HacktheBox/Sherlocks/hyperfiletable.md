**Sherlock Scenario:**
There has been a new joiner in Forela, they have downloaded their onboarding documentation, however someone has managed to phish the user with a malicious attachment. We have only managed to pull the MFT record for the new user, are you able to triage this information?

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

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/f9f4a9cf-e44c-46da-9199-d9c06d4aca85)


Answer: 3730c2fedcdc3ecd9b83cbea08373226



- **What is the name of the only user on the system?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/0bcb58b2-d733-4ca5-9b3e-75459e29a278)


Answer: Randy Savage

- **What is the name of the malicious HTA that was downloaded by that user?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/e1907d45-e3b6-410a-a639-cb2db2c49259)



Answer: Onboarding.hta

- **What is the ZoneId of the download for the malicious HTA file?**


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/e334aad9-3ade-4942-a413-b212f44ea800)


Answer: 3

- **What is the download URL for the malicious HTA?**


Copy the following and paste it in notepad to make it easier to read:

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/fbed5826-4737-4605-9ca0-46304eb8870a)


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/831b58b8-b2ed-4204-ba9e-a4005f0cfcf8)




Answer: https://doc-10-8k-docs.googleusercontent.com/docs/securesc/9p3kedtu9rd1pnhecjfevm1clqmh1kc1/9mob6oj9jdbq89eegoedo0c9f3fpmrnj/1680708975000/04991425918988780232/11676194732725945250Z/1hsQhtmZJW9xZGgniME93H3mXZIV4OKgX?e=download&uuid=56e1ab75-ea1e-41b7-bf92-9432cfa8b645&nonce=u98832u1r35me&user=11676194732725945250Z&hash=j5meb42cqr57pa0ef411ja1k70jkgphq

- **What is the allocated size for the HTA file? (bytes)**


convert it to bytes
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/38de0759-ddac-4a11-a87e-6258c7787b71)



Answer 4096

- **What is the real size of the HTA file? (bytes)**



![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d1a037c1-7750-45a2-90e0-07dcd90a6acb)


Answer: 1144



- **When was the powerpoint presentation downloaded by the user?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/b85abd69-7920-4ea3-9747-baf2ac6fc211)



Answer: 05/04/2023 13:11:49


- **The user has made notes of their work credentials, what is their password?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/4312e292-fe49-4f8d-8f58-1fbe3965731a)



Answer: ReallyC00lDucks2023!

- **How many files remain under the C:\Users\ directory? (Recursively)**
Answer: 3471
