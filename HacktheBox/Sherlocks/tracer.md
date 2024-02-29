**Sherlock Scenario:**
A junior SOC analyst on duty has reported multiple alerts indicating the presence of PsExec on a workstation. They verified the alerts and escalated the alerts to tier II. As an Incident responder you triaged the endpoint for artefacts of interest. Now please answer the questions regarding this security event so you can report it to your incident manager.


**Data Preparation:**


Tool Path: C:\Users\johnn\OneDrive\Desktop\Cyber Certificates\Hack the Box\Tools\Get-ZimmermanTools\net6

**Tool: MFTECMD.exe**



Creating USN Journal output CSV
```
.\MFTECmd.exe -f 'C:\Users\johnn\Downloads\Sherlock Practice\tracer\Tracer\C\$Extend\$J' --csv C:\Users\johnn\Downloads\SherlockPractice\tracer\ --csvf MFT-J.csv
```



**Tool: EvtxECmd.exe**
Note: cd to \EvtxeCmd path first the run the bellow command

Creating kape_event_log.csv
```
.\EvtxECmd.exe -f 'C:\Users\johnn\Downloads\SherlockPractice\tracer\Tracer\C\Windows\System32\winevt\logs\Microsoft-Windows-Sysmon%4Operational.evtx' --csv C:\Users\johnn\Downloads\SherlockPractice\tracer\ --csvf kape_event_log.csv
```



**Tool: PECmd.exe**

Creating prefetech_analysis.csv
```
./PECmd.exe -d "C:\Users\johnn\Downloads\SherlockPractice\tracer\Tracer\C\Windows\prefetch" --csv C:\Users\johnn\Downloads\SherlockPractice\tracer\ --csvf Prefetch_analysis.csv
```








**Exercise**



1. The SOC Team suspects that an adversary is lurking in their environment and are using PsExec to move laterally. A junior SOC Analyst specifically reported the usage of PsExec on a WorkStation. How many times was PsExec executed by the attacker on the system?


Import Prefetch_analysis.csv to Timeline Explorer

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/e8fded21-1f44-4d94-9e1f-86e75edab67d)


Alternatively, we can count it manually in EventViewer by using System.evtx - Event ID 7045

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/c38d439d-7d5c-4981-8a9b-48d33e1f8dc1)



Answer: 9



2. What is the name of the **service binary **dropped by PsExec tool allowing attacker to execute remote commands?


**Given:**
System.evtx - Event ID 7045.


Whenever a service is created on a Windows system, an event is recorded in Windows-System event logs with the Event ID 7045.

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/0c9cc8ee-687c-456c-8f6d-566c1b682c41)


Answer: psexesvc.exe


3. Now we have confirmed that PsExec ran multiple times, we are particularly interested in the 5th Last instance of the PsExec. What is the timestamp when the PsExec Service binary ran?


in EventViewer by using System.evtx - Event ID 7045

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/afb67593-1cda-4b5a-935d-50c16cbcef0e)



Answer: 07/09/2023 12:06:54 

4. Can you confirm the hostname of the workstation from which attacker moved laterally?

```
Evidence: Tracer\C\Windows\System32\winevt\logs\Microsoft-Windows-Sysmon%4Operational
```


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d915ef1c-b943-483f-a971-d7b69468e055)



Answer: Forela-Wkstn001 





5. What is full name of the Key File dropped by 5th last instance of the Psexec?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/64f89978-e9c5-41eb-8d90-5f822fe10b4d)



Answer: PSEXEC-FORELA-WKSTN001-95F03CFE.key






6. Can you confirm the timestamp when this key file was created on disk?

Using the same output as previous question (Key File dropped by 5th last instance of the Psexec)

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/4ec29b38-aaf6-4cf7-855b-2bd925e46a1c)


Answer: 07/09/2023 12:06:55



7. What is the full name of the Named Pipe ending with the "stderr" keyword for the 5th last instance of the PsExec?


We solved this by filtering for Event 17 (Pipe Created) and compared the time with the previous question

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/df076d23-a30c-42b6-a0e6-9b00a7d36a74)
