**Evidence:**

- **Event log files**
	- C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Windows\System32\winevt\logs
- **Prefetch files**
	- C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Windows\prefetch
- **MFT**
	- C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\$MFT
- **NTUSER.DAT**
	- C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Users\gladys\NTUSER.DAT






**Data Preparation:**



Tool Path: C:\Users\johnn\OneDrive\Desktop\Cyber Certificates\Hack the Box\Tools\Get-ZimmermanTools\net6

**Tool: MFTECMD.exe**



Creating MFT output CSV
```
.\MFTECmd.exe -f 'C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\$MFT' --csv C:\Users\johnn\Downloads\SherlockPractice\ticktock\ --csvf MFT.csv
```



Creating USN Journal output CSV
```
.\MFTECmd.exe -f 'C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\$Extend\$J' --csv C:\Users\johnn\Downloads\SherlockPractice\ticktock\ --csvf MFT-J.csv
```




**Tool: EvtxECmd.exe**
Note: cd to \EvtxeCmd path first the run the bellow command

Creating kape_event_log.csv from Sysmon log
```
.\EvtxECmd.exe -f 'C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Windows\System32\winevt\logs\Microsoft-Windows-Sysmon%4Operational.evtx' --csv C:\Users\johnn\Downloads\SherlockPractice\ticktock\ --csvf kape_event_log.csv
```

Creating kape_event_log_Security.csv from Security log
```
.\EvtxECmd.exe -f 'C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Windows\System32\winevt\logs\Security.evtx' --csv C:\Users\johnn\Downloads\SherlockPractice\ticktock\ --csvf kape_event_log_security.csv
```


**Tool: PECmd.exe**

Creating prefetech_analysis.csv
```
./PECmd.exe -d "C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Windows\prefetch" --csv C:\Users\johnn\Downloads\SherlockPractice\ticktock\ --csvf Prefetch_analysis.csv
```

Checking prefetch file
```
./PECmd.exe -f 'C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Windows\prefetch\MERLIN.EXE-E3E5C440.pf'
```


**RegRipper**


```
.\rip.exe -r "C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Users\gladys\NTUSER.DAT" -p recentdocs
```














**Questions:**

- **What was the name of the executable that was uploaded as a C2 Agent?**
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/b5c25d89-e6e6-4c2e-8f33-6fcfc13935a9)


Alternatively, we can find the malicious downloaded file in Teamviewer log 

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/b35eaa44-5415-4752-a9d3-ae977dbe54a9)


Answer: merlin.exe

- **What was the session id for in the initial access?**

**Given:**
**Teamviewer log file**: C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Users\gladys\AppData\Local\TeamViewer\Logs\TeamViewer15_Logfile

Open TeamViewer15_Logfile in Timeline explorer for ease of browsing

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/0c22698d-2d3a-4d9f-8cc2-95f4240b28e7)


Answer: -2102926010




- **The attacker attempted to set a bitlocker password on the C: drive what was the password?**


Search for "merlin.exe" in the sysmon csv file in timeline explorer and we can see 4 instances of merlin.exe

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d49b87ec-a5c9-4717-a190-28ddf825bd55)


Lets investigate all the ParentProcessID which include 4780,1992, and 5768 by searching these events the Timeline explorer filter

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/42f27327-4078-4235-bcad-0c8e30f3fe38)


We can see that there's powershell.exe running some type of encoded command. Let's copy the encoded command and paste to [Cipher Identifier (online tool) | Boxentriq](https://www.boxentriq.com/code-breaking/cipher-identifier)

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/0d1c7773-0985-4727-8fcf-b3988d7e2e99)


It's Base64 encoding technique, to decode, we run this command:

```
echo JABTAGUAYwB1AHIAZQBTAHQAcgBpAG4AZwAgAD0AIABDAG8AbgB2AGUAcgB0AFQAbwAtAFMAZQBjAHUAcgBlAFMAdAByAGkAbgBnACAAIgByAGUAYQBsAGwAeQBsAG8AbgBnAHAAYQBzAHMAdwBvAHIAZAAiACAALQBBAHMAUABsAGEAaQBuAFQAZQB4AHQAIAAtAEYAbwByAGMAZQAKAEUAbgBhAGIAbABlAC0AQgBpAHQATABvAGMAawBlAHIAIAAtAE0AbwB1AG4AdABQAG8AaQBuAHQAIAAiAEMAOgAiACAALQBFAG4AYwByAHkAcAB0AGkAbwBuAE0AZQB0AGgAbwBkACAAQQBlAHMAMgA1ADYAIAAtAFUAcwBlAGQAUwBwAGEAYwBlAE8AbgBsAHkAIAAtAFAAaQBuACAAJABTAGUAYwB1AHIAZQBTAHQAcgBpAG4AZwAgAC0AVABQAE0AYQBuAGQAUABpAG4AUAByAG8AdABlAGMAdABvAHIA | base64 -d
```

Decoded Output:
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/c58c63ce-c150-49b8-b456-ae164d6a3971)


Answer: reallylongpassword


- **What name was used by the attacker?**


First, we need to understand there was team viewer connection to the victim, so most likely the question implies the name of the attacker that was used during the Team viewer communication with the victim

In Teamviewer log file, search for the keyword "participant"

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d6eab77a-d0af-4085-ae99-66ad09b5e6b6)


- **What IP address did the C2 connect back to?**

Search for the keyword "merlin.exe" in sysmon event 3

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/6b7d8a5e-902f-4bc9-a0e9-6250b3e7b0fa)

Answer: 52.56.142.81

- **What category did Windows Defender give to the C2 binary file?**

Search for the keyword "merlin.exe" and go to the details section

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/6443e09e-a7bf-4111-b70b-e5cd37efd553)

Answer: VirTool:Win32/Myrddin.D


- **What was the filename of the powershell script the attackers used to manipulate time?**

Search for keyword ".ps1" in sysmon with all events, eventually, we'll see the suspicious powershell script


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/86849ebb-e34f-4ccc-9dfe-0052ccdb1268)



Answer: Invoke-TimeWizard.ps1

- **What time did the initial access connection start?**


Initial access in this context mean when the attacker established a team viewer connection to the victim

Teamviewer Log:

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/f37508df-cc8a-4a6d-881c-bcf1d681452e)



Answer: 2023/05/04 11:35:27

- **What is the SHA1 and SHA2 sum of the malicious binary?**


Evidence: \Collection\C\ProgramData\Microsoft\Windows Defender\Support\MPLog-07102015-052145


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/06dfffdb-5dba-4005-a445-2b3cc657fbf1)


Answer: ac688f1ba6d4b23899750b86521331d7f7ccfb69:42ec59f760d8b6a50bbc7187829f62c3b6b8e1b841164e7185f497eb7f3b4db9


- **How many times did the powershell script change the time on the machine?**


Windows	4616	The system time was changed.

Open Windows Security Event log CSV that we got from the data prep phase in Timeline Explorer, then filter for event id 4616 and executable info that contain the powershell process (Invoke-TimeWizard.ps1)

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/3c8d24c2-7225-4a43-9014-e98154466d5a)




Answer: 2371

- **What is the SID of the victim user?**

Search for the keyword "merlin.exe" and go to the details section

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/589a5da2-647e-40c5-b1cb-d25e73c0ebc5)


Answer: S-1-5-21-3720869868-2926106253-3446724670-1003
