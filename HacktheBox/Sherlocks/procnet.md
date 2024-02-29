**Sherlock Scenario:**
With the rising utilization of open-source C2 frameworks by threat actors, our red team has simulated the functionalities of one such widely employed framework. The objective of this exercise is to aid blue teams in strengthening their defenses against these specific threats. We have been provided with PCAP files and APIs collected during the event, which will serve as valuable resources. Let us now initiate the hunting process. Using the API Monitor: We are well-acquainted with opening PCAP and .EVTX files, but what are .apmx64 ? The .apmx64 file extension is associated with API Monitor, a software used to monitor and control API calls made by applications and services. To commence your analysis, follow the steps provided below: Download the API Monitor Navigate to "Files" and click on "Open" to view captured data from the file: "Employee.apmx64" or "DC01.apmx64" After opening the file, the "Monitoring Process" window will populate with a list of processes. Expand the view by clicking the '+' symbol to reveal the modules and threads associated with each process. The API calls can be observed in the "Summary" window. To focus our analysis on a specific module, click on the different DLLs loaded by the processes. TIP: When conducting analysis, it is advisable to begin by examining the API calls made by the process itself, rather than focusing solely on DLLs. For instance, if I intend to analyze the API calls of a process named csgo.exe, I will initially expand the view by clicking the '+' symbol. Then, I will narrow down my analysis specifically to 'csgo.exe' by selecting it, and I can further analyze other DLLs as needed.

**Given Evidence:**
- evtx files
- pcap files
- ampx64 files

**Tools:**
- Eventviewer
- Wireshark (Or zeek)
- APIMonitor






**Questions**


- **To which IP address and port number is the malware attempting to establish a connection ?**

Search for event id 3 for the given sysmon file in employee directory, then search for "csgo.exe"

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/89a0249e-7d0a-43bd-bf80-8b30ddbbb2ac)


Answer: 3.6.165.8:443

- **Now that you are aware of the IP address and port number, what is the JA3 fingerprint of the C2 server ?**

Wireshark Filter:

```
ip.addr == 3.6.165.8 && ssl.handshake.type == 1
```


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/2f40ce98-5c5c-472e-867c-440c87d894d0)


Answer: 19e29534fd49dd27d09234e639c4057e



- **What is the name of the C2 framework being utilized by the red team ?**

Given:  sysmon evtx file in DC folder

Filter for sysmon event 11 (FileCreate) and then look for suspicious relation between image and target image. In this case, we can see that some one ran explorer.exe to download sliver.lnk file

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/32fc7819-59ea-4b93-8d84-17c1a92e5f1d)


Answer: Silver

- **Which WIN32 API provided the red team with the current directory information ?**


Given: Employee.apmx64
Tool: API Monitor

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/edd19ed2-9b45-4c90-8881-100dd7c2518f)

- **Now that we have identified the C2 framework utilized by the red team, which C2 command is responsible for opening notepad.exe by default and loading the .NET CLR into it ?**

Reference:
https://dominicbreuker.com/post/learning_sliver_c2_11_spawndll/

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/f09b163a-3673-48f1-a9ba-f0772ffdb6ad)


Answer:  execute-assembly

- **What is the name of the module (DLL) that was loaded to gain access to Windows Vault ?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/96fd9375-414c-4e5d-b87f-0157f908bec7)


Answer: vaultcli.dll

- **After loading the mentioned module, there were a series of WIN32 APIs loaded. Which specific Win32 API is responsible for enumerating vaults ?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/54662973-2202-4a02-8774-1404d0cc2e1a)


Answer: VaultEnumerateVaults


- **Which command did the attacker execute to identify domain admins ?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/6ec5a4be-c069-440c-8b5e-482d78d5b2b5)


Answer: net group "domain admins" /dom


- **The red team has provided us with a hint that they utilized one of the tools from "ARMORY" for lateral movement to DC01. What is the name of the tool ?**

Reference: 
https://github.com/sliverarmory/armory/blob/master/armory.json

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/8e8194b3-186c-41b9-bcdc-f3e76d883b9d)




Alternatively:

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d2a6a40f-59f3-47b1-9bda-9c2c0c360e1e)


Answer: Sharpwmi

- **Which command was executed by the red team to extract/dump the contents of NTDS.DIT ?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/966ee868-1613-4580-b5b2-145b36c09171)


Answer: cmd /c ntdsutil "ac in ntds" ifm "cr fu %TEMP%\H00i0Z000.dat" q q

- **The red team has obtained the aforementioned dump by compressing it into a ZIP file. Which specific Win32 API is responsible for retrieving the full path of the file to be downloaded?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/81446f30-751a-41be-be68-1b3a1263156e)



Answer: GetFullPathNameW
