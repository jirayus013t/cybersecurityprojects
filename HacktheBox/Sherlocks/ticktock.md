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
![[Pasted image 20240204154241.png]]


Alternatively, we can find the malicious downloaded file in Teamviewer log 

![[Pasted image 20240205130209.png]]


Answer: merlin.exe

- **What was the session id for in the initial access?**

**Given:**
**Teamviewer log file**: C:\Users\johnn\Downloads\SherlockPractice\ticktock\Collection\C\Users\gladys\AppData\Local\TeamViewer\Logs\TeamViewer15_Logfile

Open TeamViewer15_Logfile in Timeline explorer for ease of browsing

![[Pasted image 20240205120547.png]]


Answer: -2102926010




- **The attacker attempted to set a bitlocker password on the C: drive what was the password?**


Search for "merlin.exe" in the sysmon csv file in timeline explorer and we can see 4 instances of merlin.exe

![[Pasted image 20240205141148.png]]

![[Pasted image 20240205141230.png]]

Lets investigate all the ParentProcessID which include 4780,1992, and 5768 by searching these events the Timeline explorer filter

![[Pasted image 20240205141657.png]]

Wen ca see that there's powershell.exe running some type of encoded command. Let's copy the encoded command and paste to [Cipher Identifier (online tool) | Boxentriq](https://www.boxentriq.com/code-breaking/cipher-identifier)

![[Pasted image 20240205141821.png]]


It's Base64 encoding technique, to decode, we run this command:

```
echo JABTAGUAYwB1AHIAZQBTAHQAcgBpAG4AZwAgAD0AIABDAG8AbgB2AGUAcgB0AFQAbwAtAFMAZQBjAHUAcgBlAFMAdAByAGkAbgBnACAAIgByAGUAYQBsAGwAeQBsAG8AbgBnAHAAYQBzAHMAdwBvAHIAZAAiACAALQBBAHMAUABsAGEAaQBuAFQAZQB4AHQAIAAtAEYAbwByAGMAZQAKAEUAbgBhAGIAbABlAC0AQgBpAHQATABvAGMAawBlAHIAIAAtAE0AbwB1AG4AdABQAG8AaQBuAHQAIAAiAEMAOgAiACAALQBFAG4AYwByAHkAcAB0AGkAbwBuAE0AZQB0AGgAbwBkACAAQQBlAHMAMgA1ADYAIAAtAFUAcwBlAGQAUwBwAGEAYwBlAE8AbgBsAHkAIAAtAFAAaQBuACAAJABTAGUAYwB1AHIAZQBTAHQAcgBpAG4AZwAgAC0AVABQAE0AYQBuAGQAUABpAG4AUAByAG8AdABlAGMAdABvAHIA | base64 -d
```

Decoded Output:
![[Pasted image 20240205141956.png]]


Answer: reallylongpassword


- **What name was used by the attacker?**


First, we need to understand there was team viewer connection to the victim, so most likely the question implies the name of the attacker that was used during the Team viewer communication with the victim

In Teamviewer log file, search for the keyword "participant"


![[Pasted image 20240205124237.png]]

- **What IP address did the C2 connect back to?**

Search for the keyword "merlin.exe" in sysmon event 3

![[Pasted image 20240204155329.png]]

Answer: 52.56.142.81

- **What category did Windows Defender give to the C2 binary file?**

Search for the keyword "merlin.exe" and go to the details section

![[Pasted image 20240204154503.png]]

Answer: VirTool:Win32/Myrddin.D


- **What was the filename of the powershell script the attackers used to manipulate time?**

Search for keyword ".ps1" in sysmon with all events, eventually, we'll see the suspicious powershell script


![[Pasted image 20240204190241.png]]



Answer: Invoke-TimeWizard.ps1

- **What time did the initial access connection start?**


Initial access in this context mean when the attacker established a team viewer connection to the victim

Teamviewer Log:

![[Pasted image 20240205143455.png]]



Answer: 2023/05/04 11:35:27

- **What is the SHA1 and SHA2 sum of the malicious binary?**


Evidence: \Collection\C\ProgramData\Microsoft\Windows Defender\Support\MPLog-07102015-052145


![[Pasted image 20240205143737.png]]

Answer: ac688f1ba6d4b23899750b86521331d7f7ccfb69:42ec59f760d8b6a50bbc7187829f62c3b6b8e1b841164e7185f497eb7f3b4db9


- **How many times did the powershell script change the time on the machine?**


Windows	4616	The system time was changed.

Open Windows Security Event log CSV that we got from the data prep phase in Timeline Explorer, then filter for event id 4616 and executable info that contain the powershell process (Invoke-TimeWizard.ps1)

![[Pasted image 20240205145221.png]]




Answer: 2371

- **What is the SID of the victim user?**

Search for the keyword "merlin.exe" and go to the details section

![[Pasted image 20240204154712.png]]


Answer: S-1-5-21-3720869868-2926106253-3446724670-1003
