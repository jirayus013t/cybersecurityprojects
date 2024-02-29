**Sherlock Scenario:**
Your SIEM system generated multiple alerts in less than a minute, indicating potential C2 communication from Simon Stark's workstation. Despite Simon not noticing anything unusual, the IT team had him share screenshots of his task manager to check for any unusual processes. No suspicious processes were found, yet alerts about C2 communications persisted. The SOC manager then directed the immediate containment of the workstation and a memory dump for analysis. As a memory forensics expert, you are tasked with assisting the SOC team at Forela to investigate and resolve this urgent incident.

**Data Preparation:**
Tools: Votatility3
Tool Path (Windows): C:\Users\johnn\Downloads\SherlockPractice\volatility3-1.0.0\volatility3-1.0.0
Tool Path (Kali): /home/kali/Downloads/Sherlock/volatility3-1.0.0/volatility3-1.0.0
Memory dump: C:\Users\johnn\Downloads\SherlockPractice\20230810.mem



```
python3 .\vol.py -f 'C:\Users\johnn\Downloads\SherlockPractice\20230810.mem' windows.info
```



Exercise



- Please identify the malicious process and confirm process id of malicious process.

```
python3 .\vol.py -f 'C:\Users\johnn\Downloads\SherlockPractice\20230810.mem' windows.malfind
```

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/b527460b-3376-4df8-b5c9-8b622dcbf057)

Answer: 6812


- The SOC team believe the malicious process may spawned another process which enabled threat actor to execute commands. What is the process ID of that child process?



```
python3 .\vol.py -f 'C:\Users\johnn\Downloads\SherlockPractice\20230810.mem' windows.pslist
```

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/e1b913b9-9f8e-4bd4-bf6d-6a3140d4a901)


Answer 4364 (cmd.exe)

- The reverse engineering team need the malicious file sample to analyze. Your SOC manager instructed you to find the hash of the file and then forward the sample to reverse engineering team. Whats the md5 hash of the malicious file?

Dump all files related to pid 6812 (svchost.exe)
```
python3 vol.py -f '/home/kali/Downloads/Sherlock/20230810.mem' -o . windows.dumpfiles.DumpFiles --pid 6812
```
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/88492c97-d63c-4297-be54-ea343bd73ec6)


Run md5sum command to get a hash of svchost.exe

```
sudo md5sum ./DumpedFileTempDelete/file.0x9e8b91ec0140.0x9e8b957f24c0.ImageSectionObject.svchost.exe.img 
```

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/32c18906-a279-46fa-9e8d-7028d1710e90)




Answer: 5bd547c6f5bfc4858fe62c8867acfbb5

- In order to find the scope of the incident, the SOC manager has deployed a threat hunting team to sweep across the environment for any indicator of compromise. It would be a great help to the team if you are able to confirm the C2 IP address and ports so our team can utilise these in their sweep.

```
python3 .\vol.py -f 'C:\Users\johnn\Downloads\SherlockPractice\20230810.mem' windows.netscan
```


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/23f82666-2be8-43b5-8265-2b6fc5dea0d0)



Answer: 13.127.155.166:8888


- We need a timeline to help us scope out the incident and help the wider DFIR team to perform root cause analysis. Can you confirm time the process was executed and C2 channel was established?

Pick the date from the previous question

Answer: 10/08/2023 11:30:03

- What is the memory offset of the malicious process?

```
python3 .\vol.py -f 'C:\Users\johnn\Downloads\SherlockPractice\20230810.mem' windows.psscan --pid 6812
```

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/93d1dadc-4c3f-4602-a3fb-d7b50d46f224)




- You successfully analyzed a memory dump and received praise from your manager. The following day, your manager requests an update on the malicious file. You check VirusTotal and find that the file has already been uploaded, likely by the reverse engineering team. Your task is to determine when the sample was first submitted to VirusTotal.

copy the md5 hash from the previous question (hash of svchost.exe) and paste it in virus total, then go to detail tab to see the first submission date

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/08bfd4ed-0b01-455c-9403-b77726e8176b)




Answer: 10/08/2023 11:58:10
