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

![[Pasted image 20240203200312.png]]

Answer: 3.6.165.8:443

- **Now that you are aware of the IP address and port number, what is the JA3 fingerprint of the C2 server ?**

Wireshark Filter:

```
ip.addr == 3.6.165.8 && ssl.handshake.type == 1
```


![[Pasted image 20240203201243.png]]

Answer: 19e29534fd49dd27d09234e639c4057e



- **What is the name of the C2 framework being utilized by the red team ?**

Given:  sysmon evtx file in DC folder

Filter for sysmon event 11 (FileCreate) and then look for suspicious relation between image and target image. In this case, we can see that some one ran explorer.exe to download sliver.lnk file

![[Pasted image 20240204121335.png]]


Answer: Silver

- **Which WIN32 API provided the red team with the current directory information ?**


Given: Employee.apmx64
Tool: API Monitor

![[Pasted image 20240204092743.png]]
- **Now that we have identified the C2 framework utilized by the red team, which C2 command is responsible for opening notepad.exe by default and loading the .NET CLR into it ?**

Reference:
https://dominicbreuker.com/post/learning_sliver_c2_11_spawndll/

![[Pasted image 20240204135651.png]]

Answer:  execute-assembly

- **What is the name of the module (DLL) that was loaded to gain access to Windows Vault ?**

![[Pasted image 20240204124323.png]]

Answer: vaultcli.dll

- **After loading the mentioned module, there were a series of WIN32 APIs loaded. Which specific Win32 API is responsible for enumerating vaults ?**

![[Pasted image 20240204124843.png]]

Answer: VaultEnumerateVaults


- **Which command did the attacker execute to identify domain admins ?**

![[Pasted image 20240204100051.png]]

Answer: net group "domain admins" /dom


- **The red team has provided us with a hint that they utilized one of the tools from "ARMORY" for lateral movement to DC01. What is the name of the tool ?**

Reference: 
https://github.com/sliverarmory/armory/blob/master/armory.json

![[Pasted image 20240204122434.png]]



Alternatively:

![[Pasted image 20240204122831.png]]

Answer: Sharpwmi

- **Which command was executed by the red team to extract/dump the contents of NTDS.DIT ?**

![[Pasted image 20240204114140.png]]

Answer: cmd /c ntdsutil "ac in ntds" ifm "cr fu %TEMP%\H00i0Z000.dat" q q

- **The red team has obtained the aforementioned dump by compressing it into a ZIP file. Which specific Win32 API is responsible for retrieving the full path of the file to be downloaded?**

![[Pasted image 20240204112706.png]]


Answer: GetFullPathNameW
