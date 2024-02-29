**Sherlock Scenario:**
You have been presented the opportunity to work as a junior DFIR consultant for a big consultancy, however they have provided a technical assessment for you to complete. The consultancy Forela-Security would like to gauge your knowledge on Windows Event Log Analysis. Please analyse and report back on the questions they have asked.
**Tools:** Eventviewer
**Evidence:** .evtx files



1. When did user cyberjunkie successfully log into his computer? (UTC)

Answer: 27/03/2023 14:37:09




2. The user tampered with firewall settings on the system. Analyze the firewall event logs to find out the Name of the firewall rule added?

Browse for Windows Firewall.evtx

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/9883b003-25f1-457b-b59f-41f7b8356f12)


Answer: Metasploit C2 Bypass

3. Whats the direction of the firewall rule?

Answer: Outbound



4. The user changed audit policy of the computer. Whats the Subcategory of this changed policy?

Windows	4719	System audit policy was changed
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/997ff2e1-587b-4c60-bf5b-0dde4f18d2d8)


Answer: Other Object Access Events

5. The user "cyberjunkie" created a scheduled task. Whats the name of this task?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/bf59a09b-53e3-4a92-8e81-2f7607504567)


Answer: HTB-AUTOMATION



6. Whats the full path of the file which was scheduled for the task?


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/2a1a9a90-169e-4329-8a92-767b62fdf2af)


Answer: C:\Users\CyberJunkie\Desktop\Automation-HTB.ps1

7. What are the arguments of the command?
Based on previous question's screenshot

Answer: -A cyberjunkie@hackthebox.eu


8. The antivirus running on the system identified a threat and performed actions on it. Which tool was identified as malware by antivirus?


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d5203e9b-6788-4fad-b2de-66529b1d60d4)



Answer: Sharphound

9. Whats the full path of the malware which raised the alert?
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/74b7ccd0-a7b1-4106-ab07-29f40983899f)


Answer: C:\Users\CyberJunkie\Downloads\SharpHound-v1.1.0.zip


10. What action was taken by the antivirus?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/f746c1ab-26ea-4718-a0cf-00cf55389eb2)




Answer: Qurantine



11. The user used Powershell to execute commands. What command was executed by the user?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/197511a8-a53d-4e5f-91b8-c9ae7a5218e0)




12. We suspect the user deleted some event logs. Which Event log file was cleared?
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/2295ec67-2d71-4abd-be92-3e233978ec48)


